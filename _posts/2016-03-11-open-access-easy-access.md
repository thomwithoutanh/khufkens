---
title: open access != easy access
author: Koen Hufkens
layout: post
permalink: /2016/03/11/open-access-easy-access/
categories:
  - Op-Ed
  - Research
  - Science
  - Software
tags:
  - API
  - code
  - data
  - open access
  - open data
  - research
  - science
  - software
---
Over the past few years, writing software left and right, I noticed a trend. Most of the software I write serves one purpose: making <a href="https://en.wikipedia.org/wiki/Open_access_(publishing)" target="_blank">open access</a> data - accessible! This should not be!

There has been a steady push for open data, increasing transparency and reproducibility of scientific reporting. Although many scientific data sources are indeed open, their access is not (easy), especially for the less computer savvy.

For example, NASA provides a wealth of open data. Although there are a <a href="https://lpdaac.usgs.gov/tools/modis_reprojection_tool" target="_blank">few tools</a> on which one can rely, non are user friendly. Luckily, most of those can be replaced by open source tools coded by data users (<a href="https://github.com/seantuck12/MODISTools">ModisTools</a>, <a href="https://github.com/khufkens/modis-land-product-subset">MODIS LSP</a>, <a href="http://www.gdal.org/" target="_blank">GDAL</a>). The European Space Agency (ESA)  fares even worse, where their data access is more restrictive and <a href="http://www.vito-eodata.be/PDF//image/Data_pool_manual.pdf" target="_blank">accessing data is an equal or even bigger mess</a>. However, ESA has seen a recent push for a more user centric experience <a href="http://proba-v-mep.esa.int/blog/mep-proba-v-release-january-26" target="_blank">on some of the projects</a>.

Looking at the field of ecology some projects do well and maintain APIs such at <a href="https://daymet.ornl.gov/">Daymet</a> and <a href="http://www.tropicos.org">Tropicos</a>. Although Tropicos requires a personal key which makes writing toolboxes cumbersome. The later left me with no other choice than <a href="http://www.khufkens.com/2016/02/12/scraping-tropicos-data-in-r/">to scrape the website</a>. The Oak Ridge National Laboratories (ORNL) also offers <a href="https://daac.ornl.gov/MODIS/">MODIS land product subsets</a> through an API, with interfaces coded by users. However, these data are truly open access (as in relatively easy to query) and should be considered the way forward.

This contrasts with for example resources such as <a href="http://www.theplantlist.org/" target="_blank">The Plant List</a>, which offers a wealth of botanical knowledge guarded behind a search box on a web page, only to be resolved by either downloading the whole database or by using a <a href="http://www.plantminer.com/">third party website</a>. Similarly the <a href="http://nsidc.org/">National Snow and Ice Data Center</a> oldest snow and ice data is stored <a href="http://www.khufkens.com/2014/07/24/georeferencing-daily-snow-depth-analysis-data/">in an incomprehensible format</a> (the more recent data is offered in an accessible geotiff format). Surprisingly, even large projects such as Ameriflux, with a rather prominent <a href="http://ameriflux.lbl.gov/" target="_blank">internet</a> <a href="https://twitter.com/ameriflux" target="_blank">presence</a>, suffer the same fate, i.e. a wealth of data largely inaccessible for quick and easy use and review.

Pooling several data access issues in the above examples, I think I've illustrated that open access data does not equal easily accessible data. A good few of us write and maintain toolboxes to alleviate these problems for themselves and the community at large. However, these efforts take up valuable time and resources and can't be academically remunerated as only a handful of tools would qualify as substantial enough to publish.

I therefore would plead that data producers (projects alike) to make their open data easily accessible by:
<ol>
	<li>creating proper APIs to access all data or metadata (for querying)</li>
	<li>making APIs truly open so writing plugins can be easy if you don't do it yourself</li>
	<li>writing toolboxes that do not rely on proprietary software (e.g. Matlab)</li>
	<li>assigning a true budget to these tasks</li>
	<li>appreciating those that develop tools on the side</li>
</ol>
