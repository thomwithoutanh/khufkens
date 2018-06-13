---
title: Crunching Jungle Rhythms data
author: Koen Hufkens
layout: post
permalink: /2016/02/11/crunching-jungle-rhythms-data/
categories:
  - Jungle Rhythms
  - R
  - Research
  - Science
tags:
  - jungle rhythms
  - research
  - science
  - software
---
I haven't blogged about Jungle Rhythms in a while. So here is a quick update on things! I'm currently working through the first batch of Jungle Rhythms data.

Although the fully annotated data is not in, I've partial data to work on and get an algorithm running. This algorithm would extract the phenological data as annotated by everyone who contributed and turn them into true dates (or weeks of a particular year).

Sadly, the data structure as used by Zooniverse is currently less than ideal. Zooniverse data exports use a comma separated file format (<a href="https://en.wikipedia.org/wiki/Comma-separated_values">CSV</a>) with <a href="http://www.json.org/">JSON</a> content. However, <a href="https://www.r-project.org/">R</a>, in which I do most of my statistics and processing, is rather bad in dealing with JSON data. Even using <a href="https://www.python.org/">Python</a>, which handles JSON better, the data structure remains rather cumbersome. I submitted a GitHub ticket (i.e. a way to request features in software or report bugs) raising this concern, and it will be addressed in the near future.

In the mean time, I'll still code up a processing routine to assess the intermediate results. Hopefully the new data format will make all this a bit more straightforward and transparent.

&nbsp;

&nbsp;