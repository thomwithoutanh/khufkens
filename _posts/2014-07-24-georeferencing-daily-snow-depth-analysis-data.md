---
title: Georeferencing daily CMC and IMS snow analysis data
author: Koen Hufkens
layout: post
permalink: /2014/07/24/georeferencing-daily-snow-depth-analysis-data/
categories:
  - R
  - Research
  - Software
tags:
  - arctic
  - R
  - research
  - snow
  - software
---
Recently I was in need of some snow cover data of the Arctic. The National Snow and Ice Data Centre (NSIDC) is focussed on tracking snow and ice properties, hence "the source" of snow and ice data. Although all data is freely available, figuring out how to read in these data often presents an issue for the uninitiated as it lacks easy to interpret geo-referencing information -- presenting most with a rather steep learning curve. Although you can produce statistics from the raw ascii files, which are relatively easy to reformat, it's rather hard to figure out a given snow depth statistic at any particular location.

For the Canadian Meteorological Centre (CMC) Daily Snow Depth Analysis product as well as the IMS Daily Northern Hemisphere Snow and Ice Analysis, hosted by the NSIDC, I figured out the CRS geo-referencing  details which can be concisely summarized as an extent (x-min, x-max, y-min, y-max) of:

<strong>-8405812,8405812,-8405812,8405812 (CMC)</strong>

and

<strong>-12126597,12126840,-12126597,12126840 (IMS)</strong>

and a CRS projection string:

<strong>"+proj=stere +lat_0=90 +lat_ts=60 +lon_0=10 +k=1 +x_0=0 +y_0=0 +a=6371200.0 +b=6371200.0 +units=m +no_defs" (CMC/IMS)</strong>

These two pieces of information should suffice to either hack together a bash script and some GDAL magic to georeference the data. However, I decided to go the R route this time around as I've been doing most of my image processing using<a href="http://cran.r-project.org/web/packages/raster/index.html"> the raster() package</a> lately.

<em><strong>CMC processing</strong></em>

Below you find a function in R to process a year of CMC Snow Depth Analysis Data, in ascii format, into compressed geotiffs. The ascii files were clean up by removing the header and the dates which split up the daily matrices (or maps). The preprocessing is done using bash command line tools. As such, you will need a *nix machine to successfully run the code below.

After cleaning the original ascii file the data is read into memory as one big data table, folded into a 3D array with a layer for each day of the year (DOY). Layers are renamed to a their proper day of year value using the "DOY_#" format. All this is finally written to compressed geotiff (~50 MB yr-1), roughly equal in size to the original compressed ascii data but georeferenced and accessible with GDAL compatible GIS software (e.g. QGIS / GRASS). For the details on the procedure I refer to the code embedded below.
<pre class="lang:default decode:true"># cmc conversion function
georeference_CMC_snow_data &lt;- function(filename="",geotiff=F){
  
  # load required packaages
  require(raster)     # GIS functionality
  require(lubridate)  # to detect leap years
  
  # the projection as used (polar stereographic (north))
  # latitude at natural origin is 60 degrees
  # I use the same spherical projection as the IMS product
  proj = CRS("+proj=stere +lat_0=90 +lat_ts=60 +lon_0=10 +k=1 +x_0=0 +y_0=0 +a=6371200.0 +b=6371200.0 +units=m +no_defs")
  
  # with a x / y resolution of 23815.5 and the pole at 353,353
  e = c(-8405812,8405812,-8405812,8405812)
  
  # some feedback
  cat("Converting file:\n")
  cat(paste(filename,"\n"))
  
  # grab year from filename
  no_path = basename(filename)
  no_extension = sub("^([^.]*).*", "\\1", no_path) 
  year = unlist(strsplit(no_extension,split="_"))[3]
  
  # remove the breaks in the data file due to insertion of
  # dates
  system(paste("sed '/",year,"/d'", filename ," &gt; tmp.txt",sep=""))
  
  # read the data using scan and force it to use a double format
  # this command returns a vector
  # adjust the length according to leap years
  if ( year == 1998){
    nr_layers=153
  }else{
    if (leap_year(year) == TRUE){
      nr_layers=366
    }else{
      nr_layers=365
    }
  }
  data_vector = scan('tmp.txt',skip=0,nlines=706*nr_layers,what=double())
  
  system("rm tmp.txt")
  # convert the vector into a 3D array, using aperm to sort byrow
  # this is similar to read.table(...,byrow=T) for tables
  data_array = aperm(array(data_vector,c(706,706,nr_layers)),c(2,1,3))
  
  # remove first data vector and free up some memory
  rm(data_vector);gc()
  
  # convert the 3D array to a rasterBrick
  rb = brick(data_array)
  
  # remove data array and free up some memory
  rm(data_array);gc()
  
  # assign the above projection and extent to the raster
  projection(rb) = proj
  extent(rb) = extent(e)
  
  if (year == 1998){
    names(rb) = c(paste("DOY_",213:365,sep=""))
  }else{
    # set layer names by day of year (DOY)
    if (leap_year(year) == TRUE){
      names(rb) = c(paste("DOY_",1:366,sep=""))
    }else{
      names(rb) = c(paste("DOY_",1:365,sep=""))
    }
  }
  
  # return data as geotiff or raster brick
  # keep in mind that these are rather large files
  # when returning data to the workspace
  if (geotiff==F){
    return(rb)
  }else{
    # write data to file
    filename = paste("cmc_analysis_",year,".tif",sep="")
    writeRaster(rb,filename,overwrite=TRUE,options=c("COMPRESS=DEFLATE"))
  }
}</pre>
<em><strong> IMS processing</strong></em>

