---
title: Loitering away
author: Koen Hufkens
layout: post
permalink: /2013/08/13/loitering-away/
categories:
  - DIY
  - Engineering
  - Research
  - UAV
tags:
  - drone
  - UAV
---
[caption id="" align="alignright" width="350"]<img class="  " style="margin: 10px;" alt="" src="https://farm8.staticflickr.com/7046/6928353289_958961c91c.jpg" width="350" height="234" /> arducopter, old config.. updated pictures will follow[/caption]

I got my <a href="http://copter.ardupilot.com/">arducopter</a> up and running the past week. I updated to the latest firmware (3.0.1). Previously I had to fight wires to calibrate the multicopter during the <a href="http://www.youtube.com/watch?v=-_mjfPlHL9o">compass calibration dance</a>. But I found out that you can do most of the setup of the arducopter over the XBee telemetry, so I don't get tangled anymore. Having calibrated my compass properly and compensating for compass drift due to motor and ESC interference (compassmot values down to 5%!) I get really good GPS hold / loiter performance. During rather windy conditions my multicopter wouldn't drift outside a 2x2m box. During calm wind conditions this was a square meter box at most. Rather impressive performance for a budget quadcopter. Next up, flying some way point missions.