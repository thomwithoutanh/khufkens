---
title: DaymetR, an official Daymet R package
author: Koen Hufkens
layout: post
permalink: /2014/05/20/daymetr-an-official-r-package/
categories:
  - R
  - Software
tags:
  - package
  - R
  - software
---
I have described my DaymetR scripts in previous posts (<a href="http://www.khufkens.com/2014/03/18/daymetr-a-daymet-single-pixel-subset-tool-for-r/">here</a> and <a href="http://www.khufkens.com/2014/04/20/daymetr-gridded-data-extension/">here</a>). However, some communication with Michele Thornton a DAYMET admin at Oak Ridge National Laboratories brought to light some issues in the way I handled specifying a region of interest. Mainly, the reprojection of the netcdf ancillary files at the edges. This is a typical issue when reprojecting raster data. As a solution I requested a shape file of the grid tiles instead of a raster and Michele kindly provided.

After cleaning up my code I also decided to wrap both the single pixel location as well as the gridded data extraction tools in a proper R package!! The package also includes the map by default so no additional data needs to be downloaded. However, I do provide the shapefile of the DAYMET grid tiles separately.

You can download my first R package on my bitbucket page linked to on my software page or by following this <a href="https://bitbucket.org/khufkens/daymetr">link</a>. I validated the code after the recent changes in server setup at DAYMET, and all seems to be fine. Please report any bugs if you find any.