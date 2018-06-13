---
title: Pi power-over-ethernet
author: Koen Hufkens
layout: post
permalink: /2016/08/14/pi-power-over-ethernet/
categories:
  - DIY
  - Engineering
tags:
  - diy
  - engineering
  - raspberry pi
---
For a project of mine I needed power-over-ethernet (PoE) to be available on my Raspberry Pi. There are several reasons why one would want to use PoE. In my case I need a reliable network connection, and power couldn't be provided consistently using solar or stand alone methods either. PoE combines both things without the necessity of having to run an extra power cable, proving a rather neat package.

Sadly, the PoE options out there weren't flexible enough to my liking. Take for example <a href="https://www.pi-supply.com/product/pi-poe-switch-hat-power-over-ethernet-for-raspberry-pi/">this $40 raspberry pi PoE HAT</a>. Although it provides power to pi there is no option to hook up additional power hungry USB devices. Some finagling would do the trick. However, if I should thinker anyway I better make something that fits my needs.

<img class="alignnone" src="https://farm9.staticflickr.com/8185/28965261635_f898bc78e1_z_d.jpg" width="640" height="360" />

So instead I made my own little (passive) PoE power solution. A first proof of concept looked like the image above. I used a passive PoE splitter to split the ethernet and the power on the left side (black dongle), to power a <a href="https://www.amazon.com/gp/product/B0194SKADU/ref=oh_aui_detailpage_o04_s00?ie=UTF8&amp;psc=1">$20 uBEC </a>which converts 12-60V into 5V/3A (red shield, I discarded the housing). The 5V output of the uBEC gets connected to the power leads two micro-USB cables (black cables) where one has its TX/RX lines patched through to a  male USB type-A cable. The latter setup allows for data transfer between a high powered device (hard drive) and the Raspberry Pi. This setup bypasses the issue of the PoE hat, which only powers the Pi and consequently can only provide limited power to connected USB device.

<img class="alignnone" src="https://farm9.staticflickr.com/8841/28346568014_21b5f3522c_z_d.jpg" width="640" height="360" />

Unhappy with this rather ugly solution I remade the same setup using screw terminals and proper type-A port on a larger prototyping shield. The whole setup remains the same but this board now neatly stacks on top of the Pi (with some additional support from standoffs and female header rows). On the left you see a version with only one powered USB port, which powers the Raspberry Pi, and an additional screw terminal output. On the right you see the exact same setup as above, where the left hand USB ports (2x) provide power only, while the right two ports consist of one patch port (connecting a power free data connection) and injecting power and data into the other.

<img class="alignnone" src="https://cdn-learn.adafruit.com/assets/assets/000/017/931/original/raspberry_pi_modelb5V.png?1405267988" width="701" height="407" />

Technically, I could connect power directly to the 5V rail on the Pi, but since I'm not sure how stable and clean the power of the uBEC is I avoid this for now. Power spikes can easily kill GPIO pins or completely fry my Pi. A future iteration might include basic fuses and overpower protection as outlined above. This would limit damage by a less than ideal or reverse input voltages.

But, for now my Raspberry Pi PoE issues are solved.

&nbsp;