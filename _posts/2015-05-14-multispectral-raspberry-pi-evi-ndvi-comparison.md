---
title: 'Multispectral raspberry pi: EVI &#8211; NDVI comparison'
author: Koen Hufkens
layout: post
permalink: /2015/05/14/multispectral-raspberry-pi-evi-ndvi-comparison/
categories:
  - DIY
  - Engineering
  - Environment
  - Research
  - Science
tags:
  - diy
  - engineering
  - PhenoCam
  - raspberry pi
  - research
---
Today two filters arrived in my mailbox, one filter is a bandpass filter from <a href="http://www.thorlabs.com/thorproduct.cfm?partnumber=FGS900">400-710nm</a>, the other filter is a longpass filter with the cut-off at <a href="http://www.thorlabs.com/thorproduct.cfm?partnumber=FGL9">721nm</a>. <a href="http://www.thorlabs.com/thorproduct.cfm?partnumber=FGS600">Narrower band pass filters are available</a> and I might order one to see if this further improves image quality.

Using this setup of two NoIR raspberry pi cameras allows me to have 4 channels. Where the NIR, red and the blue probably are relatively well separated in wavelength domain and the green mixes in a bit. As I do not restrict myself to a two band setup I can calculate an additional vegetation index (VI) in addition to the standard <a href="https://en.wikipedia.org/wiki/Normalized_Difference_Vegetation_Index">NDVI VI</a>.

With the addition of a blue band I can calculate the <a href="https://en.wikipedia.org/wiki/Enhanced_vegetation_index">EVI</a> which relies on this band to reduce some of the canopy background (soil) effects. Doing so increases the dynamic range for high biomass regions. Below I show both the NDVI and EVI as calculated from my multispectral raspberry pi camera and a scatterplot plotting the VI against each other. Out of range values (&gt; 1.5 or &lt; -1.5) of the EVI (which is not bound by a finite -1/1 range) were mapped to -1.5 and 1.5 respectively (which renders the sky blue).

The increased dynamic range is visible for the EVI as the EVI shows a non linear relationship with the NDVI, where smaller increments in higher NDVI values (especially &gt;0.4) map to more widely spaced EVI values (Figure, bottom panel). Although no saturating NDVI values are shown in the image the effect should only become more pronounced for higher NDVI values.

Also note that the different non vegetative structures are more easily separated using the EVI. Mulch (indicated by the red dot) shows up clearly in the EVI image while it is hard to distinguish from asphalt (black square) in the NDVI image. Again this shows the advantage of the extra band for some applications. <a href="http://www.khufkens.com/2015/05/10/multispectral-raspberry-pi-first-light/">As shown earlier</a> however, the blue channel is sensitive to Raleigh scattering so a distance effect might creep into the EVI index causing noise and higher uncertainties on the retrieved VI. I'm rather happy with the results so I'm probably going to use this setup. Up next is revisiting the spectral response curve work I was working on.

<img src="https://farm9.staticflickr.com/8807/17397000800_d09a4c03fe_k_d.jpg" alt="comparison" />