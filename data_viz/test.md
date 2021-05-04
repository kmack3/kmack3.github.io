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
.area.bulbasaur, .bulbasaur .circle {
  fill: #FFD700;
  stroke: none;
}
.area.charmander, .charmander .circle {
  fill: #ADD8E6;
  stroke: none;
}


.center {
margin: auto;
width: 60%;
border: 5px solid #FFFF00;
padding: 10px;
}

</style>

HEYO trying this out! 1.2

<div class="chart-container"></div>
<script>
pokemon = [
]
</script>
<script>
RadarChart.draw(".chart-container", data);
</script>


<script src="//d3js.org/d3.v3.min.js"></script>
<script src="./radar-chart.js"></script>

<script>
  // fake data
  var pokemon = [
  {
    className: 'bulbasaur', // optional, can be used for styling
        axes: [
          {axis: "speed", value: 45},
          {axis: "defense", value: 49},
          {axis: "attack", value: 49},  
          {axis: "hp", value: 45},  
          {axis: "capture rate", value: 45},
          {axis: "sp_defense", value: 65}, 
          {axis: "sp_attack", value: 65}, 
        ]
    },
    {
      className: 'charmander',
      axes: [
        {axis: "speed", value: 65},
        {axis: "defense", value: 43},
        {axis: "attack", value: 52},  
        {axis: "hp", value: 39},  
        {axis: "capture rate", value: 45},
        {axis: "sp_defense", value: 60}, 
        {axis: "sp_attack", value: 50}, 
      ]
    },
    {
      className: 'pretty cool pokemon',
      axes: [
        {axis: "speed", value: 90},
        {axis: "defense", value: 100},
        {axis: "attack", value: 100},  
        {axis: "hp", value: 100},  
        {axis: "capture rate", value: 100},
        {axis: "sp_defense", value: 100}, 
        {axis: "sp_attack", value: 100}, 
      ]
    },
    {
      className: 'mythical awesomeness',
      axes: [
        {axis: "speed", value: 100},
        {axis: "defense", value: 100},
        {axis: "attack", value: 100},  
        {axis: "hp", value: 100},  
        {axis: "capture rate", value: 100},
        {axis: "sp_defense", value: 100}, 
        {axis: "sp_attack", value: 100}, 
      ]
    },
];

function getBetterPokemon(filter_axis, better_than) {
  var best_name = "";
  var best_val = better_than;
  for (i = 0; i < pokemon.length; i++) {
    one_poke = pokemon[i]; // an object, e.g., charmander
    for (j = 0; j < one_poke.axes.length; j++) {
      if (one_poke.axes[j].axis == filter_axis) {
        if (one_poke.axes[j].value > better_than) {
          best_name = one_poke.className;
          best_val = one_poke.axes[j].value;
          return best_name;
        }
      }
    }
  }
  return null;
}

</script>

<script>
  var data = [
    {
      className: 'bulbasaur', // optional, can be used for styling
      axes: [
        {axis: "speed", value: 45},
        {axis: "defense", value: 49},
        {axis: "attack", value: 49},  
        {axis: "hp", value: 45},  
        {axis: "capture rate", value: 45},
        {axis: "sp_defense", value: 65}, 
        {axis: "sp_attack", value: 65}, 
      ]
    },
    {
      className: 'charmander',
      axes: [
        {axis: "speed", value: 65},
        {axis: "defense", value: 43},
        {axis: "attack", value: 52},  
        {axis: "hp", value: 39},  
        {axis: "capture rate", value: 45},
        {axis: "sp_defense", value: 60}, 
        {axis: "sp_attack", value: 50}, 
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