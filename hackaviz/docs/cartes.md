---
title: Example carte
---



<svg id="my_dataviz" width="400" height="400"></svg>

```js
habillage_adminsitratif
```

```js
// The svg
var svg = d3.select("svg"),
    width = +svg.attr("width"),
    height = +svg.attr("height");

var projection = d3.geoIdentity()
           .reflectY(true) 
           .fitSize([width,height],habillage_adminsitratif)
// Map and projection
//var projection = d3.geoIdentity()
//.translate([-650000.3415267066,-6850000.695430356])

	//geoNaturalEarth1()
  //  .scale(width / 1.3 / Math.PI)
  //  .translate([width / 2, height / 2])

// Load external data and boot

    // Draw the map
    svg.append("g")
        .selectAll("path")
        .data(habillage_adminsitratif.features)
        .enter().append("path")
            .attr("fill", "#69b3a2")
            .attr("d", d3.geoPath()
                .projection(projection)
            )
            .style("stroke", "#fff")


```



```js
const jo_horraire = jo
  .params({ horraire: Date.parse("2024-07-27T10:00Z") })
  .filter((d,$) => aq.op.equal( aq.op.parse_date(d.horraire), $.horraire))
  .groupby('Lieu')
  .rollup({
    capacité_h: d=> aq.op.sum(d.capacité_h)
  })
   
  
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
```


