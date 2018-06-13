---
title: Auto-align time-series of spherical images
author: Koen Hufkens
layout: post
permalink: /2017/12/25/auto-align-time-series-of-spherical-images/
categories:
  - DIY
  - PhenoCam
  - Photography
  - Research
  - Science
  - Software
tags:
  - engineering
  - image processing
  - PhenoCam
  - research
  - science
  - software
---
From time to time the spherical Theta S camera used in my virtualforest.io project needs maintenance, such as a reset after a power outage or lens cleaning. These manipulations guarantee consistency in the quality of the recorded images but sometimes cause misalignment from one image to the next.

Using the internal gyroscopic data of the camera one can <a href="https://github.com/khufkens/theta_rectify">correct the deformations</a> due to an inclination of the camera, which deform the horizon. Sadly, the included digital compass data is mostly unreliable without an external GPS connected. With no sensor data to rely on the challenge remains in aligning these images (automatically).

[caption id="attachment_1708" align="aligncenter" width="640"]<a href="http://www.khufkens.com/2017/12/25/auto-align-time-series-of-spherical-images/not_aligned/" rel="attachment wp-att-1708"><img class="size-full wp-image-1708" src="/uploads/2017/12/not_aligned.gif" alt="" width="640" height="320" /></a> Two spherical images with an offset along the x-axis.[/caption]

In the above image corrections were made to straighten the horizon. Yet, there still is a clear shift along the x-axis of the images (or a rotation along the vertical axis of the camera). Although the images are fine, they can't be used in a time series or a movie as their relative position changes. Due to this misalignment it also becomes really hard to monitor a specific part of the image for research purposes. One solution would be to manually find and correct these shifts. But, with ~100Gb of virtualforest.io data on file (2017-12-25) this is not a workable solution.

A standard way of dealing with misaligned images which are only translated (movement along x and y axis) is by applying <a href="https://en.wikipedia.org/wiki/Phase_correlation">phase correlation.</a> Phase correlation is based upon aligning the phase component (hence the name) of a (dicrete) fourier transform of the image. A fourier transform translates (image) data and expresses it as a sum of sinus waves (plus their intensities / frequencies) and the relative position of these waves (or phase).  In more technical terms it translates data from the time / space domain in to the frequency / phase domain in order to among others speed up convolutional calculations or filter data based frequency (noise reduction). The use of a fourier transform in this case can be seen as a way to speed up calculating a cross-correlation between two images.

[caption id="attachment_1701" align="aligncenter" width="640"]<a href="http://www.khufkens.com/2017/12/25/auto-align-time-series-of-spherical-images/strip/" rel="attachment wp-att-1701"><img class="wp-image-1701 size-full" src="/uploads/2017/12/strip.jpg" alt="" width="640" height="48" /></a> A selection of an image to use in determining lateral shifts (along the x-axis).[/caption]

In general, the phase correlation algorithm is fairly robust with respect to noise but self-similarity of vegetation corrupts the algorithm non the less. As such, I decided to use only a portion of the original spherical images to determine lateral shifts. I extracted the stems of the trees out of the image as this provides the most information with respect to lateral shifts. In a way the stems are a barcode representing the orientation of the camera. (In other use cases, where more man-made structures are included, this extra step is probably not needed.)

Using only these stem barcode sections I was able to successfully align one season of images. The result is an image time series which can be stacked for movies, analysis of the same portion of the image or used in interactive displays without any visible jumps!

[caption id="attachment_1707" align="aligncenter" width="640"]<a href="http://www.khufkens.com/2017/12/25/auto-align-time-series-of-spherical-images/aligned/" rel="attachment wp-att-1707"><img class="size-full wp-image-1707" src="/uploads/2017/12/aligned.gif" alt="" width="640" height="320" /></a> The same two spherical images aligned using an offset calculated with phase correlation.[/caption]

An interactive temporal spherical display covering one year of virtualforest.io imagery can be seen at this link:

<a href="https://my.panomoments.com/u/khufkens/m/vr-forest-one-year">https://my.panomoments.com/u/khufkens/m/vr-forest-one-year</a>

&nbsp;