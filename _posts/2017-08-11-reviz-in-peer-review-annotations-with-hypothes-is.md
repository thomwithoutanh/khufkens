---
title: 'reviz.in &#8211; peer-review annotations with hypothes.is'
author: Koen Hufkens
layout: post
permalink: /2017/08/11/reviz-in-peer-review-annotations-with-hypothes-is/
categories:
  - Research
  - Science
tags:
  - journals
  - peer-review
  - publishing
  - research
  - science
  - software
---
A few months ago I was flooded with review requests. And I figured that it might be time to look around for solutions and code something up allow me to annotate peer-review PDFs easily,  and generate a review report with a click of a button (<a href="https://www.elsevier.com/editors-update/story/peer-review/peer-review-challenge-winners-unveiled">as proposed years ago</a>).

Enter,<a href="http://hypothes.is"> Hypothes.is</a>. A few years ago this initiative started to facilitate the semantic or annotated web. A way to annotate web pages separate from the original creator.  Hypothes.is did exactly what I needed to <a href="https://web.hypothes.is/blog/annotating-pdfs-without-urls/">annotate any given PDF</a> (also locally stored items). However, I could not extract the data easily in a standardized way. In addition, the standard mode for the Hypothes.is client is a public one, with <a href="https://web.hypothes.is/creating-groups/">personal groups being private</a>. In short, although the whole framework had all the pieces the output wasn't optimal for peer-review, if not dangerous to reputations when accidentally leaking reviews to the web.

As such, I created <a href="http://reviz.in">Reviz.in</a>, a simple hack of the original Hypothes.is client and Google Chrome extension which makes sure you can't escape the group which holds your peer-review revision notes and generates a nice review report (see image below). In addition, I added a fancy icon and renamed the original labels (not consistently however), to differentiate the original interface from my copy to avoid confusion. I hope over time this functionality will be provided by the original Hypothes.is client, in the mean time you can read more on the installation process on the Reviz.in website:

<a href="http://reviz.in">http://reviz.in </a>

or download

the <a href="https://chrome.google.com/webstore/detail/revizin-peer-review-with/ihhpfkkjjplhcalchafaldccnlcmdcpa">Google Chrome Extension</a>.

I hope this simple hack will help people speed up their review process as to free up some time. I also hope that publishers will take note, as the lack of their innovation on this front is rather shameful.

<a href="http://www.khufkens.com/2017/08/11/reviz-in-peer-review-annotations-with-hypothes-is/revizin/" rel="attachment wp-att-1635"><img class="aligncenter size-large wp-image-1635" src="/uploads/2017/08/revizin-1024x640.png" alt="" width="525" height="328" /></a>