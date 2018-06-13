---
title: 'Processing Jungle Rhythms data: coordinate transformations and line intersections'
author: Koen Hufkens
layout: post
permalink: /2016/02/16/processing-jungle-rhythms-data-coordinate-transformations-and-line-intersections/
categories:
  - Jungle Rhythms
  - Research
  - Science
tags:
  - jungle rhythms
  - research
  - science
---
I mentioned that I started working on processing some annotations to data. This will get me a feeling for the data quality, but more so get me thinking about how to process the data efficiently.

In this blog post I'll quickly outline the methodology I'll use to process the annotations. First I have to give a quick summary on what the data looks like once annotated, how to deconstruct this data into usable data.

Below you see a picture of an annotated yearly section. Red dots outline the most common intersection coordinates for the yearly section (green crosses represent all measurements), while green lines represent the annotated life cycle events within the yearly section. Note the accuracy of all the annotations, rather amazing work by everyone who contributed!

With these key locations (red dots) of the yearly section, mainly: the start, middle and end (providing the general orientation of the yearly section within the image), the annotations can translated into true data (compensating for skewness and warping in the picture).

<img class="alignnone" src="https://farm2.staticflickr.com/1608/24686026539_cd3d12eb85_z_d.jpg" alt="" width="640" height="338" />

Each yearly section, will be processed a half year at a time using roughly four steps:
<ol>
	<li>For each year I make sure the bottom axis (bottom left - bottom middle or bottom middle - bottom right) is aligned along the Cartesian x-axis. This is done by rotating the data around the bottom left or bottom middle point, respectively. -&gt; in this image the axis is relatively close to optimal!</li>
	<li>Since I only process data within half a yearly section I trim annotated lines to fit each six month period.</li>
	<li>After trimming the annotations to fit neatly within the first six months I need to transform all these coordinates to days within a year. I know the spacing between the row is equal. As such, the total length of a row can be calculated as the distance of a line which crosses the beginning and end of half a yearly section.</li>
	<li>What remains is to calculate the distance between the beginning and start of an annotated segment relative to the total length to determine the days they cover during the year.</li>
</ol>
Finally, all these data will be combined into a matrix. This matrix will then be linked to the original species data, kept in a separate file.