---
title: R arctic polar plots
author: Koen Hufkens
layout: post
permalink: /2017/01/18/r-polar-plots/
categories:
  - R
  - Research
  - Science
  - Software
tags:
  - R
  - research
  - science
  - software
---
For a project I needed to create an appealing plot of the arctic, showing the location of some field sites. I've posted this map on twitter earlier today. Below, I'll outline a simple routine to recreate this plot, and if need be adjust it to your liking.
<blockquote class="twitter-tweet" data-lang="en">
<p dir="ltr" lang="en">Fancy R graphics for an upcoming paper. Thanks to some sp() juggling. <a href="https://t.co/aTLvBbPStU">pic.twitter.com/aTLvBbPStU</a></p>
— Koen Hufkens (@koen_hufkens) <a href="https://twitter.com/koen_hufkens/status/821739274404057091">January 18, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

First of all I downloaded an appealing background from the Blue Marble dataset as created by NASA. Geotiffs can be downloaded <a href="https://neo.sci.gsfc.nasa.gov/view.php?datasetId=BlueMarbleNG-TB">here</a> or by direct download <a href="http://neo.sci.gsfc.nasa.gov/servlet/RenderData?si=526312&amp;cs=rgb&amp;format=TIFF&amp;width=5400&amp;height=2700">following this link</a>. Alternatively you can download the less realistic and more summary style graphics as produced by <a href="http://www.naturalearthdata.com/">Natural Earth</a>.

After downloading the Blue Marble geotiff you trim the data to the lowest latitude you want to plot. Subsequently, I reproject the data to the EPSG 3995 projection (or arctic polar stereographic). All this is done using <a href="http://www.gdal.org/">GDAL</a>. This step could be done using rgdal, but at times this doesn't play nice. For now I post the command line GDAL code.

UPDATE: the below command line gdal code is not necessary anymore as I call the raster library in R now which works fine in dealing with the reprojection after some fiddling.
<pre class="lang:sh decode:true">gdalwarp -te -180 55 180 90 world.topo.bathy.200407.3x5400x2700.tif tmp.tif
gdalwarp -wo SOURCE_EXTRA=200 -s_srs EPSG:4326 -t_srs EPSG:3995 -dstnodata "255 255 255" tmp.tif blue_marble.tif</pre>
The remaining R code ingests this background image and overlays a graticule and some labels. For this I heavily borrowed from the <a href="https://edzer.github.io/sp/">sp map gallery</a>.
<pre class="lang:r decode:true crayon-selected"># load required libraries
library(sp)
library(maps)
library(rgeos)

# function to slice and dice a map and convert it to an sp() object
maps2sp = function(xlim, ylim, l.out = 100, clip = TRUE) {
  stopifnot(require(maps))
  m = map(xlim = xlim, ylim = ylim, plot = FALSE, fill = TRUE)
  p = rbind(cbind(xlim[1], seq(ylim[1],ylim[2],length.out = l.out)),
            cbind(seq(xlim[1],xlim[2],length.out = l.out),ylim[2]),
            cbind(xlim[2],seq(ylim[2],ylim[1],length.out = l.out)),
            cbind(seq(xlim[2],xlim[1],length.out = l.out),ylim[1]))
  LL = CRS("+init=epsg:4326")
  IDs = sapply(strsplit(m$names, ":"), function(x) x[1])
  stopifnot(require(maptools))
  m = map2SpatialPolygons(m, IDs=IDs, proj4string = LL)
  bb = SpatialPolygons(list(Polygons(list(Polygon(list(p))),"bb")), proj4string = LL)
  
  if (!clip)
    m
  else {
    stopifnot(require(rgeos))
    gIntersection(m, bb)
  }
}

# set colours for map grid
grid.col.light = rgb(0.5,0.5,0.5,0.8)
grid.col.dark = rgb(0.5,0.5,0.5)

# coordinate systems
polar = CRS("+init=epsg:3995")
longlat = CRS("+init=epsg:4326")

# download the blue marble data if it doesn't
# exist
if (!file.exists("blue_marble.tif")) {
download.file("http://neo.sci.gsfc.nasa.gov/servlet/RenderData?si=526312&amp;cs=rgb&amp;format=TIFF&amp;width=5400&amp;height=2700","blue_marble.tif")
}

# read in the raster map and
# set the extent, crop to extent and reproject to polar
r = raster::brick("blue_marble.tif")
e = raster::extent(c(-180,180,55,90))
r_crop = raster::crop(r,e)

# traps NA values and sets them to 1
r_crop[is.na(r_crop)] = 1 
r_polar = raster::projectRaster(r_crop, crs = polar, method = "bilinear")

# some values are not valid after transformation 
# (rgb range = 1 - 255) set these back to 1
# as they seem to be the black areas
r_polar[r_polar &lt; 1 ] = 1

# define the graticule / grid lines by first specifying
# the larger bounding box in which to place them, and
# feeding this into the sp() gridlines function
# finally the grid lines are transformed to
# the EPSG 3995 projection
pts=SpatialPoints(rbind(c(-180,55),c(0,55),c(180,85),c(180,85)), CRS("+init=epsg:4326"))
gl = gridlines(pts, easts = seq(-180,180,30), norths = seq(50,85,10), ndiscr = 100)
gl.polar = spTransform(gl, polar)

# I also create a single line which I use to mark the
# edge of the image (which is rather unclean due to pixelation)
# this line sits at 55 degrees North similar to where I trimmed
# the image
pts=SpatialPoints(rbind(c(-180,55),c(0,55),c(180,80),c(180,80)), CRS("+init=epsg:4326"))
my_line = SpatialLines(list(Lines(Line(cbind(seq(-180,180,0.5),rep(55,721))), ID="outer")), CRS("+init=epsg:4326"))

# crop a map object (make the x component a bit larger not to exclude)
# some of the eastern islands (the centroid defines the bounding box)
# and will artificially cut of these islands
m = maps2sp(c(-180,200),c(55,90),clip = TRUE)

#----- below this point is the plotting routine
# set margins to let the figure "breath" and accommodate labels
par(mar=rep(1,4))

# plot the grid, to initiate the area
# plotRGB() overrides margin settings in default plotting mode
plot(spTransform(gl, polar), lwd=2, lty=2,col="white")

# plot the blue marble raster data
raster::plotRGB(blue_marble, add = TRUE)

# plot grid lines / graticule
lines(spTransform(gl, polar), add = TRUE, lwd=2, lty=2,col=grid.col.light)

# plot outer margin of the greater circle
lines(spTransform(ll, polar), lwd = 3, lty = 1, col=grid.col.dark)

# plot continent outlines, for clarity
plot(spTransform(m, polar), lwd = 1, lty = 1, col = "transparent", border=grid.col.dark, add = TRUE)

# plot longitude labels
l = labels(gl.polar, crs.longlat, side = 1)
l$pos = NULL
text(l, cex = 1, adj = c( 0.5, 2 ),  col = "black")

# plot latitude labels
l = labels(gl.polar, crs.longlat, side = 2)
l$srt = 0
l$pos = NULL
text(l, cex = 1, adj = c(1.2, -1), col = "white")

# After all this you can plot your own site locations etc
# but don't forget to tranform the data from lat / long
# into the arctic polar stereographic projection using
# spTransform()
</pre>