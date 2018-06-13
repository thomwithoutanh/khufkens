---
title: 'DaymetR &#8211; gridded data extension'
author: Koen Hufkens
layout: post
permalink: /2014/04/20/daymetr-gridded-data-extension/
categories:
  - R
  - Research
  - Software
tags:
  - climate data
  - R
  - research
  - software
---
Short recap: Daymet is a collection of algorithms and computer software designed to interpolate and extrapolate from daily meteorological observations to produce gridded estimates of daily weather parameters - as concisely described on the <a title="Daymet website" href="http://daymet.ornl.gov/" target="_blank">Daymet website</a>.

I'm extensively using daily meteorological data to drive a grassland growth model and quick and easy access to this data is therefore key. Although I previously coded <a href="http://www.khufkens.com/2014/03/18/daymetr-a-daymet-single-pixel-subset-tool-for-r/">a script to deal with single pixel data extraction</a> I now updated my DaymetR package to include gridded (tiled) data downloads through the specification of a region of interest (top left / bottom right) or single point location.

The script has the following syntax:
<pre class="wrap:true lang:default decode:true">download.Daymet.tiles(lat1=36.0133,
lon1=-84.2625,
lat2=NA,
lon2=NA,
start_yr=1980,
end_yr=2012,
param="ALL")</pre>
If only the first set of coordinates is provided (lat1 / lon1) the tile in which these reside is downloaded. If your location / region of interest [defined by a top left (lat1 / lon1) and bottom right (lat2 / lon2) coordinate set] falls outside the scope of the DAYMET data coverage a warning is issued. All tiles covering the region of interest are downloaded for their respective parameters, e.g. minimum and maximum temperature (see bitbucket documentation for details).

This final addition is a spatial extension of the previous code, serving primarily the spatial scaling needs of my modelling exercises. All code can be found on through my software page or by following <a href="https://bitbucket.org/khufkens/daymetr/overview">this direct link to my bitbucket page</a>. I hope this might serve other people in the R community.

&nbsp;