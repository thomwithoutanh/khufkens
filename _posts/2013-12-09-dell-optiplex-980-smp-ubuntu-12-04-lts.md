---
title: Dell Optiplex 980 SMP (Ubuntu 12.04 LTS)
author: Koen Hufkens
layout: post
permalink: /2013/12/09/dell-optiplex-980-smp-ubuntu-12-04-lts/
categories:
  - Linux
  - Software
tags:
  - debugging
  - linux
  - software
---
My Dell Optiplex 980 at work doesn't seem to play nice with Ubuntu 12.04 LTS. It fails to detect hyper-treading support on the CPU and defaults back to a single core setup when effectively running a 8-core CPU. This is rather annoying.

It seems that acpi settings are to blame, adding:
<pre>acpi=ht</pre>
to your grub config file mediates this issue and enables SMP support. Sadly, adding this parameter is undone by kernel updates.