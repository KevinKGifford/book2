# Warmup

## What is the distribution of X in Y order?

<div style="border:1px grey solid; padding:5px;">
    <div><h5>X (e.g., stars)</h5>
        <input id="arg1" type="text" value="stars"/>
    </div>
    <div><h5>Y (e.g., ascending, descending)</h5>
        <input id="arg2" type="text" value="ascending"/>
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

        console.log('1st item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(arg1, sortDirection){    

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})">      \
                    <text y="20">${d.label}</text>            \
                    <rect x="30"                              \
                         width="${d.width}"                   \
                         height="20"                          \
                         style="fill:${d.color};              \
                                stroke-width:3;               \
                                stroke:rgb(0,0,0)" />         \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return d[1].length / 2
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    // Group items based on the attribute specified by users

    var groups = _.groupBy(items, arg1)
    console.log('groups:', groups)

    console.log('sortDirection', sortDirection)
    var pairs = _.sortBy(_.pairs(groups), function(d) {
        return sortDirection * d[1].length
    })

    var viz = _.map(pairs, function(d, i){                
            return {
                x: computeX(d, i),
                y: computeY(d, i),
                width: computeWidth(d, i),
                color: computeColor(d, i),
                label: d[0]
            }
         })

    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var arg1 = $('input#arg1').val()
    var arg2 = $('input#arg2').val()
    console.log('arg2', arg2)
    var sort = 1
    if (arg2 == 'Descending' || arg2 == 'descending') {sort = -1}
    viz(arg1, sort)
})  

{% endscript %}
