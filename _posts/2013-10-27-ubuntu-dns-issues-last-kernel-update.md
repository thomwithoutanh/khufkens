---
title: Ubuntu DNS issues (last kernel update)
author: Koen Hufkens
layout: post
permalink: /2013/10/27/ubuntu-dns-issues-last-kernel-update/
categories:
  - Linux
  - Software
tags:
  - blog
  - DNS
  - linux
  - network
---
I just had some trouble with my laptop not resolving http addresses after the last update of the Ubuntu kernel. Seems like there are issues with the DNS server settings. I resolved the issue by renaming / removing the deprecated old DNS server settings file resolve.conf (/etc/resolve.conf). Removing or renaming the file restored all DNS dependent services.