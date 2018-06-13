---
title: Deep learning snowy images
author: Koen Hufkens
layout: post
permalink: /2016/02/04/deep-learning-snowy-images/
categories:
  - PhenoCam
  - Research
  - Science
  - Software
tags:
  - deep learning
  - PhenoCam
  - research
  - science
---
<a href="http://www.khufkens.com/2016/01/12/a-cup-of-deep-learning-coffee/">Past week</a> I started to play with the <a href="http://caffe.berkeleyvision.org/">Caffe deep learning framework</a>. Although I initially planned on using the<a href="http://mi.eng.cam.ac.uk/projects/segnet/"> SegNet branch</a> of the Caffe framework to classify snow in PhenoCam images. However, given that it concerns a rather binary classification I don't need to segment the picture (I do not care where the snow in the image is, only if it is present). As such, a more semantic approach could be used.

Luckily people at MIT had already trained a classifier, the Places-CNN, which deals with exactly this problem, <a href="http://places.csail.mit.edu/demo.html">characterizing an image scene</a>. So, instead of training my own classifier I gave theirs a try. Depending on the image type, and mostly the view angle the results are very encouraging (even with their stock model).

For example, the below image got classified as: mountain snowy, ski slope, snowfield, valley, ski_resort. This all seems very reasonable indeed. Classifying a year worth of images at this site yielded an accuracy of  89% (compared to human observations).

&nbsp;

<a href="/uploads/2016/02/24721196381_042bb62b7e_z.jpg" rel="attachment wp-att-1032"><img class="alignnone size-full wp-image-1032" src="/uploads/2016/02/24721196381_042bb62b7e_z.jpg" alt="24721196381_042bb62b7e_z" width="640" height="427" /></a>

However, when the vantage point changes so does the accuracy of the classification, mainly due to the lack of images of this sort in the original training data set I presume. The image below was classified as: rainforest, tree farm, snowy mountain, mountain, cultivated field. As expected, the classification accuracy dropped to a mere 13%. There is still room for improvement using PhenoCam based training data. But, building upon the work by the group at MIT should make these improvements easier.

<img class="alignnone" src="https://farm2.staticflickr.com/1441/24519154990_502f2e2523_z_d.jpg" alt="" width="640" height="488" />