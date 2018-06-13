---
title: PhenoPi
layout: page
permalink: /phenopi/
---
<h3>Citizen science based phenology using a low cost camera network</h3>
Most of my research revolves around green leaf phenology, or the study of seasonal changes in vegetation. These changes can be observed visually by looking at vegetation (are there leaves or not?). However, for the sake of convenience and consistency it's far easier to setup a network of sensors. This is exactly what has been done by Prof. dr. Andrew D. Richardson at Harvard University.

The <a href="http://phenocam.sr.unh.edu/webcam/about/">PhenoCam project</a> provides automated near-surface remote sensing (digital images) of vegetation which are processed in a consistent way to track seasonal changes in vegetation greenness. Sadly, the cameras used within the context of the project are rather expensive and outside the price range of your average naturalist or interested citizen scientist.

Given this lack of a cheap alternative to 'real' PhenoCams I decided to cobble together a citizen science PhenoCam. Given that it's based upon a raspberry pi I baptized it <strong>PhenoPi</strong>. The project is currently in development, with a working prototype. There is already a public repository on my bitbucket page but the code or the documentation is rough around the edges. However, with time this should all come together. Keep track of my blog and my bitbucket page for updates. Also, support my <a href="https://hackaday.io/project/5865-phenopi">HackADay project</a> with the same name.
<h2>Project Details</h2>
<h3>What is phenology?</h3>
Phenology is the study of (the timing of) recurring life cycle events. This includes both animals and plants alike. However, my particular research interest goes out to plant phenology. Plant phenology studies when plants leaf out, when they drop their leaves or when other life cycle events happen (flowering, shoot elongation, needle drop, ... ).

Plant phenology is also tightly coupled to our climate, as it influence how much CO2 is captured through photosynthesis from the atmosphere and how bright the surface of the earth is (it's albedo). In addition it also mediates the flow of water and energy (through transpiration). In the animation below you see the seasonal changes across the globe as the growing season switches from one hemisphere to the next.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Mh4fizmiZa8" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

This process removes substantial amounts of CO2 from the atmosphere, more so in our warming world the growing season length is increasing by 3-5 days per decade. These extra days of photosynthesis provide a negative feedback to the CO2 induced warming of our world. However, how much more CO2 will be removed and how plants will respond to a warmer but also more CO2 rich world remains a question.  To answer these questions we can formulate mathematical constructions which model plant phenology in response to various climatological drivers. The output of detailed atmospheric model displaying variations in CO2 across a year is shown below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/x1SgmFa0r04" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

However, such a model must be validated against real data. One of these sources of data is remote sensing data from various satellites circling the earth. Sadly, these satellites are limited by a spatio-temporal trade off, where one can get either high resolution data at a low temporal scale or low resolution data at a high frequency. In order to optimize current phenological models a high frequency, high resolution view on plant growth would be ideal. This is where PhenoCam project comes into the picture.
<h3>The PhenoCam project</h3>
<a href="http://phenocam.sr.unh.edu/webcam/">The PhenoCam project</a> is a project started and run by Dr. Andrew D. Richarson at Harvard University which uses PhenoCams to keep watch of vegetation at both a high spatial and temporal resolution using conventional cameras. PhenoCams register a standard RGB jpeg every half hour. These images are than converted to time series of vegetation greenness using a green chromatic coordinate (Gcc) index; which is the ratio of green pixels and the images brightness (sum of all channels).
Below you find an image illustrating how various biomes respond to changes in both temperature and precipitation (image by A. D. Richardson). You see that vegetation growth in a temperate deciduous forest (A) is mostly triggered by temperature rather than precipitation. A Mediterranean oak forest (B) on the other hand is more dependent on pulses of precipitation. A similar response is recorded when comparing a northern temperate grassland with a moisture limited tropical grassland location. These PhenoCams provide us with the necessary data to validate our phenology models (including some recent work I did on grasslands). Currently the PhenoCam project has &gt; 200 cameras installed across US and elsewhere.

<img src="http://phenocam.sr.unh.edu/static/img/biome_image.png" alt="Gcc across various biomes" />

Most of the PhenoCams are installed by the project or researchers around the country. Unlike other projects such as <a href="http://budburst.org/">project budburst</a> and the <a href="https://www.usanpn.org/">National Phenology Network</a> which rely heavily on citizen scientists to observer changes in (plant) phenology the PhenoCam project does not offer such opportunity. Although citizen science involvement for data processing is being worked on, the financial burden of the current PhenoCams (~$500-900) makes involving citizen science in data collection difficult.
<h3>PhenoPi</h3>
I recognized that people often have a window pointing towards some street trees or their garden. This is a missed opportunity I feel. I was hoping to engage citizens in collecting digital repeat imagery data. This could only be achieved if the price point of the camera used was lowered, and the process of installing the software would be rather trouble free.

Involvement of citizens would widen the coverage of the PhenoCam network, adding to our knowledge of how plants respond to a changing climate but also filling in gaps in spatial and temporal coverage. The limited distance to most garden trees also ensures a close up view of the vegetation, which is lacking in most of the current PhenoCams.

So, in order to fill this gap I design some basic hardware and software to complete automated image collection (time lapse if you will) using a simple window mounted raspberry pi camera, hence PhenoPi.
<h3>Hardware</h3>
I use a simple housing for a raspberry pi, which uses suction cups to stick to any window pointing at trees. The schematics in FreeCAD format can be found <a href="https://www.dropbox.com/s/v1g0ernjct7mh63/phenopi.FCStd?dl=0">here</a>. I also provide <a href="https://www.dropbox.com/s/qjf53ba701v0m50/phenopi.dxf?dl=0">DXF drawings</a> which can be used in a laser cutter or CNC machine. For those with a 3D printer an <a href="https://www.dropbox.com/s/em69on4z0umzici/phenopi.stl?dl=0">SLT file</a> is provided as well however but I'm not sure about the strength of the design. The design is the third version, and should be final. Below I show the original version which is larger and did not have a viewfinder or any buttons or indicator LEDs. An earlier version of the PhenoPi is shown below, illustrating the operation / mounting on a window.

<img src="https://farm9.staticflickr.com/8771/17042506637_de615ca4c2_z_d.jpg" alt="phenopi v1" />

I would say that from a hardware point of view the project is finished! I did consider some alternative setups but they all lead to either increasing cost, complexity and / or degraded data (moving camera lenses would cause drift which is a major issue in post processing).
<h3>Software</h3>
The current sofware can be found on <a href="https://bitbucket.org/khufkens/phenopi">my bitbucket</a> PhenoPi page (currently offline, broke the code while rewriting some routines - check in later, or check my github page). The code available currently covers an upload script which triggers the camera, a script which pulls additional localized weather data, an installation script for the former and a script to install a real time clock if necessary.

Currently the majority of the work is software based, trying to streamline the install to make it as painless as possible. A complete hands off install will not be possible so it will always remain sort of a DIY project I fear. The current software has been running for almost a year at my parent’s place, capturing the onset of spring greenup nicely! Below, the rise and fall in camera greenness (Gcc) for the birch in the back of the garden.

&nbsp;

<img class="aligncenter wp-image-1082 size-medium_large" src="/uploads/2015/04/example_plot-768x459.png" alt="example_plot" width="768" height="459" />

So, my proof of concept works. Post processing software is already in place as this is an integral part of the PhenoCam project. All software is freely available online and can be found <a href="http://phenocam.sr.unh.edu/webcam/tools/">here</a>.