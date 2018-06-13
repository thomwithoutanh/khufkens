---
title: Scraping Zooniverse statistics
author: Koen Hufkens
layout: post
permalink: /2015/12/21/scraping-zooniverse-statistics/
categories:
  - R
  - Research
  - Science
  - Software
tags:
  - R
  - research
  - science
  - software
  - statistics
  - zooniverse
---
In order to keep track of the <a href="http://www.junglerhythms.org">Jungle Rhythms</a> project I wanted some basic summary statistics, as shown on the<a href="https://www.zooniverse.org/projects/khufkens/jungle-rhythms"> front page of the project</a>. However, the front end API of the project does not allow these basic statistics to be pulled from a database. Furthermore, fetching all the project data can only be done once a day (to prevent heavy traffic on the database), keeping me from generating these statistics myself. Still, I want to keep track of how classifications and users change across time.

So, I wrote a web scraper in R which I run every half hour. It renders the page using <a href="http://phantomjs.org/">PhantomJS</a>, as it is a dynamic page. It then grabs the resulting html file and puts it through the rvest R package to extract all necessary (time stamped) elements and writes everything to file. It updates a file if it exists. You can find the code (an R function) below.
<pre class="lang:r decode:true " title="Zooniverse web scraper">#' Grab basic zooniverse statistics from the front page of a project
#' @param url: Location of zooniverse project
#' @param file: the name of the output file to export statistics to
#' @param path: location of the phantomjs binary (system specific)
#' @keywords zooniverse, statistics, web scraping
#' @export
#' @examples
#' with defaults returns a file called users.stats.csv
#' for the Jungle Rhythms project
#' zooniverse.info()
#' [requires the rvest package for post-processing]
#' [http://phantomjs.org/download.html]
#' 

zooniverse.info &lt;- function(url="http://www.zooniverse.org/projects/khufkens/jungle-rhythms/home",
                                  file="user.stats.csv",
                                  path="~/your.phanthom.js.location/"){
  
  # read the required libraries
  require(rvest)
  
  # grab current date and time (a time stamp)
  date = format(Sys.Date(),"%Y-%m-%d") 
  time = format(Sys.time(),"%H:%M")
    
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
                     }, 3000); // Change timeout to render page
                     }
                     });", url), con="scrape.js")

  # process the script with phantomjs / scrapes zooniverse page
  system(sprintf("%s/./phantomjs scrape.js &gt; scrape.html",path),wait=TRUE)
  
  # load the retrieved rendered javascript page
  main = read_html("scrape.html")
  
  # set html element selector (which html fields to retrieve)
  sel = '.project-metadata-stat div'
  
  # process the html file using selection and render as text
  data = html_nodes(main,sel) %&gt;% html_text()
  
  # if data is retrieved, append to user.stats.csv file
  # if this fails, you most likely need more time to render
  # the page (see timeout above)
  if (!identical(data, character(0))){
    
    # kick out description fields and convert to numeric
    data = as.numeric(data[-c(2,4,6,8)]) 
    
    # merge into dataframe
    data = data.frame(date, time, t(data))
    colnames(data) = c('date','time','registerd_users',
                       'classifications','subjects','retired_subjects')
    
    # append stats with the current date and time
    # to an already existing data file
    if (file.exists("user.stats.csv")){
      write.table(data,"user.stats.csv",quote=F,row.names=F,col.names=F,append=T)
    }else{
      write.table(data,"user.stats.csv",quote=F,row.names=F,col.names=T)
    }
  }
  
  # remove html file and javascript
  file.remove("scrape.html")
  file.remove("scrape.js")
}</pre>
&nbsp;