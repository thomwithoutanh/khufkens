---
title: UAV vegetation monitoring
author: Koen Hufkens
layout: post
permalink: /2013/08/29/uav-vegetation-monitoring/
categories:
  - Research
  - Software
  - UAV
tags:
  - arducopter
  - blog
  - FOTO
  - image processing
  - research
  - UAV
---
Yesterday I took my quadcopter for a spin with a small camera attached to take pictures of the field below. The field where I do my test flights and general practice runs is an unmanaged grassland. The grassland has diverse structure depending on moisture and nutrient availability. The idea was to see if it was possible to quantify this diversity based upon texture metrics. Sadly the city is hosting the world cup sheep herding this week-end and therefore has cut the grass. I still gave it a try as texture patterns remained visible in the pictures I took (sadly without much ecological significance and mostly due to straw left behind by the mower). The picture below shows a subset of the original image (excluding the landing skids of the quadcopter). Note the differences in texture.

[caption id="" align="alignnone" width="508"]<img style="margin: 10px;" src="http://farm4.staticflickr.com/3672/9620890672_952b0fe937_o.png" alt="" width="508" height="506" /> B&amp;W grassland image from UAV[/caption]

To quantify the complexity of the grassland I applied the FOTO method as described by Proisy et al. (2007). The FOTO method uses radially averaged Fourier spectra to capture differences in the dominant frequencies of a scene. Most common implementations of the method pertain to quantifying canopy structure, however this approach is universal as it can capture any difference in texture (from grassland to canopy or urban areas). The result of this classification is shown as an overlay of RGB colours on the original black and white image below. Although the camera is no high end camera and my quadcopter not top notch either the results show the potential of UAVs and basic image processing techniques to quantify grassland (bio-)diversity. Optimizing the camera (better optics) and the UAV platform for heavy lifting (hexa / octo setup) would make for an easily deployable setup to map (bio-)diversity on regular intervals at low cost.

[caption id="" align="alignnone" width="508"]<img src="http://farm8.staticflickr.com/7346/9620890872_2a6c214e78_o.png" alt="" width="508" height="509" /> FOTO based grassland classification[/caption]

<strong>References:</strong>

Proisy, C., Couteron, P., &amp; Fromard, F. (2007). Predicting and mapping mangrove biomass from canopy grain analysis using Fourier-based textural ordination of IKONOS images. Remote Sensing of Environment, 109(3), 379–392. doi:10.1016/j.rse.2007.01.009

&nbsp;