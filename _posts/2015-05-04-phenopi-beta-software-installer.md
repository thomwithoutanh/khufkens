---
title: 'PhenoPi: beta software installer'
author: Koen Hufkens
layout: post
permalink: /2015/05/04/phenopi-beta-software-installer/
categories:
  - DIY
  - Engineering
  - Research
  - Science
  - Software
tags:
  - diy
  - engineering
  - PhenoPi
  - research
  - software
---
<strong>UPDATE: the installer is currently offline as I broke the code while rewriting routines and haven't had time to fix this yet. Check in later.</strong>

I'm releasing a first version of my PhenoPi software installation package onto the world. The goal of the set of scripts is to minimize the amount of time spend on setting up a PhenoPi camera. The current set of scripts should be run on a clean raspberry pi to ensure a proper setup. Any volunteers to test the code are welcome (but do this on a non operational pi as it might mess with your stuff). Currently no images will be uploaded to the PhenoPi server, so everything is stored in the phenopi_images folder.

New additions to the code are a privacy filter so images do not include up to the bottom half of the image in those uploaded for scientific research. This to avoid privacy issues when overlooking gardens when I'm primarily interested in the vegetation higher up or in the distance.

## Installation

In your raspberry pi home directory (/home/pi) clone the project to your raspberry pi using the following command (with git installed)

git clone https://khufkens@bitbucket.org/khufkens/phenopi.git

(this location will be updated to github in the near future, if this command doesn't work check my github page)

all files will be cloned into a directory called phenopi

## Use

To run the basic install using the following command:

sh /home/pi/phenopi/install_phenopi.sh site_name privacy_value

or

./install_phenopi.sh site_name privacy_value

in the /home/pi/phenopi directory

with "site_name"  the name of the site (no spaces allowed) and "privacy_value" how much of the bottom of the image in % you want to see removed (0, 25 or 50 are accepted values, default is 0)

After the installation your camera should be up and running and you should be able to find a website displaying constantly updating image at

http://IP:8080

This will website will look like this, showing the current image stream as well as the latest uploaded image (at night the colours are strange due to reflections of status LEDs and long exposures).

[![Screen_Shot_2015-05-02_at_22.47.15.png](http://i.publiclab.org/system/images/photos/000/009/739/medium/Screen_Shot_2015-05-02_at_22.47.15.png)](http://i.publiclab.org/system/images/photos/000/009/739/original/Screen_Shot_2015-05-02_at_22.47.15.png)

## Notes

Make sure that your raspberry pi camera is enabled, a description on how to enable your camera is provided on [the raspberry pi site](https://www.raspberrypi.org/documentation/usage/camera/)