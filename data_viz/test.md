<script src="//code.jquery.com/jquery.js"></script>
<style>

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


<div class="chart-container"></div>
<script>
RadarChart.draw(".chart-container", data);
</script>


<script src="//d3js.org/d3.v3.min.js"></script>
<script src="./radar-chart.min.js"></script>

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
  // var game = svg.selectAll('g.game').data(
  //   [
  //     data,
  //     data,
  //     data,
  //     data
  //   ]
  // );
  // game.enter().append('g').classed('game', 1);
  // game
  //   .attr('transform', function(d, i) { return 'translate(150,600)'; })
  //   .call(chart);



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

<!-- ## Conclusions

That's it!  If you followed along, you should now have a working interactive D3.js plot on your website so you can visualize your own graph or network data.  Thanks for reading.  Send me an email at rtylermclaughlin@gmail.com if you have any questions! 

I'd like to thank the following bloggers for their helpful posts.  Without them I'd have struggled quite a bit longer: [Andrew Mehrmann](http://dkmehrmann.github.io/blog/2016/05/01/d3.html), [Eric Bickel](https://ehbick01.github.io/2017/05/09/embedding-d3-visuals-in-rmarkdown/), and [Tyler Clavelle](https://tclavelle.github.io/blog/blogdown_github/) -->