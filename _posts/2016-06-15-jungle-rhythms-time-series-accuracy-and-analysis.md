---
title: 'Jungle Rhythms: time series accuracy and analysis'
author: Koen Hufkens
layout: post
permalink: /2016/06/15/jungle-rhythms-time-series-accuracy-and-analysis/
categories:
  - Jungle Rhythms
  - Research
  - Science
tags:
  - citizen science
  - DR Congo
  - ecology
  - forest
  - jungle rhythms
  - phenology
  - research
  - science
---
Today marks the 6th month since <a href="https://www.zooniverse.org/projects/khufkens/jungle-rhythms">Jungle Rhythms</a>' launch. During this period more than 8500 citizen scientists, from <a href="http://www.khufkens.com/2016/05/24/jungle-rhythms-user-statistics-location/">both sides of the Atlantic</a>, contributed to the project. Currently, ~60% (~182 000 classifications) of the total workload is finished and almost half of the tasks at hand are fully retired.

Next week I'll present some of the first results at the <a href="http://www.atbc2016.org/EN/events.php?IDManif=837&amp;IDModule=71&amp;IDRub=1239">Association of Tropical Biology and Conservation (ATBC) 2016 meeting</a>. But, not to keep anyone waiting I'll describe some of the retrieved time series, in addition to initial estimates of classification accuracy in this blog post. These analysis are done with a first iteration of my processing code and partial retrievals, so gaps and processing errors are present. I'll focus on <a href="https://en.wikipedia.org/wiki/Millettia_laurentii"><em>Millettia laurentii</em></a>, commonly known as Wenge or faux ebony, a tropical endangered tree species.

Below you see a figures marking the four annotated life cycle phases of tropical trees as present in the Jungle Rhythms data. From top to bottom I list flowering, fruit development, fruit dissemination (fr. ground) and leaf drop or senescence. In the figures black bars mark the location of Jungle Rhythms derived estimations of life cycle events, while red bars mark the location of independent validation data. Dashed vertical grey lines mark annual boundaries.

[caption id="" align="aligncenter" width="1025"]<a href="https://farm8.staticflickr.com/7420/27069152973_da86a2e9d0_o_d.png"><img class="" src="https://farm8.staticflickr.com/7420/27069152973_da86a2e9d0_o_d.png" width="1025" height="395" /></a> Figure 1.[/caption]

Overall, Jungle Rhythms classification results were <strong>highly accurate</strong> with overall accuracies using <a href="https://en.wikipedia.org/wiki/Cohen%27s_kappa"><em>Kappa</em> values</a>, a measure of accuracy, range from a low of 0.56 (Figure 1.) up to 0.97, where values between 0.81–1 are considered to be in almost perfect agreement. Lower values of of the <em>Kappa</em> index are mostly due to missing Jungle Rhythms data. Not all data has been processed yet, and these data haven't been excluded from the validation statistics. For example in Figure 1. the year 1950 and 1947 are missing. In more complete time series, accuracy rises to <em>Kappa</em> values of 0.85 and 0.97, Figures 2. and 3 respectively.

[caption id="" align="aligncenter" width="1025"]<a href="https://farm8.staticflickr.com/7699/27400247420_94a1f55581_o_d.png"><img src="https://farm8.staticflickr.com/7699/27400247420_94a1f55581_o_d.png" width="1025" height="395" /></a> Figure 2.[/caption]

On a life cycle event basis similar performance is noted. However, there might be a slight bias for instances where events span longer time periods, a known processing error. Furthermore, some uncertainty is also due to an imperfect validation dataset. For example, the lack of validation data (red marks) in 1953 for senescence in Figure 2 is an error in the validation data not in the Jungle Rhythms classification data. This further illustrates that error rates do exist in "expert" classified data.

Although no formal analysis has been executed a quick visual comparison shows recurrent leaf drop and flowering at the <a href="http://www.khufkens.com/2016/03/01/drc-weather-and-life-cycle-events/">peak of the dry season</a>. Although some trees show similar patterns (Figure 1 and 2), others do not (Figure 3, below). These differences in phenology across individuals shows the great plasticity of tree phenology in the tropics, and potential independence of both light or temperature cues, but more <a href="http://www.khufkens.com/2016/03/01/drc-weather-and-life-cycle-events/">in tune with water availability</a> (proximity to water sources).

[caption id="" align="aligncenter" width="1025"]<a href="https://farm8.staticflickr.com/7225/27066461914_8f51b6e466_o_d.png"><img src="https://farm8.staticflickr.com/7225/27066461914_8f51b6e466_o_d.png" width="1025" height="395" /></a> Figure 3.[/caption]

Summarizing, classification results of the Jungle Rhythms project are highly accurate. Furthermore, it's highly likely that with proper post processing all classification results will reach perfect agreement. More so, the retrieved data already illustrates some of the phenological patterns in <em>Millettia laurentii</em> (Wenge), and how they correspond across years (Figures 1 and 2) or how they might differ between individuals (Figure 3).

Once more, I thank all the citizen scientists who contributed to this project. Without your contributions, one classification at a time, this would not have been possible.