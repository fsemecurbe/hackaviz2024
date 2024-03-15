---
title: Example carte
---

```js
import * as echarts from "npm:echarts";
//import op from "npm:arquero";
```

```js
const myChart = echarts.init(display(html`<div style="width: 600px; height:400px;"></div>`));


var option = {
  dataset: [
    {
      dimensions: ['name', 'age', 'profession', 'score', 'date'],
      source: [
        ['Hannah Krause', 41, 'Engineer', 314, '2011-02-12'],
        ['Zhao Qian', 20, 'Teacher', 351, '2011-03-01'],
        ['Jasmin Krause ', 52, 'Musician', 287, '2011-02-14'],
        ['Li Lei', 37, 'Teacher', 219, '2011-02-18'],
        ['Karle Neumann', 25, 'Engineer', 253, '2011-04-02'],
        ['Adrian Groß', 19, 'Teacher', '-', '2011-01-16'],
        ['Mia Neumann', 71, 'Engineer', 165, '2011-03-19'],
        ['Böhm Fuchs', 36, 'Musician', 318, '2011-02-24'],
        ['Han Meimei', 67, 'Engineer', 366, '2011-03-12']
      ]
    },
    {
      transform: {
        type: 'sort',
        config: { dimension: 'score', order: 'desc' }
      }
    }
  ],
  xAxis: {
    type: 'category',
    axisLabel: { interval: 0, rotate: 30 }
  },
  yAxis: {},
  series: {
    type: 'bar',
    encode: { x: 'name', y: 'score' },
    datasetIndex: 1
  }
};

myChart.setOption(option);
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
```


