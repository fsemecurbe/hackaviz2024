---
title: Example carte
---



<svg id="my_dataviz" width="400" height="400"></svg>





```js
// The svg
var svg = d3.select("#my_dataviz"),
    width = +svg.attr("width"),
    height = +svg.attr("height");

var projection = d3.geoIdentity()
           .reflectY(true) 
           .fitSize([width,height],habillage_adminsitratif)

    svg.append("g")
        .selectAll("path")
        .data(habillage_adminsitratif.features)
        .enter().append("path")
            .attr("fill", "#69b3a2")
            .attr("d", d3.geoPath()
                .projection(projection)
            )
            .style("stroke", "#fff")

// create a tooltip
    const Tooltip = d3.select("#my_dataviz")
      .append("div")
      .attr("class", "tooltip")
      .style("opacity", 1)
      .style("background-color", "white")
      .style("border", "solid")
      .style("border-width", "2px")
      .style("border-radius", "5px")
      .style("padding", "5px")
    
  const mouseover = function(event, d) {
      Tooltip.style("opacity", 1)
    }
    var mousemove = function(event, d) {
      console.log(d.Lieu)
      Tooltip
        .html("toto")
        .style("left", (event.x)/2 + "px")
        .style("top", (event.y)/2 - 30 + "px")
    }
    var mouseleave = function(event, d) {
      Tooltip.style("opacity", 0)
    }


svg
      .selectAll("myCircles")
      .data(jo_horraire)
      .enter()
      .append("circle")
        .attr("cx", d => projection(localisation_lieux.features.filter(dd => dd.properties.Lieu==d.Lieu)[0].geometry.coordinates)[0])
        .attr("cy", d => projection(localisation_lieux.features.filter(dd => dd.properties.Lieu==d.Lieu)[0].geometry.coordinates)[1])
        .attr("r", d => d.capacité_h**0.5/10)
        .attr("stroke-width", 1)
        .attr("fill-opacity", .8)
        .attr("fill", "grey")
        .style("stroke", "white")
      .on("mouseover", mouseover)
      .on("mousemove", mousemove)
      .on("mouseleave", mouseleave)



    svg.selectAll("text")
    .data(libelle.features)
    .enter()
    .append("text")
    .text(d => d.properties.emplacement)
    .attr("x", d => projection(d.geometry.coordinates)[0])
    .attr("y", d => projection(d.geometry.coordinates)[1])
    .attr("text-anchor", "middle")
    .attr("font-size", "15px")
    .attr("fill", "black");


```




```js
const jo_horraire = jo
  .params({ horraire: Date.parse("2024-08-04T16:00Z") })
  .filter((d,$) => aq.op.equal( aq.op.parse_date(d.horraire), $.horraire))
  .groupby('Lieu')
  .rollup({
    capacité_h: d=> aq.op.sum(d.capacité_h)
  })
  .orderby(aq.desc('capacité_h'))
  .objects()
  
```

```js
const jo_horraire_ville = jo
  .params({ horraire: Date.parse("2024-07-27T10:00Z") })
  .filter((d,$) => aq.op.equal( aq.op.parse_date(d.horraire), $.horraire))
  .groupby('ville')
  .rollup({
    capacité_h: d=> aq.op.sum(d.capacité_h)
  })
  .objects()
```





```js
const jo_csv = await FileAttachment("data/jo_horraire.csv").csv({typed: true});
const jo = aq.from(jo_csv)
const localisation_lieux = await FileAttachment("data/localisation_lieux.geojson").json()
const habillage_adminsitratif = FileAttachment("data/habillage_adminsitratif_fr.geojson").json()
const libelle = FileAttachment("data/libelle_administratif_fr.geojson").json()
```


