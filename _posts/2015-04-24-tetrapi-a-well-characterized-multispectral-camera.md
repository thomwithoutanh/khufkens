---
title: 'TetraPi: a well characterized multispectral camera'
author: Koen Hufkens
layout: post
permalink: /2015/04/24/tetrapi-a-well-characterized-multispectral-camera/
categories:
  - DIY
  - Engineering
  - Environment
  - Research
  - Science
tags:
  - diy
  - engineering
  - raspberry pi
  - research
---
<h4>Motivation and goals</h4>
Many of the projects on the Public Lab deal with infragrams, NDVI or NRG images to measure vegetation health. Although the images produced do discriminate between healthy and diseased or stressed vegetation, the imageging pipeline is not well characterized. Furthermore, commercial companies offering well defined multipsectral cameras (&gt; 2 channels) do this at a steap price tag (~$5000).

Both the lack of a of a well characterized image sensor, which makes quantitative research impossible (by using inverse modeling using amongst others <a href="http://teledetection.ipgp.jussieu.fr/prosail/">PROSAIL</a> or <a href="http://ieeexplore.ieee.org/xpl/articleDetails.jsp?reload=true&amp;arnumber=609073">DART</a>) and lowering the price of an inherently simple device which should be accessible to everyone made me start this project.

For my concept two conditions need to be met:
<ol>
	<li>Designing a multispectral camera based upon a raspberry pi and a raspberry pi multiplexer, and writing the necessary software.</li>
	<li>Characterize the raspberry pi imaging sensor; extract the spectral response curves and set those free on the web.</li>
</ol>
<h4>1. TetraPi: a multispectral raspberry pi</h4>
<h5>Hardware</h5>
Designing the housing, either as a sled design to be mounted in a standard outdoor security camera housing and a fully independent camera has been fairly straightforward. Below you see the design of a camera sled which fits a <a href="http://www.amazon.com/Vitek-VT-EH10-Indoor-Outdoor-Enclosure/dp/B002L16NUC/ref=sr_1_1?ie=UTF8&amp;qid=1431971696&amp;sr=8-1&amp;keywords=vitek+housing">VITEK security camera housing</a>, as well as the stand alone / mobile version.

[caption id="" align="aligncenter" width="351"]<a href="https://farm8.staticflickr.com/7637/17066772778_2ae22e6d0d_z_d.jpg"><img src="https://farm8.staticflickr.com/7637/17066772778_2ae22e6d0d_z_d.jpg" alt="" width="351" height="263" /></a> TetraPi, mobile version[/caption]

[caption id="" align="aligncenter" width="353"]<img class="" src="https://farm8.staticflickr.com/7588/17252775952_067d3a9b03_z_d.jpg" alt="" width="353" height="467" /> TetraPi, static version[/caption]

