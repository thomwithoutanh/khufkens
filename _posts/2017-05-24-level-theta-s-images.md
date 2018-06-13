---
title: Level Theta S images
author: Koen Hufkens
layout: post
permalink: /2017/05/24/level-theta-s-images/
categories:
  - Photography
  - Software
tags:
  - Photography
  - ricoh theta
  - software
---
My [Virtual Forest](http://virtualforest.io) project is still running strong and generates tons of spherical images (currently ~50GB). However, the post on which the camera sits is not perfectly level.  The Theta S camera normally compensates for this using an internal gyroscope which detects pitch and roll of the camera.  Yet, when downloading images directly from the camera no adjustments are made and the pitch and roll data is merely recorded in the EXIF data of the image.

As such I wrote a small bash script which rectifies (levels the horizon) in Theta S spherical images using this internal EXIF data. This is an alternative implementation to the [THETA EXIF Library](https://github.com/regen100/thetaexif) by [Regen](https://github.com/regen100). I use his cute Lama test images for reference. All credit for the funky images go to Regen. Below is the quick install guide to using my script. I hope it helps speed up people's Theta S workflow.

## Install
Download, fork or copy paste the script from [my github repository](https://github.com/khufkens/theta_rectify) to your machine and make it executable.

```bash
$ chmod +x theta_rectify.sh
```

## Use
```bash
$ theta_rectify.sh image.jpg
```

The above command will rectify the image.jpg file and output a new file called image_rectified.jpg.

![](/uploads/2017/05/equirectangular.jpg)

![](/uploads/2017/05/equirectangular_rectified.jpg)

Visual comparison between my results and those of [Regen's python script](http://www.regentechlog.com/2014/06/26/python-thetaexif/) show good correspondence.

## Requirements
The script depends on a running copy of exiftools, imagemagick and POVRay. These tools are commonly available in most Linux distros, and can be installed on OSX using tools such as homebrew. I lack a MS Windows system, but the script should be easily adjusted to cover similar functionality.