The IMS processing is similar to the CMC processing although the variable header info makes things more complicated. The first part of the routine takes care of finding the starting line on which the actual data begins. The rest of the routine reads in the data line by line, splitting the long character vector in it's individual numbers and converting it to a matrix.

This matrix is converted to a raster() object which can be assigned an extent and projection. Finally, this data is written to a compressed geotiff.
<pre class="lang:default decode:true ">georeference_IMS_snow_data &lt;- function(filename="",geotiff=F){
  
  # you need the raster library to georeference
  # the image matrix
  require(raster)
  
  # Scan for the line on which "Dim Units"
  # occurs for the second time, I'll scan for the
  # first 200 lines if necessary, assuming no 
  # header will be longer than this even given
  # some changes in the header info through time
  # I also assume that the basic naming of the 
  # header info remains the same (file size etc)
  
  # the projection as used (polar stereographic (north))
  proj = CRS("+proj=stere +lat_0=90 +lat_ts=60 +lon_0=10 +k=1 +x_0=0 +y_0=0 +a=6371200.0 +b=6371200.0 +units=m +no_defs")
  
  # set extent based upon documentation info x / y resolution top left coordinate
  e = extent(-12126597.0,12126840,-12126597,12126840.0)
  
  # set string occurence to 0
  str_occurence = 0
  
  # attache and open file
  con &lt;- file(filename)
  open(con)
    for(i in 1:200){
      
      # read a line of the ascii file
      l = readLines(con, n=1)
      
      # find the occurence of "Dim Units"
      occurence = grep("Dim Units",l,value=F)
      
      # only process valid results (skip empty strings)
      if(length(occurence)!=0){
        str_occurence = str_occurence + occurence 
      }
      
      # when two occurences are found return the
      # line of the second one
      if (str_occurence == 2){
        skip_lines &lt;&lt;- i # set the global variable skip_lines to i
        break
      }
    }
  # close file
  close(con)
  
  # read in the data after the header data assumes a 1024 x 1024 matrix
  data_list = scan(filename,skip=skip_lines,nlines=1024,what=character())
  
  # split every element (a long character string) in the list into it's
  # individual characters and return to the original list object
  data_list = lapply(data_list,function(x)substring(x,seq(1,nchar(x),1),seq(1,nchar(x),1)))
  
  # unlist the list and wrap it into a matrix
  data_matrix = matrix(as.numeric(unlist(data_list)),1024,1024)
  
  # convert to a raster object
  r = raster(data_matrix)
  
  # assign the raster a proper extent and projection
  extent(r) = e
  projection(r) = proj
  
  # return the raster
  if (geotiff == F){
    return(r)
  }else{
    no_extension = unlist(strsplit(basename(filename),split='\\.'))[1]
    writeRaster(r,paste(no_extension,".tif",sep=""),overwrite=T,options=c("COMPRESS=DEFLATE"))
  }
}</pre>
&nbsp;

&nbsp;