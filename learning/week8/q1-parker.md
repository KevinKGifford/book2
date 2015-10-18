# Q1


## What is the average rating of a particular category in a particular city? (filtered by # of reviews)

<div style="border:1px grey solid; padding:5px;">
    <div><h5>Category</h5>
        <input id="arg1" type="text" value="Restaurants"/>
    </div>
    <div><h5>City</h5>
        <input id="arg2" type="text" value="Pittsburgh"/>
    </div>
    <div><h5>Min # of Reviews</h5>
        <input id="arg3" type="text" value="200"/>
    </div>
    <div style="margin:20px;">
        <button id="viz">Vizualize</button>
    </div>
</div>

<div class="myviz" style= "width:100%; height:500px; border: 1px black solid; padding: 5px;">
Data is not loaded yet
</div>

{% script %}
items = 'not loaded yet'

$.get('http://bigdatahci2015.github.io/data/yelp/yelp_academic_dataset_business.5000.json.lines.txt')
    .success(function(data){
        var lines = data.trim().split('\n')

        // convert text lines to json arrays and save them in `items`
        items = _.map(lines, JSON.parse)

        console.log('number of items loaded:', items.length)
        $('.myviz').html('number of records load:' + items.length)
        console.log('first item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(arg1, arg2, arg3){    

    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect x="0"   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
		                 <text x="${d.width}+20" y="15">${d.label}</text> \
                    </g>'

    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d[1]*100
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return "City: " + d[0] + "  Avg Rating: " + d[1]
    }
	
    var filtered = _.filter(items, function(d){
      return _.includes(d.categories,arg1)
    })
    
    filtered = _.filter(filtered, function(d){
	   return (d.city == arg2)	
    })
    
    filtered = _.filter(filtered, function(d){
       return (parseInt(arg3) <= d.review_count)
    })
    
    groups = _.groupBy(filtered, function(d){
        return d.city
    })
    
    groups = _.mapValues(groups, function(d){
	    var sum = _.sum(_.pluck(d, 'stars'))
	    return _.ceil((sum / (d.length)), 2)
    })

    var pairs = _.pairs(groups)
    
    var viz = _.map(pairs, function(d, i){ 
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var arg1 = $('input#arg1').val()
    var arg2 = $('input#arg2').val()
    var arg3 = $('input#arg3').val()
    viz(arg1, arg2, arg3)
})  

{% endscript %}

# Authors

This UI is developed by
* [Full name](link to github account)
* [Full name](link to github account)
* [Full name](link to github account)
* [Full name](link to github account)
* [Full name](link to github account)
