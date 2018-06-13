---
title: 'Processing Jungle Rhythms data: intermediate results'
author: Koen Hufkens
layout: post
permalink: /2016/02/17/processing-jungle-rhythms-data-intermediate-results/
categories:
  - Jungle Rhythms
  - R
tags:
  - citizen science
  - jungle rhythms
  - R
  - software
---
After a few days of struggling with R code I have the first results of processed data at my fingertips! Below you see a 3 window plot which shows the original image on top, the annotated image in the middle and the final extracted data at the bottom. The middle window gives you an idea of the geometry involved in the calculation of the final data at the bottom. I'll shortly detail my approach, as I abandoned the idea I proposed in a previous blog post.

Using the most common six coordinates for each yearly sections (red dots overlaying the individual markings as green crosses - middle window) I calculate the approximate location of the rows as marked by the red dots on the vertical bold lines (<a href="http://mathworld.wolfram.com/Circle-LineIntersection.html">line - circle intersection</a>). All annotations are then projected on to this ideal row (<a href="http://mathworld.wolfram.com/OrthogonalProjection.html">projection of a point onto a line</a>), rendering the coloured lines for different observation types. Finally, with the overall distance of each given row (on a half year basis - <a href="http://mathworld.wolfram.com/PythagorassTheorem.html">point to point distance</a>) I calculate the location in time which is occupied by an annotation. Classifying the lines into life cycle event types is done by minimizing the <a href="http://mathworld.wolfram.com/Point-LineDistance2-Dimensional.html">distance to the ideal row</a>.

All images are shown to 10 independent citizen scientists, and for each individual annotation these values are summed. Where there is a high degree of agreement among citizen scientists I will see a larger total sum. If there is an unanimous agreement among them the sum would be 10 for a given day of year (DOY). As not all subjects are retired, the value as displayed below only has a maximum count of 8. The spread around the edges of the annotations is due to variability in the classifications.

You already notice some pattens in the life cycle events. More on these patterns in a later post, when I can match the data with species names.

<img class="alignnone" src="https://farm2.staticflickr.com/1505/24727871039_bda9d2c734_b_d.jpg" alt="" width="652" height="1024" />

&nbsp;