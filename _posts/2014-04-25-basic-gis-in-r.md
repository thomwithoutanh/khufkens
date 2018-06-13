---
title: Basic GIS in R
author: Koen Hufkens
layout: post
permalink: /2014/04/25/basic-gis-in-r/
categories:
  - R
  - Software
tags:
  - code
  - example
  - GIS
  - R
  - software
---
Recently I had to help out a colleague with processing some spatial data in R. The overall goal was to find landscape parameters (topography, soil characteristics, etc.) within the neighbourhood of a flux tower.

The below  code gives a short introduction on how to extract data from both raster and shapefiles (vector data), very common operations .  I will add more tips and tricks as I come across them.
<pre class="lang:default decode:true "># Load libraries.
# For raster data
require(raster)

# For vector data
require(sp)

# Set flux tower coordinates.
lat= 45.2041
lon=-68.7402

# Define lat / lon projection.
lat_lon &lt;- CRS("+init=epsg:4326")

# Define UTM projection.
utm &lt;- CRS( "+proj=utm +zone=19 +north +ellps=WGS84 +datum=WGS84 +no_defs")

# Read in the coordinates and assign them a projection, # otherwise they remain just 'numbers'
location &lt;- SpatialPoints(cbind(lon,lat), lat_lon)

# Convert to utm using the above projection string
# using spTransform.
location_utm &lt;- spTransform(location, utm)

# Load raster Digital Terrain Map (DTM) data.
# If the raster data has no projection,
# you will have to assign a projection.
# I assume the file has a projection.
dtm &lt;- raster('DTM.tif')

# Relative height of the location of the tower.
dtm_data &lt;- extract(dtm,location_utm)

# Read in soil shapefile.
soils &lt;-  readShapeSpatial("soils.shp")

# Assign projection to the soils.shp file
# if not provided.
proj4string(soils) &lt;- utm

# Extract soil polygon attributes.
soil_attributes &lt;- over(location_utm,soils)</pre>
&nbsp;