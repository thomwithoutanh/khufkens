---
title: Google Earth Engine time series subset tool
author: Koen Hufkens
layout: post
permalink: /2017/07/22/google-earth-engine-time-series-subset-tool/
categories:
  - python
  - Research
  - Science
  - Software
tags:
  - GEE
  - python
  - research
  - science
  - software
---
<a href="https://earthengine.google.com/">Google Earth Engine (GEE)</a> has provided a way to massively scale a lot of remote sensing analysis. However, more than often time series analysis are carried out on a site by site basis and scaling to a continental or global level is not required. Furthermore, some applications are hard to implement on GEE or prototyping does not benefit from direct spatial scaling. In short, working on a handful of reference pixels locally is often still faster than Google servers. I hereby sidestep the handling of large amounts of data (<a href="http://www.khufkens.com/2016/04/20/modis-hdf-data-extraction-in-r/">although sometimes helpful</a>) to get to single location time series subsets with a GEE hack.

I wrote <a href="https://github.com/khufkens/gee_subset">a simple python script / library called <strong>gee_subset.py</strong></a> which allows you to extract time series for a particular location or it's neighbourhood. This tool is similar to my <a href="https://github.com/khufkens/modis-land-product-subset">MODIS subset</a> or <a href="https://github.com/khufkens/daymetr">daymetr</a> tools, which all facilitate the extraction of time series of remote sensing or climatological data respectively.
<pre class="lang:sh decode:true">git clone https://github.com/khufkens/gee_subset</pre>
My python script expands this functionality to all available GEE products, which include high resolution <a href="https://explorer.earthengine.google.com/#search/tag:landsat">Landsat</a> and <a href="https://explorer.earthengine.google.com/#search/tag:sentinel">Sentinel</a> data, <a href="https://explorer.earthengine.google.com/#search/climate">includes climatological data</a> among others Daymet, but also representative concentration pathway (RCP) CMIP5 model runs.

Compared to the ORNL DAAC MODIS subset tool performance is blazing fast (thank you Google). An example query, calling the python script from R, downloaded two years (~100 data points) of Landsat 8 Tier 1 data for two bands (red, NIR) in ~8 seconds flat. Querying a larger footprint (1x1 km) only creates a small overhead (13 sec. query). The resulting figure for the point location with the derived NDVI values is shown below. The demo script to recreate this figure is included in the <a href="https://github.com/khufkens/gee_subset/tree/master/examples">example folder of the github repository</a>.

[caption id="attachment_1614" align="aligncenter" width="880"]<a href="http://www.khufkens.com/2017/07/22/google-earth-engine-time-series-subset-tool/demo_vis/" rel="attachment wp-att-1614"><img class="wp-image-1614 size-full" src="/uploads/2017/07/demo_vis.png" alt="" width="880" height="524" /></a> NDVI values from Landsat 8 Tier 1 scenes. black lines depicts a loess fit to the data, with the gray envelope representing the standard error.[/caption]