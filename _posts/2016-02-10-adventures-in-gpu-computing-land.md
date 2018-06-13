---
title: Adventures in GPU computing and Deep Learning land
author: Koen Hufkens
layout: post
permalink: /2016/02/10/adventures-in-gpu-computing-land/
categories:
  - Engineering
  - Research
  - Science
  - Software
tags:
  - CUDA
  - deep learning
  - GPU computing
  - research
  - science
  - software
---
The past weeks I've been toying with <a href="http://www.khufkens.com/2016/01/29/odyssey-caffe-segnet-installation-instructions/">GPU computing</a> and <a href="http://www.khufkens.com/2016/02/04/deep-learning-snowy-images/">Deep Learning</a>. This is a brief summary of things I've learned in setting things up (hardware and software).

Let's talk about hardware! I'm currently running everything on a <a href="http://www.amazon.com/gp/product/B00V4HY522?keywords=msi%20gtx%20gaming%204g&amp;qid=1453489057&amp;ref_=sr_1_2&amp;sr=8-2">NVIDIA MSI GTX960 4GB Gaming</a> card. Given the cost of the card (~$250) and the fact that for a lot of the Deep Learning applications memory will be the limiting factor (Google: "<em>Check failed: error == cudaSuccess (2 vs. 0) out of memory</em>") rather than CUDA cores, this is a really good card. Most faster cards (more CUDA cores) will have the same amount of memory. In short, you might gain some time but it does not allow you to train more complex models (larger in size). Unless you upgrade to a NVIDIA GTX Titan X card with 12GB of memory (costing 5x more) or dedicated compute units such as a Tesla K40 you won't see a substantial memory increase in the product line.

On the software end, the documentation on the installation of the Caffe framework is sufficient to get you started (on Ubuntu at least). The documentation is good and the community seems to be responsive, judging from the forum posts, github issues. However, I've not engaged in any real trouble shooting requests (as things went well). The only issue I encountered when setting up the <a href="http://mi.eng.cam.ac.uk/projects/segnet/">Caffe-Segnet</a> implementation was the above mentioned "out of memory error". Although people mentioned that they got things running on 4GB cards I just couldn't get it to work. In the end I realized that the memory being used to drive my displays (~500MB) might just tip the scale.

Indeed, offloading graphics duties from the dedicated card onto the motherboard's integrated graphics did free up enough memory to make things work. An important note here is that you need to install the <a href="https://developer.nvidia.com/cuda-downloads">CUDA Toolbox and drivers</a> without their OpenGL drivers. The CUDA Toolbox overwrites the Ubuntu originals and breaks the graphics capabilities of your integrated graphics in the process.

To install the CUDA Toolbox correctly, with full memory capabilities first select the integrated graphics (iGPU) as your preferred GPU in your system BIOS. Then download the CUDA Toolbox runtime and run the following command:
<pre class="lang:sh decode:true">sudo ./cuda_x.x.x_linux.run --no-opengl-libs</pre>
This should install all CUDA libraries and drivers while at the same time prevent the Toolbox from overwriting your iGPU OpenGL libraries. In addition you might also want to make sure you initiate the NVIDIA devices on boot (devices should be accessible but not driving any display) by placing the below script in your /etc/init.d/ folder.
<pre class="lang:sh decode:true ">#!/bin/bash

NVDEVS=`lspci | grep -i NVIDIA`
N3D=`echo "$NVDEVS" | grep "3D controller" | wc -l`
NVGA=`echo "$NVDEVS" | grep "VGA compatible controller" | wc -l`

N=`expr $N3D + $NVGA - 1`
for i in `seq 0 $N`; do
   mknod -m 666 /dev/nvidia$i c 195 $i
done

mknod -m 666 /dev/nvidiactl c 195 255
</pre>
(I'm not quite sure the latter script has any use as the devices might be initiated already anyway. However, I rather list it here for future reference.)

In the end, both the <a href="http://places.csail.mit.edu/demo.html">MIT scene recognition model</a> as well as the <a href="http://mi.eng.cam.ac.uk/projects/segnet/">Caffe-Segnet</a> implementation work. Below you see the output of the Segnet Camvid demo, which uses the input of a webcam and classifies it on the fly into the 12 classes. The output is garbage as the scene does not represent a street view (the original intent of the classifier), but it shows that the system runs at a solid 210ms per classification.

<img class="aligncenter wp-image-1039" src="/uploads/2016/02/camvid_output-300x124.png" alt="camvid_output" width="640" height="265" />

&nbsp;