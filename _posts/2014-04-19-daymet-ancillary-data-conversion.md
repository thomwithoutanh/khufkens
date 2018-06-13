---
title: DAYMET ancillary data conversion
author: Koen Hufkens
layout: post
permalink: /2014/04/19/daymet-ancillary-data-conversion/
categories:
  - Linux
  - Research
  - Software
tags:
  - bash
  - climate data
  - data
  - linux
  - research
  - script
  - software
---
I recently posted <a href="http://www.khufkens.com/2014/03/18/daymetr-a-daymet-single-pixel-subset-tool-for-r/">tools</a> to extract <a href="http://daymet.ornl.gov/custom_home">DAYMET </a>data for point locations. However, the product as produced by the DAYMET team is gridded data. If you want to scale models driven by DAYMET data spatially you need access to this gridded (spatial) data.

Sadly, access to the data is rather convoluted. If you want to download this gridded data, or tiles, you first have to figure out which tiles you need. This means, going to the DAYMET website and clicking several times within the tiles you want to download to get their individual numbers. After establishing all these tile numbers you can download them from the <a href="http://daymet.ornl.gov/thredds/catalog/allcf/catalog.html">THREDDS server.</a>

Obviously this routine is less than ideal, if for example you want to do regional to state based research and have to select a large number of tiles. I hope to automate this process. First step in this process is reprojecting the tiles grid to a latitude and longitude. Oddly enough the format of the netCDF file was not strandard and rather confusing. It took me a while to figure out how to accomplish this. Here is a bash script to do so. These instructions work on all <a href="http://daymet.ornl.gov/datasupport">ancillary netCDF grids</a> as provided by DAYMET. From the generate tile file using the below code you can either determine the tile based upon any given point location or alternatively for a region of interest. The latter is my goal and in the works, as the former is covered by previous code.
<pre class="lang:default decode:true">#!/bin/bash

# get filename with no extension
no_extension=`basename $1 | cut -d'.' -f1`

# convert the netCDF file to an ascii file 
gdal_translate -of AAIGrid $1 original.asc

# extract the data with no header
tail -n +7 original.asc &gt; ascii_data.asc

# paste everything together again with a correct header
echo "ncols        8011" 	&gt;  final_ascii_data.asc
echo "nrows        8220"	&gt;&gt; final_ascii_data.asc
echo "xllcorner    -4659000.0" 	&gt;&gt; final_ascii_data.asc
echo "yllcorner    -3135000.0" 	&gt;&gt; final_ascii_data.asc
echo "cellsize     1000" 	&gt;&gt; final_ascii_data.asc
echo "NODATA_value 0"    	&gt;&gt; final_ascii_data.asc 

# append flipped data
tac ascii_data.asc &gt;&gt; final_ascii_data.asc

# translate the data into Lambert Conformal Conic GTiff
gdal_translate -of GTiff -a_srs "+proj=lcc +datum=WGS84 +lat_1=25 n +lat_2=60n +lat_0=42.5n +lon_0=100w" final_ascii_data.asc tmp.tif

# convert to latitude / longitude
gdalwarp -of GTiff -overwrite -t_srs "EPSG:4326" tmp.tif tmp_lat_lon.tif

# crop to reduce file size, only cover DAYMET data areas
gdal_translate -a_nodata -9999 -projwin -131.487784581 52.5568285568 -51.8801911189 13.9151864748 tmp_lat_lon.tif $no_extension.tif

# clean up
rm original.asc
rm ascii_data.asc
rm final_ascii_data.asc
rm tmp.tif
rm tmp_lat_lon.tif</pre>
&nbsp;