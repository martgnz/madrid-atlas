# Madrid Atlas TopoJSON

This repository provides a simple script to generate TopoJSON files from the Madrid City Council's [Statistics portal](http://www.madrid.es/portales/munimadrid/es/Inicio/El-Ayuntamiento/Estadistica/Areas-de-informacion-estadistica/Territorio-climatologia-y-medio-ambiente/Territorio/Cartografia?vgnextfmt=default&vgnextoid=aa9309789246c210VgnVCM2000000c205a0aRCRD&vgnextchannel=e59b40ebd232a210VgnVCM1000000b205a0aRCRD) vector data.

## Usage
In a browser (using [d3-geo](https://github.com/d3/d3-geo) and Canvas):

```html
<!DOCTYPE html>
<canvas width="960" height="600"></canvas>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/topojson.v2.min.js"></script>
<script>

var context = d3.select("canvas").node().getContext("2d"),
    path = d3.geoPath().context(context);

d3.json("https://martingonzalez.net/madrid-tracts.v1.json", function(error, madrid) {
  if (error) throw error;

  context.beginPath();
  path(topojson.mesh(madrid));
  context.stroke();
});

</script>

```

In Node (using [d3-geo](https://github.com/d3/d3-geo) and [node-canvas](https://github.com/Automattic/node-canvas)):

```js
var fs = require("fs"),
  d3 = require("d3-geo"),
  topojson = require("topojson-client"),
  Canvas = require("canvas"),
  madrid = require("./node_modules/madrid-atlas/madrid/census_tracts.json");

var canvas = new Canvas(960, 600),
  context = canvas.getContext("2d"),
  path = d3.geoPath().context(context);

context.beginPath();
path(topojson.mesh(madrid));
context.stroke();

canvas.pngStream().pipe(fs.createWriteStream("preview.png"));
```
## Generating the files
Clone or download the repo, start a terminal and run `npm install` in the folder. This command will run the script and move the generated files to the `mad` folder.

If you need to make further adjustments (simplification, quantization) you can change the `prepublish` script and run `npm install` again. 

## File Reference
<a href="#madrid/census_tracts.json" name="madrid/census_tracts.json">#</a> <b>madrid/census_tracts.json</b> [<>](https://martingonzalez.net/madrid-tracts.v1.json "Source")

A TopoJSON which contains five objects: *census tracts*, *neighborhoods*, *districts* and *city*. Every tract, neighborhood and district has its corresponding identifier, so it's easy to get started. 

<a href="#madrid/census_tracts.json_census_tracts" name="madrid/census_tracts.json_census_tracts">#</a> *madrid*.objects.<b>census_tracts</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22272536/f742bd7a-e29a-11e6-8dd8-5f618b82acc5.png" width="480" height="auto">

<a href="#madrid/census_tracts.json_neighborhoods" name="madrid/census_tracts.json_neighborhoods">#</a> *madrid*.objects.<b>neighborhoods</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22272610/60be5084-e29b-11e6-9cca-dc8ee094d9eb.png" width="480" height="auto">

<a href="#madrid/census_tracts.json_districts" name="madrid/census_tracts.json_districts">#</a> *madrid*.objects.<b>districts</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22272630/7df7c144-e29b-11e6-9c21-12be27d03035.png" width="480" height="auto">

<a href="#madrid/census_tracts.json_city" name="madrid/census_tracts.json_city">#</a> *madrid*.objects.<b>city</b>

<img src="https://cloud.githubusercontent.com/assets/1236790/22272646/99ba2caa-e29b-11e6-8ec0-d30f176875c7.png" width="480" height="auto">


### Inspiration

The original idea and implementation comes from Mike Bostock’s [us-atlas](https://github.com/topojson/us-atlas) and [world-atlas](https://github.com/topojson/world-atlas).

Check out [es-atlas](https://github.com/martgnz/es-atlas) and [barcelona-atlas](https://github.com/martgnz/barcelona-atlas), packages which provide other Spanish administrative regions data with the same format.
