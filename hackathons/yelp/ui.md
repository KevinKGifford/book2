# UI

## What businesses have category (W) in city (X) with at least (Y) reviews and at least a rating of (Z)?

<div style="border:1px grey solid; padding:5px;">
    <div><h5>Category</h5>
        <input id="arg0" type="text" value="Mexican"/>
    </div>
    <div><h5>City</h5>
        <input id="arg1" type="text" value="Phoenix"/>
    </div>
    <div><h5>Minimum Number of Reviews</h5>
        <input id="arg2" type="text" value="200"/>
    </div>
    <div><h5>Minimum Rating (0.0 - 5.0)</h5>
        <input id="arg3" type="text" value="4.0"/>
    </div>    
    <div style="margin:20px;">
        <button id="viz">Vizualize</button>
    </div>
</div>

<div class="myviz" style="width:100%; height:500px; border: 1px black solid; padding: 5px;">
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

        // Show in the myviz that the data is loaded
        $('.myviz').html('number of records load:' + data.length)

        console.log('first item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(arg0, arg1, arg2, arg3) {    

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <text y="15">${d.label}</text> \
                    <rect x="400"   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text x=405 y="15">${d.label2}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return d.stars * 50
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d.name + ' (reviews: ' + d.review_count + ')'
    }

    function computeLabel2(d, i) {
        return ' Rating: ' + d.stars
    }

    // UI processing logic

    console.log(arg0); console.log(arg1); console.log(arg2); console.log(arg3);


    var filter_city = _.filter(items, function(d) {
        return (d.city == arg1)
    })
    var filter_reviews = _.filter(filter_city, function(d) {
        return (d.review_count >= parseInt(arg2))
    })
    var filter_stars = _.filter(filter_reviews, function(d) {
        return (d.stars >= parseFloat(arg3))
    })

    // Have filtered 'items' based on city, #reviews and rating (stars)
    // Find all objects with a category element matching user input 'arg0'

    console.log('first filter_stars', filter_stars[0])

    var filter_categories = _.filter(filter_stars, function(f) {
        return _.some(f.categories, function(d) {
            return d == arg0
        })
    })

    // Take the first 20 items to visualize    
    items = _.take(filter_categories, 20)

    var viz = _.map(items, function(d, i) {                
        return {
            x: computeX(d, i),
            y: computeY(d, i),
            width: computeWidth(d, i),
            color: computeColor(d, i),
            label: computeLabel(d, i),
            label2: computeLabel2(d, i)
        }
    })

    var result = _.map(viz, function(d){
        // invoke the compiled template function on each viz data
        return template({d: d})
    })

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var arg0 = $('input#arg0').val()
    var arg1 = $('input#arg1').val()
    var arg2 = $('input#arg2').val()
    var arg3 = $('input#arg3').val()
    viz(arg0, arg1, arg2, arg3)
})  

{% endscript %}

# Authors

This UI is developed by
* [William Farmer](http://github.com/willzfarmer)
* [Kevin Gifford](http://github.com/kevinkgifford)
* [Parker Illig](http://github.com/pail4944)
* [Robert Kendl](http://github.com/DomoYeti)
* [Andrew Krodinger](http://github.com/drewdinger)
* [John Raesly](http://github.com/jraesly)


