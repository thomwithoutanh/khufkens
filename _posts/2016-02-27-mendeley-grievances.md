---
title: Mendeley grievances and quick hack
author: Koen Hufkens
layout: post
permalink: /2016/02/27/mendeley-grievances/
categories:
  - Op-Ed
  - Research
  - Software
tags:
  - Mendeley
  - op-ed
  - reference manager
  - software
---
I was recently reminded by <a href="https://www.mendeley.com/">Mendeley </a>that I ran out of 'free' space on my account (i.e. 2.Gb of pdf storage). As a previous paying customer (who reverted back to a free account as they bumped it from 500mb free to 2Gb) I was more than happy to pay again, so I thought.

However, in the 3 years since I last payed, their storage offer has not kept up with the times. For a \\$55/yr you now get a WHOPPING - 5Gb - of pdf storage space, a Pro plan goes for \\$110/yr and an impressive 10Gb, while unlimited storage will cost you \\$165/yr! Given that Mendeley is now owned by Elsevier, a dominant if not the biggest scientific publisher around, their database of pdfs can easily be reduced by an order of magnitude (as most files are already stored by Elsevier!). Given this large overlap their storage overhead should be smaller than before their acquisition by Elsevier, warranting a price cut not a price increase relative to other / previous services. In comparison, a Dropbox account costs me \\$100/yr for 1Tb! Even better deals are possible with a Google Drive account (however I like Dropbox's integration and Linux support better).

In short, given the current state of cheap storage these price points are less than impressive, and it reeks of profit maximization using some user statistics and marketing parameters (at which price point are people likely to pay for  a given feature). Sadly, I'm of the opinion the charge does not reflect the true value of the service.

As such, for anyone who has cloud storage and does not want to pay extortion prices for pdf storage:
<ul>
 	<li>just disable your pdf syncing feature (click 'Edit Settings' next to the All Documents tab title)</li>
 	<li>create a soft link between wherever your pdf's used to be stored and a location within your cloud storage.</li>
</ul>
An example below for the standard configuration on OSX:
<pre class="lang:sh decode:true crayon-selected"># move all your files to your cloud storage (e.g. Dropbox)
# on OSX your Mendeley Desktop folder in Documents stores all your
# pdfs

mv ~/Documents/Mendeley Desktop ~/Dropbox

# now create a soft link between the new folder and where the folder
# used to be

ln -s ~/Dropbox/Mendeley\ Desktop ~/Documents/Mendeley\Desktop

# Migrate the database to your Dropbox (for convenience)
# the link below if for linux, look up the locations for Mac
# and Windows on the Mendeley website
ln -s ~/Dropbox/Mendeley\ Ltd./ ~/.local/share/data/Mendeley\ Ltd.

# now do this for all your computers. Your database will keep up to date
# through Mendeley (and Dropbox), while your files keep in sync
# through your cloud storage service</pre>
The above hack will allow you to keep your references in sync using the Mendeley database, and platform while at the same time keeping your files in sync through your cloud storage service. This way you can bypass their rather questionable business model.

Given this hack and an ironic twist of fate they now miss out on my \\$55/yr. If they would have provided 10Gb I would have payed, as the markup from my current quota is significant and a worthwhile upgrade. Sadly, I now have to resort to tricks, albeit legal, to keep the same functionality (a basic reference manager).

I guess this is one example of software as a service gone wrong. I hope that they will change their business model in the future, so I can become a paying customer again.

DISCLAIMER: You will lose the ability to read your pdf's from within the Mendeley app on iOS or Android (but you could still do so using e.g. the Dropbox app)!

&nbsp;