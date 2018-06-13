---
title: 'MCD10A1: a combined MODIS aqua / terra snow cover product'
author: Koen Hufkens
layout: post
permalink: /2016/04/25/mcd10a1-a-combined-modis-aqua-terra-snow-cover-product/
categories:
  - Climate
  - Environment
  - Research
  - Science
  - Software
tags:
  - climate
  - code
  - google earth engine
  - research
  - science
  - software
---
For an ongoing project I had to calculate fractional snow cover transition dates across Alaska. These transition dates mark when snow levels drop below 5% coverage or when continuous accumulation of more than 5% starts. The period between these two dates can be considered snow free, and has the potential to support vegetation growth.

For this exercise I used my usual set of tools and data streams (<a href="http://www.khufkens.com/2016/04/20/modis-hdf-data-extraction-in-r/">R + local MODIS HDF tiles</a>). Sadly, this processing chain took forever. Given the rather simple nature of the code I decided to turn to <a href="https://earthengine.google.com/">Google Earth Engine (GEE)</a> and rewrote the analysis into GEE javascript. Below you find the resulting MCD10A1 product, a combined MODIS Aqua / Terra snow cover product. The original MODIS products (MOD10A1/MYD10A1) as processed by the <a href="http://nsidc.org/">National Snow and Ice Data Center</a> have the tendency to be biased low. Hence, my solution fuses the two data streams using a maximum value approach across the two data streams. Furthermore, the script is written to loop over all years with sufficient data (2003 - current), and collects the above mentioned transitions dates. Finally, the script runs a simple regression analysis to mark trends in snow melt dates, answering the question if snow melt occurs later or earlier with each passing year!?

Below you see a snow cover trend map of the US and Canada, with earlier snow melt marked in red (-) and later snow melt marked in blue (+). Some patterns stand out, mainly mountain ranges in the West of the US have seen earlier and earlier snow melt dates. Alaska, doesn't see very clear patterns, aside from earlier snow melt in the southern peninsula. Arctic Canada does see clear earlier snow melt throughout. Surprisingly, some later snow melt dates can be noted as well. From the middle of Alberta, Canada sweeping east to the great lakes a trend of later snow melt dates is noted. A word of caution is needed as these regions might be heavily influenced by the "polar vortex" in 2015, creating a long winter season, and potentially skewing the trend analysis. Furthermore, locations without data are places with a very short snow cover period, which were excluded for clarity (i.e. these are areas were snow cover has less of an influence on the growing season).

In short, as long as the analysis isn't too complicated GEE is a quick way to get good and reproducible results. Sadly, the system does have it's limitation and makes true time series analysis (with custom aggregation and curve fitting techniques) rather difficult if not impossible. I feel that these kinds of analysis are clearly outside the scope of the more GIS oriented GEE setup. I tip my hat to Google for providing this service, rather impressive I must say.

<img class="aligncenter size-medium_large wp-image-1255" src="/uploads/2016/04/snowMelt_trends-768x463.png" alt="snowMelt_trends" width="768" height="463" />
