---
title: Spatial Panel Analysis Method in R
author: Koen Hufkens
layout: post
permalink: /2014/07/23/spatial-panel-analysis-method-in-r/
categories:
  - R
  - Research
  - Software
tags:
  - Alaska
  - arctic
  - R
  - research
  - software
---
<p class="size-medium wp-image-363">I just posted a new project on my software page. It's a fixed effects panel analysis function. It uses a method from econometrics to detect trends over time across a window of pixels. This increases the robustness of trend analysis as compared to a pixel by pixel regression analysis. An added benefit is the speed you gain as you downscale the original data by a factor defined by the panel size used. The function makes use of the Panel Data Econometrics "plm" package in R.  A detailed overview can be found in <a href="http://www.jstatsoft.org/v27/i02/paper">this publication</a>.</p>
For a project in the Alaska I ran a quick analysis on MODIS GPP trends across the whole dataset (2001-2013). Below you see the results of a panel analyis of the MODIS GPP data using a panel size of 11 or roughly ~1/10 a degree. Given the pronounced warming of the Arctic a steady increase in GPP is noted across Alaska (up to 0.03 Kg C m -2).

[caption id="attachment_363" align="alignnone" width="640"]<a href="/uploads/2014/07/panel_analysis_GPP.png"><img class="wp-image-363 size-full" src="/uploads/2014/07/panel_analysis_GPP.png" alt="fixed effects panel analysis of MODIS GPP over time" width="640" height="480" /></a> fixed effects panel analysis of MODIS GPP over time[/caption]