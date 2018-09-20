---
title: Using github as a tile server
teaser: "creating nice maps using Qgis, leaflet and github ..."
author: Koen Hufkens
layout: post
permalink: /2018/09/19/github-tile-server/
categories:
  - GIS
  - remote_sensing
  - maps
tags:
  - remote_sensing
  - science
  - maps
---

In much of my work I use geospatial data (either vector or raster maps). However, visualizing this data easily for people unfamiliar with geographic information system (GIS) toolsets is often difficult. 

However, most people unfamiliar with these kinds of data are often only interested in the result (a nice map) rather than learning how to handle a new visualization tool. In short, you need to present mapping data in an appealing way.

There are commercial to do so, most prominently mapbox. Mapbox does have a [free option](https://www.mapbox.com/pricing/), but this obviously only stretches so far. The other option I want to present here is using github as a fast and easy way to host map tiles and a micro-website, by using QGis and the Qtiles plugin.

The basic procedure would be to load your mapdata into [QGis](https://www.qgis.org/en/site/), install the [QTiles plugin](https://plugins.qgis.org/plugins/qtiles/), and run the plugin to build map tiles. Make sure to activate the "write leaflet-based viewer" option. This will generate a folder with a map tiles pyramid and an html file to render a leaflet map. You can specify the number of zoom levels included and the size and data format of the tiles. Most defaults work well, but I suggest to increase the file dimensions to reduce the number of tiles generated.

Once done you upload the data to github as you would with any other project and point the leaflet code to your project location (replace the bold text, and render settings, in the generated html file below to use your github project).

```
var mytile = L.tileLayer('https://github.com/YOUR_GITHUB_HANDLE/PROJECT_NAME/raw/master/TILES_FOLDER/{z}/{x}/{y}.png', {
        maxZoom: 8,
	minZoom: 5,
        tms: false
      }).addTo(map);
```

For one of my projects, mapping mountain forests in Europe, The line above would read:

```
var mytile =L.tileLayer('https://github.com/khufkens/mountain_forest_map/raw/master/tiles/{z}/{x}/{y}.png', {
        maxZoom: 8,
	minZoom: 5,
        tms: false
      }).addTo(map);
```

You can read through the complete [html file](https://github.com/khufkens/mountain_forest_map/blob/master/index.html) in my [github project repo](https://github.com/khufkens/mountain_forest_map). The generated map is shown below.

<style>
.legend {
	text-align: left;
	line-height: 18px;
	color: #555;
	padding: 6px 8px;
	font: 16px/18px Arial, Helvetica, sans-serif;
	background: rgba(255,255,255,0.8);
	box-shadow: 0 0 15px rgba(0,0,0,0.2);
	border-radius: 5px;
}

.legend h4 {
    margin: 0 0 5px;
	color: #777;
}

.legend i {
	width: 18px;
	height: 18px;
	float: left;
	margin-right: 8px;
	opacity: 0.7;
}

.legend .circle {
	border-radius: 50%;
	width: 10px;
	height: 10px;
	margin-top: 8px;
}

.info {
    padding: 6px 8px;
    font: 14px/16px Arial, Helvetica, sans-serif;
    background: white;
    background: rgba(255,255,255,0.8);
    box-shadow: 0 0 15px rgba(0,0,0,0.2);
    border-radius: 5px;
}
.info h4 {
    margin: 0 0 5px;
    color: #777;
}

img {
  border-radius: 0%;
}

</style>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.4/dist/leaflet.css">
<script src="https://unpkg.com/leaflet@1.3.4/dist/leaflet.js"></script>

<div id="map" style="width: 600px%; height: 600px; z-index:0;"></div>

<script type="text/javascript">
      var map = L.map('map').setView([46.529, 6.746], 5);
      var baselayer =  L.tileLayer('http://{s}.google.com/vt/lyrs=s&x={x}&y={y}&z={z}',{
    	maxZoom: 8,
    	minZoom: 5,
    	subdomains:['mt0']}).addTo(map);

      var mytile =L.tileLayer('https://github.com/khufkens/mountain_forest_map/raw/master/tiles/{z}/{x}/{y}.png', {
        maxZoom: 8,
        tms: false
      }).addTo(map);

L.control.layers({'Basemap':baselayer},{'Mountain Forest':mytile}).addTo(map);

function getColor(d) {
    return d == 5  ? '#5E3C99' :
                d == 4  ? '#B2ABD2' :
                d == 3  ? '#F7F7F7' :
                d == 2  ? '#FDB863' :
                d == 1  ? '#E66101' :
                                '#E66101' ;
}

var legend = L.control({position: 'bottomright'});

legend.onAdd = function (map) {
      var div = L.DomUtil.create('div', 'info legend'),
         grades = [1, 2, 3, 4, 5],
         labels = ['ENF','DNF','EBF','DBF','MIX'];

    for (var i = 0; i < grades.length; i++) {
        div.innerHTML +=
            '<i style="background:' + getColor(grades[i]) + '"></i> ' +
            labels[i] + '<br><br>';
    }
    return div;
};

legend.addTo(map);
</script>
	



