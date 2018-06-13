---
title: Want to get published, show me your code.
author: Koen Hufkens
layout: post
permalink: /2017/10/31/want-to-get-published-show-me-your-code/
categories:
  - R
  - Research
  - Science
  - Software
---
<blockquote>All too often one is still confronted with a statement at the end of the manuscript reading: "Code is available from the authors at reasonable request".</blockquote>
The last few years there has been a strong focus on open data and open access journals. This is in part stimulated by <a href="https://www.nature.com/news/1-500-scientists-lift-the-lid-on-reproducibility-1.19970">a reproducibility crisis in science</a>, often in the biomedical sciences. However, the strong focus on data and journal access alone is misplaced.

Many fields such as ecology, remote sensing and elsewhere rely increasingly on ever more complex software (models). Furthermore, they use ever larger amounts of data. Yet, there isn't the same demand for releasing code and / or open coding practices. All too often one is still confronted with a statement at the end of the manuscript reading: "Code is available from the authors at reasonable request".

What reasonable means is often unclear, but it clearly does not stimulate reproducibility (e.g., a critical request might not be "reasonable"). It also actively interferes with the task of reviewers who make assumptions (in good faith) that the analysis was correctly executed. However, with the amount of data (sources) used as well as the number of lines of code produced errors are far from unlikely.

 <img style="float: right;" src="/uploads/2017/10/cover-229x300.png" alt="" width="229" height="300" />

With services such as <a href="https://github.com/">Github</a> and <a href="https://www.docker.com/">Docker</a> containers there should be a requirement for any study heavy on the modelling side, and which relies on open data, to be fully reproducible if not through a small <a href="https://github.com/khufkens/phenograss-example">worked example</a> if the full dataset would be prohibitive in size or when ethically not desired.

More so, when it comes to model comparisons there should be an active effort to formalize these <a href="https://github.com/pecanproject">comparisons in community driven frameworks</a> (e.g., an R package, a python package, docker images, or a formalized workflow). Such rigorous efforts are required to truly assess model performance and quantify model errors at all levels (from source data to model structure). Alas, such efforts are few and far between in ecology, as are open and good coding practices.

The lack of this transparency is in part fueled by a gatekeeper effect. It is profitable not to share the code, as it is profitable not to share data. Not sharing code puts other scientists at a disadvantage, as similar studies or incremental advance upon the original code can't easily be made. Provided that not sharing code constitutes a breakdown of any reproducibility, and actively slows down scientific process, I'm inclined not to consider studies fit for publication without accessible source code.

<em>note 1: The active sharing of algorithms if far more common in computer science and physics.</em>

<em>note 2: I got pushback on the notion that there is a gatekeeper effect in science. Yet, the fact that a "reasonable request" is mentioned, not merely any request, implies a gatekeeper effect. It is up to the authors to decide how and who will get access to code (and applications thereof) and who doesn't. But what about licensing? Although, licensing might require citations (CC-BY), release under the same license (GPL) or prohibiting commercial applications (CC-NC), this still guarantees access to the code to begin with.</em>

&nbsp;