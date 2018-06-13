---
title: OV5647 spectral response
author: Koen Hufkens
layout: post
permalink: /2015/11/02/ov5647-spectral-response/
categories:
  - DIY
  - Engineering
  - Research
  - Science
tags:
  - diy
  - engineering
  - raspberry pi
---
In order to use the raspberry pi cameras within a more rigorous framework I needed the full spectral response of the chipset used in these cameras, the OV5647 by OmniVision. I set out to do acquire the response curves using a <a href="http://www.khufkens.com/2015/04/19/raspberry-pi-camera-spectral-response-curve-intro/">DIY diffraction grating approach</a>.

During this process I was contacted by Howard Shapiro who volunteered his old spectrofluorometer to accomplish this task faster and more precisely. Although, initial tests together with Howard showed promise, it would remain an arduous task to quantify the whole spectral response. Recently, Howard managed to dig up
<a href="/uploads/2015/11/006_paper_rhodes_omnivision_bsi2.pdf">some documentation</a> on the chipset which displayed the spectral response (quantum efficiency, QE) in a graph (given that the OmniBSI chipset as displayed is the one residing in the OV5647). This has made things considerably easier. I used these slides to digitize the quantum efficiency of the OV5647 between 400 and 700 nm. A physical measurement will still be needed to quantify the remaining near infrared spectrum (&gt; 700 nm, data can be found on the <a href="http://www.khufkens.com/ov5647-spectral-response/" target="_blank">OV5647 spectral response page</a>).

![](https://farm6.staticflickr.com/5802/22708983135_19b6043fe4_z_d.jpg)

Sadly, the original graph only covers wavelenghts between 400 and 700 nm, leaving out the near infrared (NIR) part of the spectrum. The red edge, located at 680-730nm, is a key part of the spectrum key in vegetation remote sensing applications. At the red edge vegetation reflectance changes from low to higher values. The magnitude of this differences is an indication of plant health.

Although I got most of the picture, due to good detecitve work by Howard, I still don't have the complete picture. Some physical measurements will still be necessary to get the complete spectral response / QE of the chip but I'm at least halfway there.

![](https://farm1.staticflickr.com/652/22521053850_e739739c6f_z_d.jpg)
