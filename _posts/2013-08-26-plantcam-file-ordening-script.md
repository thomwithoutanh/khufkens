---
title: PlantCam file ordening script
author: Koen Hufkens
layout: post
permalink: /2013/08/26/plantcam-file-ordening-script/
categories:
  - Linux
  - Software
  - Uncategorized
---
I recently got a copy of the data as retrieved by my PlantCam installed in Yangambi, DR Congo. Before processing could proceed I had to rename all files from the standard Wingscapes PlantCam format to a format compatible with the PhenoCam GUI.

I wrote a little script in bash that does just this job. If run within a directory of PlantCam images it will use the exif data to create the appropriate filenames and file structures as required by the Phenocam GUI. You find the code below. The code depends on the CLI exif program and takes one parameter, mainly the site name.
<pre class="lang:sh decode:true">#!/bin/bash

# convert wingscape PlantCam files
# and moves the files into the desired file structure
# for easy processing with the PhenoCam GUI or toolkit
#
# NOTE: requires a running version of linux/Mac or cygwin
# with exif installed.
#
# written by Koen Hufkens, 24/08/2013

# get a list of all wingscape files
files=`ls *.JPG`

# pick your own sitename
sitename=$1

for i in $files;
do
	# extract date and time from exif data
	date=`exif $i | grep "Date and Time" | head -n 1 | cut -d'|' -f2 | cut -d' ' -f1 | sed 's/:/_/g'`
	time=`exif $i | grep "Date and Time" | head -n 1 | cut -d'|' -f2 | cut -d' ' -f2 | sed 's/://g'`

	# construct the final filename and rename (with copy not move)
	tmp=`echo "$sitename $date $time.jpg"`
	filename=`echo $tmp | sed 's/ /_/g'`	

	cp $i $filename

done

# sort files into the correct data structure (a folder for each month)

years=`ls *.jpg | cut -d'_' -f2 | uniq`
months=`ls *.jpg | cut -d'_' -f3 | uniq`

for i in $years;
do

echo $i

# if the year directory does not exist, create it
if [ ! -d "./$i" ]; then
mkdir ./$i
fi

	for j in $months;
	do

		# if the month directory does not exits, create it
		if [ ! -d "./$j" ]; then
		mkdir ./$i/$j
		fi

		#files_to_move=`ls *_${i}_${j}_*.jpg`
		mv *_${i}_${j}_*.jpg ./$i/$j

	done
done</pre>
&nbsp;