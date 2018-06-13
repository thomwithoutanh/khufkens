---
title: 'snotelr &#8211; a R package for easy access to SNOTEL data'
author: Koen Hufkens
layout: post
permalink: /2017/01/03/snotelr-a-r-package-for-easy-access-to-snotel-data/
categories:
  - Environment
  - R
  - Research
  - Science
  - Software
tags:
  - climate data
  - data
  - R
  - research
  - science
---
I recently created the <a href="https://khufkens.github.io/MCD10A1">MCD10A1 product</a>. This is a combined MODIS MOD10A1 and MYD10A1 product, alleviating some of the low bias introduced by either overpass through a maximum value approach. This approach has been used in the study by <a href="http://www.sciencedirect.com/science/article/pii/S0309170812002953">Gascoin et al. (2013)</a> but I wanted some additional validation of the retrieved values.

As such I looked at the <a href="http://www.wcc.nrcs.usda.gov/about/mon_automate.html">SNOTEL network</a> which  "... is composed of over 800 automated data collection sites located in remote, high-elevation mountain watersheds in the western U.S. They are used to monitor snowpack, precipitation, temperature, and other climatic conditions. The data collected at SNOTEL sites are transmitted to a central database, called the Water and Climate Information System, where they are used for water supply forecasting, maps, and reports." Here, the snowpack metrics could provide the needed validation data for my MCD10A1 product.

Although the SNOTEL website offers plenty of plotting options for casual exploration and the occasional report, but the interface remains rather clumsy with respect to full automation. As such, and similar to my <a href="http://www.khufkens.com/2016/03/11/amerifluxr-a-r-toolbox-to-facilitate-ameriflux-level2-data-exploration/">amerifluxr package</a> (both in spirit and execution), I created the <strong><a href="https://khufkens.github.io/snotelr/">snotelr R package</a></strong>. Below you find a brief description of the package and it's functions.
<h2 id="installation">Installation</h2>
You can quick install the package by installing the following dependencies
<pre class="lang:default decode:true ">install.packages("devtools")</pre>
and downloading the package from the github repository
<pre class="lang:default decode:true">library(devtools)
install_github("khufkens/snotelr")
</pre>
<pre class="lang:default decode:true">library(devtools)
install_github("khufkens/snotelr")
</pre>
<h2 id="use">Use</h2>
Most people will prefer the GUI to explore data on the fly. To envoke the GUI use the following command:
<pre class="lang:default decode:true">library(snotelr)
snotel.explorer()</pre>
This will start a shiny application with an R backend in your default browser. The first window will display all site locations, and allows for subsetting of the data based upon state or a bounding box. The bounding box can be selected by clicking top-left and bottom-right.

<img src="https://farm1.staticflickr.com/325/31266804673_131c3e8898_b_d.jpg" alt="" />

The <em>plot data</em> tab allows for interactive viewing of the soil water equivalent (SWE) data together with a covariate (temperature, precipitation). The SWE time series will also mark snow phenology statistics, mainly the day of:
<ul>
 	<li>first snow melt</li>
 	<li>a continuous snow free season (last snow melt)</li>
 	<li>first snow accumulation (first snow deposited)</li>
 	<li>continuous snow accumulation (permanent snow cover)</li>
 	<li>maximum SWE (and it’s amount)</li>
</ul>
<img src="https://farm1.staticflickr.com/429/31959389961_90723239f3_b_d.jpg" alt="" />

For in depth analysis the above statistics can be retrieved using the <strong>snow.phenology()</strong> function
<pre class="lang:default decode:true "># with df a SNOTEL file or data frame in your R workspace
snow.phenology(df)</pre>
To access the full list of SNOTEL sites and associated meta-data use the <strong>snotel.info()</strong> function.
<pre class="lang:default decode:true "># returns the site info as snotel_metadata.txt in the current working directory
snotel.info(path = ".") 

# export to data frame
data = snotel.info(path = NULL)</pre>
To query data for e.g. site 924 as shown in the image above use:
<pre class="lang:default decode:true ">download.snotel(site = 924)</pre>
&nbsp;
<div class="language-R highlighter-rouge"></div>