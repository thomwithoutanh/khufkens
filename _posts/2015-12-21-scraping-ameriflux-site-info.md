---
title: Scraping Ameriflux site info
author: Koen Hufkens
layout: post
permalink: /2015/12/21/scraping-ameriflux-site-info/
categories:
  - Climate
  - Research
  - Science
  - Software
tags:
  - ameriflux
  - climate
  - R
  - research
  - science
  - software
---
On the flight home from AGU 2015 I realized that the same code that I used to scrape <a href="http://www.khufkens.com/2015/12/21/scraping-zooniverse-statistics/">Zooniverse</a> statistics could easily be changed to grab the site summary data from the<a href="http://ameriflux.lbl.gov/sites/site-list-and-pages/"> Ameriflux LBL page</a>. As with the Zooniverse code, it relies on external <a href="http://phantomjs.org/">PhantomJS</a> binaries.

The function returns a data frame with all scraped data (site names, lat/long, altitude etc...). Errors in the table are due to errors in the original data, not the conversion (mainly start and end dates).

I'll use this function in combination my <a href="https://bitbucket.org/khufkens/ameriflux-download-tool">Ameriflux download tool</a> to provide easier sub-setting of the data. Keep an eye on my blog for upcoming updates to my Ameriflux download tool.
<pre class="lang:r decode:true" title="Ameriflux site summary scraping">#' Grabs the ameriflux site table from the LBL site
#' @param url: Location of the Ameriflux site table
#' @param path: location of the phantomjs binary (system specific)
#' @keywords Ameriflux, sites, locations, web scraping
#' @export
#' @examples
#' # with defaults, outputting a data frame
#' df &lt;- ameriflux.info()
#' [requires the rvest package for post-processing]
#' http://phantomjs.org/download.html

ameriflux.info &lt;- function(url="http://ameriflux.lbl.gov/sites/site-list-and-pages/",
                           path="~/my.phantom.js.path/"){
  
  # read the required libraries
  require(rvest)
  
  # subroutines for triming leading spaces
  # and converting factors to numeric
  trim.leading &lt;- function (x)  sub("^\\s+", "", x)
  as.numeric.factor &lt;- function(x) {as.numeric(levels(x))[x]}
  
  # write out a script phantomjs can process
  # change timeout if the page bounces, seems empty !!!
  writeLines(sprintf("var page = require('webpage').create();
                     page.open('%s', function (status) {
                     if (status !== 'success') {
                     console.log('Unable to load the address!');
                     phantom.exit();
                     } else {
                     window.setTimeout(function () {
                     console.log(page.content);
                     phantom.exit();
                     }, 3000); // Change timeout to render the page
                     }
                     });", url), con="scrape.js")
  
  # process the script with phantomjs / scrapes zooniverse page
  system(sprintf("%s/./phantomjs scrape.js &gt; scrape.html",path),wait=TRUE)
  
  # load html data
  main = read_html("scrape.html")
  
  # set html element selector for the header
  sel_header = 'thead'
  
  # Extract the header data from the html file
  header = html_nodes(main,sel_header) %&gt;% html_text()
  header = unlist(strsplit(header,"\\n"))
  header = unlist(lapply(header,trim.leading))
  header = header[-which(header == "")]
  
  # set html element selector for the table
  sel_data = 'td'
  
  # process the html file and extract stats
  data = html_nodes(main,sel_data) %&gt;% html_text()
  data = matrix(data,length(data)/length(header),length(header),byrow=TRUE)
  df = data.frame(data)
  colnames(df) = header
  
  # reformat variables into correct formats (not strings)
  # this is ugly, needs cleaning up
  df$SITE_ID = as.character(df$SITE_ID)
  df$SITE_NAME = as.character(df$SITE_NAME)
  df$TOWER_BEGAN = as.numeric.factor(df$TOWER_BEGAN)
  df$TOWER_END = as.numeric.factor(df$TOWER_END)
  df$LOCATION_LAT = as.numeric.factor(df$LOCATION_LAT)
  df$LOCATION_LONG = as.numeric.factor(df$LOCATION_LONG)
  df$LOCATION_ELEV = as.numeric.factor(df$LOCATION_ELEV)
  df$MAT = as.numeric.factor(df$MAT)
  df$MAP = as.numeric.factor(df$MAP)
  
  # drop double entries
  df = unique(df)
  
  # drop first row (empty)
  df = df[-1,]
  
  # remove temporary html file and javascript
  file.remove("scrape.html")
  file.remove("scrape.js")
  
  # return data frame
  return(df)
}</pre>
&nbsp;