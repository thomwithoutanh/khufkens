---
title: A cup of deep learning coffee
author: Koen Hufkens
layout: post
permalink: /2016/01/12/a-cup-of-deep-learning-coffee/
categories:
  - Research
  - Science
  - Software
tags:
  - PhenoCam
  - research
  - science
  - software
---
Recently I started experimenting with <a href="https://en.wikipedia.org/wiki/Deep_learning">deep learning,</a> a set of <a title="Algorithm" href="https://en.wikipedia.org/wiki/Algorithm">algorithms</a> that attempt to model high-level abstractions in data by using multiple processing layers. The reason for this excursion in a more exotic classifier framework is the tricky issue of snow.

Snow on evergreen canopies artificially decreases the greenness value of an image, corrupting an otherwise rather smooth PhenoCam time series of greenness. Below you see a split image of a normal snow free canopy, and a snow covered canopy, visually showing this decrease in greenness.

<img class="aligncenter" src="https://farm2.staticflickr.com/1681/24259869051_c6f270bf0a_z_d.jpg" alt="" width="640" height="488" />

These snowy days result in the dips in Gcc (greenness) as seen in the time series of image greenness below.

<img class="aligncenter" src="http://phenocam.sr.unh.edu/data/archive/niwot2/ROI/niwot2_EN_0001_gcc90_3day.png" alt="" width="800" height="300" />

Within the lab we tried various techniques to spot snow on these canopies. These techniques were mostly colour metrics, based upon the distances in colour space (from either white or grey) of a particular pixel or region of interest. However, all efforts failed or were not generalizable across all sites, meaning that every site would need to be parameterized independently (which is a processing headache).

In an effort to address this classification problem I installed the <a href="http://mi.eng.cam.ac.uk/projects/segnet/">SegNet variety</a> of the <a href="http://caffe.berkeleyvision.org/">Caffe Deep Learning Framework </a>(hence the blog post title). The <a href="http://mi.eng.cam.ac.uk/projects/segnet/">SegNet framework</a> allows for pixel based classification based upon a deep learning approach, originally designed to quickly (matter of millisecons) classify street images to assist autonomous vehicles. However, I hope this approach might help solve the issue of classifying these snowy days, recognizing snowy canopies instead of pedestrians. Results will follow in the coming weeks.

&nbsp;

&nbsp;

&nbsp;

&nbsp;