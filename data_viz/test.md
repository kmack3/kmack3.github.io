<script src="//code.jquery.com/jquery.js"></script>
<style>

.node {
  stroke: #fff;
  stroke-width: 1.5px;
}

.link {
  stroke: #999;
  stroke-opacity: .6;
}

/* Radar css file */ 
.radar-chart .level{stroke:grey;stroke-width:.5}.radar-chart .axis line{stroke:grey;stroke-width:1}.radar-chart .axis .legend{font-family:sans-serif;font-size:10px}.radar-chart .axis .legend.top{dy:1em}.radar-chart .axis .legend.left{text-anchor:start}.radar-chart .axis .legend.middle{text-anchor:middle}.radar-chart .axis .legend.right{text-anchor:end}.radar-chart .tooltip{font-family:sans-serif;font-size:13px;transition:opacity 200ms;opacity:0}.radar-chart .tooltip.visible{opacity:1}.radar-chart .area{stroke-width:2;fill-opacity:.5}.radar-chart.focus .area{fill-opacity:.1}.radar-chart.focus .area.focused{fill-opacity:.7}.radar-chart .circle{fill-opacity:.9}.radar-chart .area,.radar-chart .circle{transition:opacity 300ms,fill-opacity 200ms;opacity:1}.radar-chart .d3-enter,.radar-chart .d3-exit{opacity:0}


.radar-chart .area {
  fill-opacity: 0.7;
}
.radar-chart.focus .area {
  fill-opacity: 0.3;
}
.radar-chart.focus .area.focused {
  fill-opacity: 0.9;
}
.area.germany, .germany .circle {
  fill: #FFD700;
  stroke: none;
}
.area.argentina, .argentina .circle {
  fill: #ADD8E6;
  stroke: none;
}

</style>

HEYO trying this out!

