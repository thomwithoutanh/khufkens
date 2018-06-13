---
author: Koen Hufkens
layout: page
title: Scientific Software
permalink: /code/
menu: true
order: 4
---

Below you find a list of my most mature scientific software, which at times is deposited at official (peer-reviewed) repositories. A full list of all my projects can be found on [my github page](https://github.com/khufkens). Additional small code snippets are indexed as [gists](https://gist.github.com/khufkens).

# R packages

## [daymetr](https://github.com/khufkens/daymetr)

![](https://travis-ci.org/khufkens/daymetr.svg)![](https://codecov.io/gh/khufkens/daymetr/branch/master/graph/badge.svg)![](https://www.r-pkg.org/badges/version/daymetr)![](https://cranlogs.r-pkg.org/badges/grand-total/daymetr)

A programmatic interface to the [Daymet web services](http://daymet.ornl.gov). Allows for easy downloads of Daymet climate data directly to your R workspace or your computer. Routines for both single pixel data downloads and gridded (netCDF) data are provided. Please use the below references when using Daymet data and the package.

To install the current stable release use a CRAN repository:

``` r
install.packages("daymetr")
library("daymetr")
```

## [phenocamr](https://github.com/khufkens/phenocamr)

![Build Status](https://travis-ci.org/khufkens/phenocamr.svg?branch=master)![codecov](https://codecov.io/gh/khufkens/phenocamr/branch/master/graph/badge.svg)![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/phenocamr)![CRAN\_Downloads](https://cranlogs.r-pkg.org/badges/grand-total/phenocamr)

Facilitates the retrieval and post-processing of PhenoCam time series. The post-processing of PhenoCam data includes outlier removal and the generation of data products such as phenological transition dates. If requested complementary [Daymet climate data](https://daymet.ornl.gov/) will be downloaded and merged with the PhenoCam data for modelling purposes. For a detailed overview of the assumptions made during post-processing I refer publications by Hufkens et al. (2018) and Richardson et al. (2018). Please cite the Hufkens et al. (2018) paper when using the package. A worked example is included below and in the package vignette.

To install the current stable release use a CRAN repository:

``` r
install.packages("phenocamr")
library(phenocamr)
```

## [phenor](https://github.com/khufkens/phenor)

![Build Status](https://travis-ci.org/khufkens/phenor.svg?branch=master)![codecov](https://codecov.io/gh/khufkens/phenor/branch/master/graph/badge.svg)

The phenor R package is a phenology modelling framework in R. The framework leverages measurements of vegetation phenology from four common phenology observation datasets combined with (global) retrospective and projected climate data.

I refer to [Hufkens et al. (2018)](
http://onlinelibrary.wiley.com/doi/10.1111/2041-210X.12970/full) for an in depth description and worked example of the phenor R package. All code used to generate the referenced publication is provided in a [separate github repository](https://github.com/khufkens/phenor_manuscript). Please refer to this paper when using the package for modelling efforts. 

To install the toolbox in R run the following commands in a R terminal

``` r
if(!require(devtools)){install.packages(devtools)}
devtools::install_github("khufkens/phenor")
library(phenor)
```
	
## [MODISTools](https://github.com/khufkens/MODISTools)

![Build Status](https://travis-ci.org/khufkens/MODISTools.svg)![codecov](https://codecov.io/gh/khufkens/MODISTools/branch/master/graph/badge.svg)

Programmatic interface to the ['MODIS Land Products Subsets' web services](https://modis.ornl.gov/data/modis_webservice.html). Allows for easy downloads of ['MODIS'](http://modis.gsfc.nasa.gov/) time series directly to your R workspace or your computer. When using the package please cite the manuscript as referenced below.

To install the development releases of the package run the following commands:

``` r
if(!require(devtools)){install.package("devtools")}
devtools::install_github("khufkens/MODISTools")
library("MODISTools")
```

## [snotelr](https://github.com/khufkens/snotelr)

![Build Status](https://travis-ci.org/khufkens/snotelr.svg?branch=master)![codecov](https://codecov.io/gh/khufkens/snotelr/branch/master/graph/badge.svg)

SnotelR is a R toolbox to facilitate easy SNOTEL data exploration and downloads through a convenient R [shiny](http://shiny.rstudio.com/) based GUI. In addition it provides a routine to extract basic snow phenology metrics.

You can quick install the package with the following commands:

``` r
if(!require(devtools)){install.package(devtools)}
devtools::install_github("khufkens/snotelr")
```

# Python packages & code

## [daymetpy](https://github.com/khufkens/daymetpy)

daymetpy attempts to fill the need for easy, integrated access to gridded daily Daymet weather data. The data are hosted by the Oak Ridge National Laboratories DAAC and accessed from [their web service](https://daymet.ornl.gov/web_services.html).

Install the package using [pip](https://en.wikipedia.org/wiki/Pip_(package_manager)) and the following command:

``` bash
pip install daymetpy
```

## [GEE subsets](https://github.com/khufkens/gee_subset)

This is a small python script to subset GEE gridded data products into time series for a given location or list of locations. This script should make it easier to subset remote sensing time series for processing external to GEE. 

This in part replaces for example the ORNL DAAC MODIS subsets or Daymet web services, but extends these to higher resolution date such as Landsat and Sentinel. More so, it should also work on all other gridded products using the same product / band syntax (e.g. the MODIS phenology product MCD12Q2 or the MODIS snow product MOD10A1). If this code made your life easier please refer to it using the Zenodo citation and DOI (see below / medallion) in any research papers. To install the code, clone the github repository.

``` bash
git clone https://github.com/khufkens/google_earth_engine_subsets.git
```

Make sure you have a working Google Earth Engine python API setup. The installation instructions can be found on the [GEE developer site](https://developers.google.com/earth-engine/python_install).

# Javascript


## [MCD10A1](https://github.com/khufkens/MCD10A1)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.162765.svg)](https://doi.org/10.5281/zenodo.162765)

The Google Earth Engine (GEE) code within this project combines MODIS Aqua - Terra snow cover products and derives snow melt - accumulation (snow phenology) dates.

The original MODIS products (MOD10A1 & MYD10A1, for the Terra and Aqua platforms respectively) as processed by the National Snow and Ice Data Center (NSIDC) have the tendency to be biased low.

My code fuses these two data streams using a maximum value approach to account for this bias. More importantly, the script also generates a derived snow phenology product where I calculate the date of snow melt (< 5% snow cover) in spring and and snow accumulation (> 5%) in autumn.

As an example the script runs a simple regression analysis to mark trends in snow melt dates, answering the question if snow melt occurs later or earlier over time. To use [the code](https://github.com/khufkens/MCD10A1/blob/master/MCD10A1.js) copy it to your GEE editor.