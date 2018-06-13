---
title: DaymetR / DaymetPy updates
author: Koen Hufkens
layout: post
permalink: /2015/11/15/daymetr-daymetpy-updates/
categories:
  - R
  - Research
  - Science
  - Software
tags:
  - climate data
  - daymet
  - R
  - software
  - update
---
The ORNL DAAC which hosts the Daymet data is shifting to a HTTPS only policy. As such I updated my DaymetR R package to reflect these changes. Users running an old version will have to reinstall the new version to reflect the migration to secure data connections.

You can find the updated package on my github page (see link on the software page). Please reinstall the package to guarantee continued functionality. Similar corrections have been made to the DaymetPy code.