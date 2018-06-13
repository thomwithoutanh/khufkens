---
title: Octave 3.6.x image processing on Ubuntu Legacy
author: Koen Hufkens
layout: post
permalink: /2013/12/06/octave-3-6-x-image-processing-on-ubuntu-legacy/
categories:
  - Linux
  - Research
  - Software
tags:
  - image processing
  - linux
  - matlab
  - octave
---
If you want to install the most recent version of Octave, an open source MatLab clone, on your older Ubuntu box you are out of luck. Only the most recent repositories (&gt; 13.x) carry the latest Octave releases.

I found this <a href="http://blogs.bu.edu/mhirsch/2012/08/octave-3-6-on-ubuntu-12-04/">quick fix</a> for the problem. It involves four steps on a terminal.
<pre>sudo apt-add-repository ppa:octave/stable
sudo apt-get update
sudo apt-get install octave
sudo apt-get install liboctave-dev</pre>
To get to a fully functioning image processing toolbox run the following commands:
<pre>sudo octave</pre>
on the octave prompt
<pre>pkg -verbose install -forge general
pkg -verbose install -forge control
pkg -verbose install -forge specfun
pkg -verbose install -forge signal
pkg -verbose install -forge image</pre>
The command 'pkg list' will return a list of installed packages
<pre>pkg list</pre>
<pre>Package Name  | Version | Installation directory
--------------+---------+-----------------------
     control *|   2.6.0 | /usr/share/octave/packages/control-2.6.0
     general  |   1.3.2 | /usr/share/octave/packages/general-1.3.2
       image  |   2.0.0 | /usr/share/octave/packages/image-2.0.0
      signal  |   1.2.2 | /usr/share/octave/packages/signal-1.2.2
     specfun  |   1.1.0 | /usr/share/octave/packages/specfun-1.1.0</pre>
Finally, you load the image package using:
<pre>pkg load image</pre>
&nbsp;