Even though D3 has a [serious learning curve](https://medium.com/@enjalot/the-hitchhikers-guide-to-d3-js-a8552174733a), it is possible to make use of its nice interactive visualizations **without really knowing anything about JavaScript**.  As it turns out, it is really not that hard to get your own data in an interactive D3 graph!
This post will walk you through how to visualize a graph aka network in D3 with your own data.


For this post and for my website, I wanted to learn to visualize my own network data using D3.  I followed a lot of steps by other bloggers like [Andrew Mehrmann](http://dkmehrmann.github.io/blog/2016/05/01/d3.html) and [Eric Bickel](https://ehbick01.github.io/2017/05/09/embedding-d3-visuals-in-rmarkdown/) to get this to work.    Here's what the end result looks like: 

<div class="chart-container"></div>
<script>
RadarChart.draw(".chart-container", data);
</script>

<div id='d3div'></div>

You are viewing a network of jazz scales.  The nodes represent different musical scales.  The edges connect any two scales that share 6 common tones.  The nodes are colored by scale type: blue nodes are Major scales.  This is a generalization of the [Circle of Fifths](https://en.wikipedia.org/wiki/Circle_of_fifths)  You can click and drag the nodes around and if you hover over the nodes, the name of the scale appears.

## The pieces behind making this work

First, take a look at the code from [this visualization](http://bl.ocks.org/mbostock/4062045)  in the [D3 gallery](https://github.com/mbostock/d3/wiki/Gallery).  

Notice that there is also a `.json` file that contains characters from *Les Miserables*.  **JSON** is the standard format for input and output to D3.js.  So you will definitely need to have your graph data into the JSON format.   

## JSON format for network data

If you already have your data in the JSON format, you can skip ahead.  My graph data happened to be in a custom `.csv` format, so I ended up writing a python script to convert the `.csv` to a `.json` file.  
Know that there are helpful JSON packages in **Python** and **R** that might work with standard formats.  Currently [igraph in R](http://kateto.net/networks-r-igraph) does not have an export JSON function. but [Cytoscape](http://cytoscape.org) and [NetworkX](https://networkx.github.io), a popular network library in Python, do support JSON output.  You may have to do something similar to what I did and write your own whatever-to-JSON conversion script, in which case, I'll walk you through the format.  

This is what a minimal non-hierarchical graph in JSON format looks like:


This JSON file contains all the information about the graph's links and nodes.   You can Specify what the color the nodes should be via the `"group"` argument.  In the `"links"` element, which refers to the edges in the graph, it specifies how the nodes are connected (as directed *source/target* edges) Lastly, the `"value"` argument will determine the thickness of the edge.  Very simple!

Once your data is in the format like this, you will also need to create a folder called `/scripts/` in the root directory of your github.io repository and move your `.json` file there.

## JavaScript 

Now go to [Andrew Mehrmann's blog post](http://dkmehrmann.github.io/blog/2016/05/01/d3.html) and copy the JavaScript (which was derived from [Mike Bostock's original force-directed graph visualization](http://bl.ocks.org/mbostock/4062045) and modified for size autoscaling) and paste it directly into your markdown file.  Make sure you also copy the `style tag`.  Thanks, Andrew and Mike!  

It doesn't really matter where you put the javascript or the style tag.  
The location of the graph will be wherever you put the following `div tag` into your markdown file:



I don't know why you need so many of the parent-directory symbols but I couldn't get it to work with any fewer.





<script src="//d3js.org/d3.v3.min.js"></script>
<script src="./radar-chart.min.js"></script>
<script>

  var width = $("#d3div").width(),
      height = 400;

  var color = d3.scale.category20();

  var force = d3.layout.force()
      .charge(-62)
      .linkDistance(80)
      .size([width, height]);

  var svg = d3.select("#d3div").append("svg")
      .attr("width", width)
      .attr("height", height);

  d3.json("./jazz_scales_network_minCTs6.json", function(error, graph) {
    if (error) throw error;

    force
        .nodes(graph.nodes)
        .links(graph.links)
        .start();

    var link = svg.selectAll(".link")
        .data(graph.links)
      .enter().append("line")
        .attr("class", "link")
        .style("stroke-width", function(d) { return Math.sqrt(d.value); });

    var node = svg.selectAll(".node")
        .data(graph.nodes)
      .enter().append("circle")
        .attr("class", "node")
        .attr("r", 5)
        .style("fill", function(d) { return color(d.group); })
        .call(force.drag);

    node.append("title")
        .text(function(d) { return d.name; });

    force.on("tick", function() {
      link.attr("x1", function(d) { return d.source.x; })
          .attr("y1", function(d) { return d.source.y; })
          .attr("x2", function(d) { return d.target.x; })
          .attr("y2", function(d) { return d.target.y; });

      node.attr("cx", function(d) { return d.x; })
          .attr("cy", function(d) { return d.y; });
    });
  });
</script>


<script>
  var data = [
    {
      className: 'germany', // optional, can be used for styling
      axes: [
        {axis: "strength", value: 13, yOffset: 10},
        {axis: "intelligence", value: 6},
        {axis: "charisma", value: 5},  
        {axis: "dexterity", value: 9},  
        {axis: "luck", value: 2, xOffset: -20}
      ]
    },
    {
      className: 'argentina',
      axes: [
        {axis: "strength", value: 6},
        {axis: "intelligence", value: 7},
        {axis: "charisma", value: 10},  
        {axis: "dexterity", value: 13},  
        {axis: "luck", value: 9}
      ]
    }
  ];
  var chart = RadarChart.chart();
  var svg = d3.select('body').append('svg')
    .attr('width', 600)
    .attr('height', 800);

  // draw one
  svg.append('g').classed('focus', 1).datum(data).call(chart);

  // draw many radars
  var game = svg.selectAll('g.game').data(
    [
      data,
      data,
      data,
      data
    ]
  );
  game.enter().append('g').classed('game', 1);
  game
    .attr('transform', function(d, i) { return 'translate(150,600)'; })
    .call(chart);

// retrieve config
chart.config();
// all options with default values
chart.config({
  containerClass: 'radar-chart', // target with css, the default stylesheet targets .radar-chart
  w: 600,
  h: 600,
  factor: 0.95,
  factorLegend: 1,
  levels: 3,
  maxValue: 0,
  minValue: 0,
  radians: 2 * Math.PI,
  color: d3.scale.category10(), // pass a noop (function() {}) to decide color via css
  axisLine: true,
  axisText: true,
  circles: true,
  radius: 5,
  open: false,  // whether or not the last axis value should connect back to the first axis value
                // if true, consider modifying the chart opacity (see "Style with CSS" section above)
  axisJoin: function(d, i) {
    return d.className || i;
  },
  tooltipFormatValue: function(d) {
    return d;
  },
  tooltipFormatClass: function(d) {
    return d;
  },
  transitionDuration: 300
});

</script>

## Conclusions

That's it!  If you followed along, you should now have a working interactive D3.js plot on your website so you can visualize your own graph or network data.  Thanks for reading.  Send me an email at rtylermclaughlin@gmail.com if you have any questions! 

I'd like to thank the following bloggers for their helpful posts.  Without them I'd have struggled quite a bit longer: [Andrew Mehrmann](http://dkmehrmann.github.io/blog/2016/05/01/d3.html), [Eric Bickel](https://ehbick01.github.io/2017/05/09/embedding-d3-visuals-in-rmarkdown/), and [Tyler Clavelle](https://tclavelle.github.io/blog/blogdown_github/)