---
title: 'raspberry pi camera: spectral response curves (grating properties)'
author: Koen Hufkens
layout: post
permalink: /2015/04/20/raspberry-pi-spectral-response-curves-grating-properties/
categories:
  - DIY
  - Engineering
  - Research
  - Science
tags:
  - diy
  - engineering
  - PhenoPi
  - raspberry pi
  - research
  - science
  - spectrometer
  - spectrometry
---
<span style="color: #ff0000;">UPDATE: Since, I found the spectral response curves (in the visible spectrum) of both v1 and v2 raspberry pi cameras. You can find the digital response curves on <a style="color: #ff0000;" href="http://www.khufkens.com/projects/ov5647-spectral-response/">my projects page</a>.</span>

As described previously, a diffraction grating splits light in it's wavelength components (intensities). However, the angle of the diffracted light as it 'exits' the diffraction grating is dependent on the number of slits (grooves) in the grating. To correctly align any sensor (parallel) with the grating and register the diffracted light a little math is required.

For a diffraction order m, given an incident beam of light at angle $sin_{\theta_i}$,  a given wavelength $\lambda$ and a slit density d; a diffracted beam will exit at an angle $sin_{\theta_d}$ according to:

$d [ sin_{\theta_i} - sin_{\theta_d}  ] = m \lambda$

or in function of the 'exit' angle:

$\theta_d = asin(\frac{m \lambda} {d} + sin_{\theta_i})$

Using this relationship we can calculate the incident angle at which the the exiting light at a given wavelength will be orthogonal to the grating (or parallel to a sensor), or alternatively the angle at which the detector should be placed.

Using the above equation I ran an analysis for a set number of incident angles and wavelengths to extract the overall diffraction properties for a 300 lines/mm and 1351 lines/mm grating (a professional Thorlabs grating and DVD grooves respectively). Optimal grating angles, where the diffracted light exits orthogonal to the grating (going straight into a sensor) is calculated to be ~12 and ~67 degrees for (300 and 1351 lines/mm respectively). I'll be using a 300 lines/mm grating at a 12 degree angle.

[caption id="attachment_621" align="alignnone" width="832"]<a href="/uploads/2015/04/300_lines_mm.png"><img class=" wp-image-621" src="/uploads/2015/04/300_lines_mm.png" alt="300 lines/mm grating" width="832" height="687" /></a> 300 lines/mm grating[/caption]

[caption id="attachment_622" align="alignnone" width="832"]<a href="/uploads/2015/04/1351_lines_mm.png"><img class=" wp-image-622" src="/uploads/2015/04/1351_lines_mm.png" alt="1351 lines/mm grating" width="832" height="688" /></a> 1351 lines/mm grating[/caption]