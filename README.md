# Madrid Atlas TopoJSON

This repository provides a simple script to generate TopoJSON files from the Madrid City Council's [Statistics portal](http://www.madrid.es/portales/munimadrid/es/Inicio/El-Ayuntamiento/Estadistica/Areas-de-informacion-estadistica/Territorio-climatologia-y-medio-ambiente/Territorio/Cartografia?vgnextfmt=default&vgnextoid=aa9309789246c210VgnVCM2000000c205a0aRCRD&vgnextchannel=e59b40ebd232a210VgnVCM1000000b205a0aRCRD) vector data.

## Usage
In a browser (using [d3-geo](https://github.com/d3/d3-geo) and Canvas):

```html
<!DOCTYPE html>
<canvas width="960" height="500"></canvas>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/topojson.v2.min.js"></script>
<script>

var context = d3.select("canvas").node().getContext("2d"),
  projection = d3.geoMercator(),
  path = d3.geoPath(projection, context);

d3.json("https://martingonzalez.net/mad-census-tracts.v1.json", function(error, mad) {
  if (error) throw error;

  context.beginPath();
  path(topojson.mesh(mad));
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
  mad = require("./node_modules/mad-atlas/mad/census_tracts.json");

var canvas = new Canvas(960, 500),
  context = canvas.getContext("2d"),
  projection = d3.geoMercator(),
  path = d3.geoPath(projection, context);

context.beginPath();
path(topojson.mesh(mad));
context.stroke();

canvas.pngStream().pipe(fs.createWriteStream("preview.png"));
```
## Generating the files
Clone or download the repo, start a terminal and run `npm install` in the folder. This command will run the script and move the generated files to the `mad` folder.

If you need to make further adjustments (simplification, quantization) you can change the `prepublish` script and run `npm install` again. 

## File Reference
<a href="#mad/census_tracts.json" name="mad/census_tracts.json">#</a> <b>mad/census_tracts.json</b> [<>](https://martingonzalez.net/mad-census-tracts.v1.json "Source")

A TopoJSON which contains five objects: *census tracts*, *neighborhoods*, *districts* and *city*. Every tract, neighborhood and district has its corresponding identifier, so it's easy to get started. 

<a href="#mad/census_tracts.json_census_tracts" name="mad/census_tracts.json_census_tracts">#</a> *mad*.objects.<b>census_tracts</b>

...

<a href="#mad/census_tracts.json_neighborhoods" name="mad/census_tracts.json_neighborhoods">#</a> *mad*.objects.<b>neighborhoods</b>

...

<a href="#mad/census_tracts.json_districts" name="mad/census_tracts.json_districts">#</a> *mad*.objects.<b>districts</b>

...

<a href="#mad/census_tracts.json_city" name="mad/census_tracts.json_city">#</a> *mad*.objects.<b>city</b>

...


### Inspiration

The original idea and implementation comes from Mike Bostock’s [us-atlas](https://github.com/topojson/us-atlas) and [world-atlas](https://github.com/topojson/world-atlas). Check out [es-atlas](https://github.com/martgnz/es-atlas), a package which provides Spanish administrative regions' data with the same format.
