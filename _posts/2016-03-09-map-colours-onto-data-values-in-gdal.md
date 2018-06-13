---
title: map colours onto data values in GDAL
author: Koen Hufkens
layout: post
permalink: /2016/03/09/map-colours-onto-data-values-in-gdal/
categories:
  - Software
tags:
  - GIS
  - image processing
  - research
  - software
---
This is a quick post originating from a discussion I had recently. Sometimes GIS data does not come with it's original colour map but only as raw numbers. These raw numbers (classes) are fine for calculations, but rather limit the way you visualize things. Here, I'll show how to map colours to the classes or ranges using the <a href="http://www.gdal.org/">Geospatial Data Abstraction Library (GDAL)</a>.

All you need is a list of classes which you want to map to particular colours. The format of this colour table is rather flexible and is described in full on the <a href="https://grass.osgeo.org/grass64/manuals/r.colors.html">GRASS r.colors page</a>. For this particular example I used the colours of the 0.5 km MODIS-based Global Land Cover Climatology map, which translates into a table with 16 classes (I attached the table at the end of the blog post). You can download the data form <a href="http://landcover.usgs.gov/global_climatology.php">the USGS website</a> if you want to try this example (warning: large file - 4GB unzipped).

&nbsp;

<img class="size-full wp-image-1199 aligncenter" src="/uploads/2016/03/global_veg.jpg" alt="global_veg" width="700" height="236" />
<pre class="lang:sh decode:true"># If the colour table is saved in colours.csv the following
# command links a proper colour table to a geotiff file
# without this information.
gdaldem color-relief input.tif colours.csv output.tif</pre>
The above command might map the colours to the classes but the map still remains rather static. If you want to create a Google Earth compatible file (mapped onto a 3D sphere), you can do so by translating the file format. The resulting KML file should open in Google Earth if you have a copy running.
<pre class="lang:default decode:true">gdal_translate -of KMLSUPEROVERLAY input.tif output.kml -co format=png
</pre>
The full colour table for the image as linked to above is shown below.
<pre class="lang:sh decode:true ">0 186 254 253
1 0 100 1
2 77 165 87
3 125 204 15
4 98 232 101
5 55 198 132
6 214 119 117
7 253 237 160
8 185 231 141
9 255 224 27
10 254 192 107
11 37 138 220
12 252 251 0
13 251 3 4
14 147 144 5
15 254 220 211
16 191 191 191</pre>
&nbsp;

&nbsp;