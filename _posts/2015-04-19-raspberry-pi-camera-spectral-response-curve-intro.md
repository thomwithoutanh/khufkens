---
title: 'raspberry pi camera: spectral response curves (intro)'
author: Koen Hufkens
layout: post
permalink: /2015/04/19/raspberry-pi-camera-spectral-response-curve-intro/
categories:
  - DIY
  - Engineering
  - Research
  - Science
tags:
  - camera
  - optics
  - PhenoCam
  - PhenoPi
  - raspberry pi
  - research
  - science
  - spectrometetry
  - spectrum
---
<span style="color: #ff0000;">UPDATE: Since, I found the spectral response curves (in the visible spectrum) of both v1 and v2 raspberry pi cameras. You can find the digital response curves on <a style="color: #ff0000;" href="http://www.khufkens.com/projects/ov5647-spectral-response/">my projects page</a>.</span>

In the previous post I described my project to democratize phenology monitoring. From a purely scientific point of view adding citizen science cameras to the PhenoCam network would increase the coverage, however to truly replace the current StarDot cameras in more than a citizen science project I need to characterize the spectral response of the raspberry pi camera's imaging sensor.

The spectral response of any imaging sensor (or most of them anyway) is determined by the formula used on the microlenses in <a href="https://en.wikipedia.org/wiki/Bayer_filter">the bayer filter</a>. In practice every imaging sensor is monochrome, it's only by adding this bayer filter, a checkerboard of tiny red/green/blue filters alternatively overlaying all pixels, that you can extract color from your imaging sensor.

Sadly, most spectral responses of the imaging sensors are corporate secrets. I'm unsure why, but I assume that knowing the spectral response of the filters tells something about which process is used and how. This being said, this doesn't mean you can't measure it!

Measuring the spectral response of a sensor is generally done using a <a href="https://en.wikipedia.org/wiki/Monochromator">monochromator</a>, a light source which emits a particular wavelength, and a <a href="https://en.wikipedia.org/wiki/Spectrometer">spectrometer</a>, a device to measure the intensity of that light source in function of wavelength. Here the monochromator emits light of a known wavelength which is simultaneously measured by the spectrometer and the imaging sensor. The spectrometer provides a true intensity measurement at this wavelength while the imaging sensor provides an intensity measurement for every bayer filter colour at this particular wavelength. If we cycle through all wavelengths the output of such an analysis are spectral response curves, showing the sensitivity of each bayer filter component colour across all wavelengths. Although this methodology is sound, finding a monochromator is rather hard. Yet an alternative approach exists.

A monochromator uses a diffraction grating to split a known light source into it's component wavelengths. This same diffraction grating is not selective and at any given time outputs all light components only at a slightly different angle. The monochromator only passes the desired wavelength, as shown below (left image).

[caption id="" align="alignnone" width="486"]<a href="http://skullsinthestars.com/2010/03/04/rolling-out-the-optical-carpet-the-talbot-effect/"><img class="" src="http://skullsinthestars.files.wordpress.com/2010/03/fourierseries_gratingapps_c.jpg?w=640" alt="" width="486" height="127" /></a> Basic function of a monochromator (left) and a spectrometer (right). The monochromator passes only light of a certain wavelength, while the spectrometer measures the intensity of the light at a given wavelength (as reflected of an object). Clicking the image will guide you to an interesting blog post about the Talbot effect, a diffraction property.[/caption]

So, in theory we could use a diffraction grating to do all the work for us without the intermediary and elusive monochormator! However, the transmission properties of a grating are wavelength dependent. This is the reason why in an ordinary (monochromator / spectrometer) setup you need to measure the true intensity as well as the image sensor response simultaneously. The only way to calculate the spectral response curve of the sensor is to factor in the wavelength dependent transmission properties of a grating. For most classroom gratings these properties are not described, but when ordering from an optical instrument builder they are!

In short, given a known light source (characterized using a spectrometer), a cheap but <a href="http://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=1123#e42a503e-ff97-45d0-9b84-312574e582f0-1123">characterized grating</a> it is possible to get a crude approximation of the spectral response of any imaging sensor using one image (well two actually as you need to calibrate the relative location of the spectrum - using a CFL light for example)!

Next step, designing a grating housing (a cheap spectrometer) for this task.

&nbsp;

&nbsp;