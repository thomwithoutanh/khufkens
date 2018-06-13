---
title: DaymetR, a Daymet single pixel subset tool for R
author: Koen Hufkens
layout: post
permalink: /2014/03/18/daymetr-a-daymet-single-pixel-subset-tool-for-r/
categories:
  - R
  - Research
  - Software
tags:
  - climate data
  - R
  - research
  - Richardson Lab
  - software
---
Daymet is a collection of algorithms and computer software designed to interpolate and extrapolate from daily meteorological observations to produce gridded estimates of daily weather parameters - as concisely described on the <a title="Daymet website" href="http://daymet.ornl.gov/" target="_blank">Daymet website</a>.

As I'm extensively using daily meteorological data to drive my grassland model, quick and easy access to this data is key. However accessing single pixel values through the java tool provided was a bit cumbersome and did not fit my workflow. As such I wrote my own tool which queries the website and allows you to subset time series of a single pixel location (given a latitude, longitude position) all within R. Data is either just dowloaded to the current working directory or imported as a structured array into your R workspace for further analysis or formatting.

You can find a link to the DaymetR code on my software page or follow <a title="DaymetR on bitbucket" href="https://khufkens.github.io/daymetr" target="_blank">this link to my github</a>.

ps. since this post I have added a python version of the same code. A link can be found on the software section of my website.