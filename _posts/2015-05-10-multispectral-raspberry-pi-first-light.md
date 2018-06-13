---
title: 'Multispectral raspberry pi: first light'
author: Koen Hufkens
layout: post
permalink: /2015/05/10/multispectral-raspberry-pi-first-light/
categories:
  - DIY
  - Engineering
  - Research
  - Science
tags:
  - diy
  - engineering
  - image processing
---
I just got the first images from the <a href="http://www.khufkens.com/projects/tetrapi/">multispectral raspberry pi</a>. I currently have two cameras running on the camera multiplexer. One standard RGB pi camera and one NoIR pi camera with a blue rosco filter.

Below you can find two NDVI images (plus the original RGB image) generated from either a combination of the bands from the two cameras or a standard infragram processing (using Ned Horning's <a href="https://github.com/nedhorning/PhotoMonitoringPlugin">Fiji Plugin</a>). For reference, black is 0, white is 1.

Note that the fidelity of the NDVI image from the split camera setup (<a href="http://www.ivmech.com/magaza/en/ivmech-m-2/ivport-raspberry-pi-camera-module-multiplexer-p-90">multiplexer</a>) generates images with a far higher range in values then those solely generated using the NoIR infragram camera. Also note that with distance the signal decreases for the infragram as well, where the row of trees in the back for the dual camera setup shows a far less pronounced distance effect due to raleigh scattering of blue light (thanks cfastie for reminding me of this).

I ordered two glass filters, one bandpass filter (400-700nm) and one longpass filter (&gt; 721nm). This should allow me to increase the contrast even further using two NoIR cameras. The reason for no using a single red bandpass filter is to allow me to retain a visible light image which is well defined (I know what filters I use - instead of guessing on the response of the cut filter of the raspberry pi camera ). This visible light image will allow me to calculate other indices as well e.g. a greenness index (Gcc) or maybe an enhanced vegetation index (EVI).

I will try to characterize this spectral response of the colour raspberry pi camera as well as the NoIR one in the coming weeks or so, but until then I'll make due with two filters and two NoIR cameras.

<h3>Two camera setup</h3>

<img src="https://farm9.staticflickr.com/8876/17508553381_1942e956f5_z_d.jpg" alt="NDVI - two cameras" />

<h3>single camera infragram</h3>

<img src="https://farm8.staticflickr.com/7674/17482393056_f86b0fe9b0_z_d.jpg" alt="NDVI - single camera" />

colour scale
&lt;img src="http://i.publiclab.org/system/images/photos/000/009/863/original/NDVI_VGYRM_lutA.jpg" alt="scale" style="width: 200px;"/&gt;

<h3>RGB</h3>

<img src="https://farm9.staticflickr.com/8898/17492805451_b95390bd30_z_d.jpg" alt="RGB - single camera" />