<div id="my_dataviz2"></div>

<style>
.circle:hover{
  stroke: black;
  stroke-width: 4px;
}
</style>


```js

// set width and height of svg
const width = 460
const height = 400

// The svg
const svg = d3.create("svg")
  .append("svg")
  .attr("width", width)
  .attr("height", height)

// Map and projection
const projection = d3.geoMercator()
    .center([4, 47])                // GPS of location to zoom on
    .scale(1020)                       // This is like the zoom
    .translate([ width/2, height/2 ])

// Create data for circles:
const markers = [
  {long: 9.083, lat: 42.149, name: "Corsica"}, // corsica
  {long: 7.26, lat: 43.71, name: "Nice"}, // nice
  {long: 2.349, lat: 48.864, name: "Paris"}, // Paris
  {long: -1.397, lat: 43.664, name: "Hossegor"}, // Hossegor
  {long: 3.075, lat: 50.640, name: "Lille"}, // Lille
  {long: -3.83, lat: 58, name: "Morlaix"}, // Morlaix
];

// Load external data and boot
d3.json("https://raw.githubusercontent.com/holtzy/D3-graph-gallery/master/DATA/world.geojson").then( function(data){

    // Filter data
    data.features = data.features.filter( d => d.properties.name=="France")

    // Draw the map
    svg.append("g")
        .selectAll("path")
        .data(data.features)
        .join("path")
          .attr("fill", "#b8b8b8")
          .attr("d", d3.geoPath()
              .projection(projection)
          )
        .style("stroke", "black")
        .style("opacity", .3)

    // create a tooltip
    const Tooltip = d3.select("#my_dataviz2")
      .append("div")
      .attr("class", "tooltip")
      .style("opacity", 1)
      .style("background-color", "white")
      .style("border", "solid")
      .style("border-width", "2px")
      .style("border-radius", "5px")
      .style("padding", "5px")

    // Three function that change the tooltip when user hover / move / leave a cell
    const mouseover = function(event, d) {
      Tooltip.style("opacity", 1)
    }
    var mousemove = function(event, d) {
      Tooltip
        .html(d.name + "<br>" + "long: " + d.long + "<br>" + "lat: " + d.lat)
        .style("left", (event.x) +3500 + "px")
        .style("top", (event.y)  + "px")
    }
    var mouseleave = function(event, d) {
      Tooltip.style("opacity", 0)
    }

    // Add circles:
    svg
      .selectAll("myCircles")
      .data(markers)
      .join("circle")
        .attr("cx", d => projection([d.long, d.lat])[0])
        .attr("cy", d => projection([d.long, d.lat])[1])
        .attr("r", 14)
        .attr("class", "circle")
        .style("fill", "69b3a2")
        .attr("stroke", "#69b3a2")
        .attr("stroke-width", 3)
        .attr("fill-opacity", .4)
      .on("mouseover", mouseover)
      .on("mousemove", mousemove)
      .on("mouseleave", mouseleave)


})
display(svg.node());
```