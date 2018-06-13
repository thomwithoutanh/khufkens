---
title: 'AmeriFluxR: a R toolbox to facilitate Ameriflux Level2 data exploration'
author: Koen Hufkens
layout: post
permalink: /2016/03/11/amerifluxr-a-r-toolbox-to-facilitate-ameriflux-level2-data-exploration/
categories:
  - Research
  - Science
  - Software
tags:
  - R
  - research
  - science
  - software
---

Today I launch the first version of <a href="http://khufkens.github.io/amerifluxr/">AmerifluxR</a>. The AmeriFluxR package is a R toolbox to facilitate easy Ameriflux Level2 data exploration and downloads through a convenient R <a href="http://shiny.rstudio.com/">shiny</a> based GUI. This toolset was motivated by my need to quickly assess what data was available (metadata) and what the inter-annual variability in ecosystem fluxes looked like (true data).

The package provides a mapping interface to explore the distribution of the data (metadata). Subsets can be made geographically and/or by vegetation type. Summary statistics (# sites / # site years) are provided on top of the page. The Data Explorer tab allows for more in depth analysis of the true data (which is downloaded and merged into one convenient file on the fly). A snapshot of the initial Map and Site Selection landing page is shown below.

<a href="/uploads/2016/03/interface.png"><img class="aligncenter wp-image-1226 size-medium_large" src="/uploads/2016/03/interface-768x421.png" alt="interface" width="768" height="421" /></a>

&nbsp;

In the Data Explorer tab one can plot ecosystem productivity data (GPP / NEE) for a selected site. You can select a plot displaying all data on a daily basis (consecutively) or overlaying data yearly. Note that although all sites are listed, not all of them have accessible data. The plot area will notify you of this.

[caption id="attachment_1227" align="aligncenter" width="768"]<a href="/uploads/2016/03/daily_gpp.png"><img class="wp-image-1227 size-medium_large" src="/uploads/2016/03/daily_gpp-768x487.png" alt="daily_gpp" width="768" height="487" /></a> GPP at Harvard Forest[/caption]

&nbsp;

[caption id="attachment_1228" align="aligncenter" width="768"]<a href="/uploads/2016/03/yearly_nee.png"><img class="wp-image-1228 size-medium_large" src="/uploads/2016/03/yearly_nee-768x490.png" alt="yearly_nee" width="768" height="490" /></a> overlaying daily NEE values (together with the long term mean and standard deviation; LTM and SD respectively)[/caption]

The package can be conveniently installed using only 3 commands on the R terminal (the first line takes care of dependencies, the second line loads devtools which is required to install from a github repository, line 3).
<pre class="lang:r decode:true">install.packages(c("rvest","data.table","RCurl","DT","shiny","shinydashboard","leaflet","plotly","devtools"))
require(devtools)
install_github("khufkens/amerifluxr")</pre>
To get started, just type
<pre class="lang:r decode:true">ameriflux.explorer()</pre>
on the command line and the above screen will pop-up in your favourite browser (preferentially Chrome).

Future development will include higher level products as well as other metrics (yearly summaries, etc...). I welcome anyone to join this effort and potential scientific endeavours that spring from this. Drop me a line by email or on <a href="https://github.com/khufkens">GitHub</a>.