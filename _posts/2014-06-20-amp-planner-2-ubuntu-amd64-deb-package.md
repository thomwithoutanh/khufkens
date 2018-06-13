---
title: AMP planner 2, Ubuntu amd64 deb package
author: Koen Hufkens
layout: post
permalink: /2014/06/20/amp-planner-2-ubuntu-amd64-deb-package/
categories:
  - Linux
  - Software
  - UAV
tags:
  - arducopter
  - linux
  - software
---
I picked up my UAV work again. However, I was annoyed by the fact that the standard debian package of the APM planner 2 does not install nicely on the latest Ubuntu 14.04 LTS install because of the upgraded openscenegraph99 libraries (which mess up dependencies).

As such I changed the control file on the original debian package, repackaged it and giving some softlinking the package should installs nicely without triggering any errors in your package manager.

!! The nightly builds of the APM planner now address the above issue. I encourage you to download a nightly build or latest release when using Ubuntu 14.04. My bitbucket repository is offline as of now. !!

<del>Awaiting an update of the official APM planner deb file on the main site you can download my file (at your own discretion) and read the installation instructions here: https://bitbucket.org/khufkens/apm-planner-fix.</del>

&nbsp;