Both designs are largely finished and might need some cosmetic updates, but by and large these designs work. Features include a provision for four raspberry pi cameras (NOIR or RGB) and a filter holder which accomodates 1" dielectric filters as produced by for example <a href="https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=1001">Thorlabs</a>. This modular design allows for simulateous acquisition of for example a photochemical reflectance index (PRI), a true normalized difference vegetation index (NDVI), as well as a standard RGB image. Multiplexing the cameras is taken care of by an <a href="http://www.ivmech.com/magaza/en/ivmech-m-2/ivport-raspberry-pi-camera-module-multiplexer-p-90">IVMECH raspberry pi shield</a> multiplexer.
<h5><em>envisioned mobile camera features</em></h5>
<ul>
	<li>four channel camera [finished]</li>
	<li>adaptable filters (1" dielectric filters) [finished]</li>
	<li>12-24VDC battery operated [ubec ordered]</li>
	<li>local and remote (cable) trigger [3 mm jack connection in place, push button installed]</li>
	<li>ad-hoc wifi access to upload images directly to a laptop [software issue]</li>
</ul>
<h5><em>envisioned static camera features</em></h5>
<ul>
	<li>four channel camera [finished]</li>
	<li>adaptable filters (1" dielectric filters) [finished]</li>
	<li>12-24VDC  Power-over-Ethernet (PoE) or battery operated [ubec ordered]</li>
	<li>UPS feature using [awaiting the pijuice shield]</li>
	<li>ad-hoc wifi access to upload images directly to a laptop for remote sites [software issue]</li>
	<li>wired and wireless time lapse image acquisition and uploads, for on the grid location [todo]</li>
</ul>
Operation of the mobile version is still dependent on a tripod to ensure a proper exposure. The mobile version will use a ubec to step down an external 12-24VDC source to 5V needed by the pi, alternatively it can be hooked up directly to the 5V output of for example a drone. The plywood design should make the whole setup not overly heavy. There is also still room to shrink the design, but for convenience reasons I keep it larger as it makes it easier to work on the system.
<h5> CAD designs</h5>
The drawings of both cameras can be found here:
<ul>
	<li>The mobile camera ( <a href="https://www.dropbox.com/s/wiy71jmbatptzfn/tetracam_mobile.FCStd?dl=0">FreeCAD</a> / DXF)</li>
	<li>The sled design ( <a href="https://www.dropbox.com/s/4ug9gd7dsxxdx53/tetracam.FCStd?dl=0">FreeCAD</a> / DXF)</li>
</ul>
All designs are based upon 3 and 9 mm plywood or acrylic. For environmental reasons I try to minimize plastics as much as possible. The mobile version has not fixed power socket yet as I still have to deside on the size.
<h5>Budget</h5>
<h5><em>mobile version</em></h5>
<ul>
	<li>Raspberry pi B+ ($30)</li>
	<li>4 x raspberry pi camera ($120)</li>
	<li>16GB micro SD card ($10)</li>
	<li>USB wifi adapter ($10)</li>
	<li>5V ubec ($10)</li>
	<li>camera multiplexer ($96)</li>
	<li>real time clock ($10)</li>
	<li>small electronics ($4)</li>
	<li>plywood housing ($5)</li>
	<li>screws / stand offs (&lt; $4)</li>
</ul>
Total: ~$300
<h5><em>static version</em></h5>
<ul>
	<li>Raspberry pi B+ ($30)</li>
	<li>pijuice battery pack UPS ($41)</li>
	<li>4 x raspberry pi camera ($120)</li>
	<li>16GB micro SD card ($10)</li>
	<li>USB wifi adapter ($10)</li>
	<li>5V ubec ($10)</li>
	<li>camera multiplexer ($96)</li>
	<li>outdoor housing  ($25)</li>
	<li>plywood housing ( $5)</li>
</ul>
Total: ~$350

Note:

I do not include the dielectric filters in the price as these are somewhat optional and run at $100 a piece. The prices listed above also indicate a full system, meaning 4 cameras where this might exceed the need of some people.

A complete system would run for less than \$800 dollar (or a fifth of the price of a commercial system - in hardware cost). A standard NDVI system would set you back \$400. Altenatively, a photochemical reflectance index (PRI) system (2 filters) will cost you a little south of \$500.
<h5>Software</h5>
Little progress has been made on this part. I have some script which I can recycle from my PhenoPi project but I'm still waiting on the multiplexer and until it's arrival little can be done from a software development point of view.
<h4>2. Characterizing the OmniVision OV5647 imaging sensor</h4>
Inverse modelling of either the standard RGB signal or derived spectral features is based upon a thorough knowledge of the spectral response of a camera. The spectral response of a camera is defined as the wavelength dependent sensitivity of a given sensor channel (RGB).

For reasons unknown most imaging sensors do not come with this information. However, this spectral response can be measured using a monochromator (a rare piece of equipment) or if one knows the spectral response of your source light and the wavelength dependent characteristics of a diffraction grating.

Using both a well characterized light source (measured with a spectrometer) and a well characterized grating, it should be possible to back out the spectral response of the raspberry pi camera. I refer to my raspberry pi spectrometer research notes for all the details regarding measuring the source light, and further calibrating the spectra.

More updates soon...