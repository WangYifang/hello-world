/* eslint-disable no-console */
<template>
    <div>
        <h2>{{ msg }}</h2>
        <div id="barchart">
        </div>
    </div>
</template>

<script>
import * as d3 from 'd3'
export default {
    name: "BarChart",
    props: ["data"],
    data(){
        return {
            msg: "👌 from barChart.vue"
        };
    },
    created() {
    },
    mounted(){
        this.drawBarChart();
    },
    watch: {
        data: function(){
            console.log("In watch!");
            this.drawBarChart();
        }
    },
    methods: {
        drawBarChart() {
            if(this.data.length == 0){
                console.log("In barChart.vue - drawBarChart(): faill to load data.\n");
            }
            else{
            const margin = {top: 20, right: 20, bottom: 30, left: 50};
            const width = 960 - margin.left - margin.right;
            const height = 500 - margin.top - margin.bottom;
            // set the ranges
            const tooltip = d3.select("body").append("div").attr("class", "toolTip");
            const colours = d3.scaleOrdinal()
                .range(["#6F257F", "#CA0D59"]);
                
            const x = d3.scaleBand().rangeRound([0, width]).padding(0.1);
            const y = d3.scaleLinear().rangeRound([height, 0]);

            d3.select("#barchart").select("svg").remove();

            // debug
            console.log("---------> test!");
            console.log("this.data: ", this.data);
            
            const svg = d3.select("#barchart").append("svg")
                .attr("width", width + margin.left + margin.right)
                .attr("height", height + margin.top + margin.bottom);
            const g = svg.append("g")
                .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

            // scale the range of the data
            x.domain(this.data.map(function(d) { return d.area; }));
            y.domain([0, d3.max(this.data, function(d) { return d.value; })]);

            g.append("g")
                .attr("id", "g_test")
                .attr("class", "axis axis--x")
                .attr("transform", "translate(0," + height + ")")
                .call(d3.axisBottom(x));
            g.append("g")
                .attr("class", "axis axis--y")
                .call(d3.axisLeft(y).ticks(5).tickFormat(function(d) { return parseInt(d / 1000) + "K"; }).tickSizeInner([-width]))
            .append("text")
                .attr("transform", "rotate(-90)")
                .attr("y", 6)
                .attr("dy", "0.71em")
                .attr("text-anchor", "end")
                .attr("fill", "#5D6971")
                .text("Average House Price - (£)");
            g.selectAll(".bar")
                .data(this.data)
            .enter().append("rect")
                .attr("id", d=>{
                    return "rect_" + d.area;
                })
                .attr("x", function(d) { 
                    return x(d.area); 
                })
                .attr("y", function(d) { return y(d.value); })
                .attr("width", x.bandwidth())
                .attr("height", function(d) { return height - y(d.value); })
                .attr("fill", function(d) { return colours(d.area); })
                .on("mousemove", function(d){
                    tooltip
                    .style("left", d3.event.pageX - 50 + "px")
                    .style("top", d3.event.pageY - 70 + "px")
                    .style("display", "inline-block")
                    .html((d.area) + "<br>" + "£" + (d.value));
                })
                .on("mouseout", function(d){ 
                    tooltip.style("display", "none");
                });          
            }
        }
    },
    computed: {
    }
}
</script>

<style>
body {
  background-color: #F1F3F3    
}
.axis {
	font: 15px sans-serif;
}
.axis path,
.axis line {
  fill: none;
  stroke: #D4D8DA;
  stroke-width: 1px;
  shape-rendering: crispEdges;
}
.toolTip {
  position: absolute;
  display: none;
  min-width: 80px;
  height: auto;
  background: none repeat scroll 0 0 #ffffff;
  border: 1px solid #6F257F;
  padding: 14px;
  text-align: center;
}
</style>