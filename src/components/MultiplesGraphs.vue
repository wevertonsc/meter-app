<template>
    <div id="graph" >
    </div>
</template>

<script>
import * as d3 from 'd3';
export default {

}

var margin = {top: 8, right: 10, bottom: 2, left: 10},
    width = 600 - margin.left - margin.right,
    height = 300 - margin.top - margin.bottom;

var parseDate = d3.time.format("%b %Y").parse;

var x = d3.time.scale()
    .range([0, width]);

var y = d3.scale.linear()
    .range([height, 0]);

var area = d3.svg.area()
    .x(function(d) { return x(d.date); })
    .y0(height)
    .y1(function(d) { return y(d.measure); });

var line = d3.svg.line()
    .x(function(d) { return x(d.date); })
    .y(function(d) { return y(d.measure); });

//d3.tsv("http://localhost:8085/api/v1/meter/measures", type, function(error, data) {
//d3.tsv("stocks.tsv", type, function(error, data) {
d3.tsv("https://server-monitoring-v1.herokuapp.com/api/v1/meter/measures", type, function(error, data) {
  var meters = d3.nest()
      .key(function(d) { return d.meter; })
      .entries(data);

  x.domain([
    d3.min(meters, function(meter) { return meter.values[0].date; }),
    d3.max(meters, function(meter) { return meter.values[meter.values.length - 1].date; })
  ]);

  var svg = d3.select("div").selectAll("svg")
      .data(meters)
    .enter().append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
    .append("g")
      .attr("transform", "translate(" + margin.left + "," + margin.top + ")")
      .each(multiple)

  svg.append("text")
      .attr("x", width - 6)
      .attr("y", height - 6)
      .style("text-anchor", "end")
      .text(function(d) { return d.key; });

});

function multiple(meter) {
  var svg = d3.select(this);

  y.domain([0, d3.max(meter.values, function(d) { return d.measure; })]);

  svg.append("path")
      .attr("class", "area")
      .attr("d", area(meter.values))
       .style("fill", "#e7e7e7");

  svg.append("path")
      .attr("class", "line")
      .attr("d", line(meter.values));
}

function type(d) {
  d.measure = +d.measure;
  d.date = parseDate(d.date);
  return d;
}

</script>

<style>
#graph {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-image:'3.jpg';
}
</style>

