---
title: 'Basic pattern matching: saturday morning hack'
author: Koen Hufkens
layout: post
permalink: /2016/05/28/basic-pattern-matching-saturday-morning-hack/
categories:
  - R
  - Science
  - Software
tags:
  - R
  - science
  - software
---
The <a href="https://www.zooniverse.org/projects/jukoch/pattern-perception">Pattern Perception Zooniverse project</a> asks Zooniversites to classify patterns based upon their (dis)similarity. Yet given the narrow scope of the problem (no rotations, same output every time) it was well worth exploring how a simple covariance based metric would perform. The input to the problem is a set of 7 images, one reference image and six scenarios to compare it to. Below you see the general layout of the image as shown on the Zooniverse website (I'll use a different image afterwards).

<img class="aligncenter size-medium_large wp-image-1281" src="/uploads/2016/05/model_run-768x576.png" alt="model_run" width="768" height="576" />

The basic test would be to calculate x features for all maps and compare the six scenarios to the reference map and record the<a href="https://en.wikipedia.org/wiki/Covariance"> covariance</a> of each of these comparisons. I then rank and plot the images accordingly. Although I'm pretty certain that any greyscale covariance metric would perform well in this case (including the raw data). However, I added a spatially explicit information based upon the <a href="http://scikit-image.org/docs/dev/auto_examples/features_detection/plot_glcm.html">Grey Level Co-occurence Matrix (GLCM)</a> features. This ensures in part the inclusion of some spatial information such as the homogeneity of the images.

When performing this analysis on a set of images this simple approach works rather well. The image below shows you the ranking (from good to worse - top to bottom) of six images (left) compared to the reference image (right) (Fig. 1). This ranking is based upon the covariance of all GLCM metrics. In this analysis map 3 seems not to fall nicely in the sequence (to my human eye / mind). However, all GLCM features are weighted equally in this classification. When I only use the "homogeneity" GLCM feature in a classification a ranking of the images appears more pleasing to the eye (Fig. 2).

A few conclusions can be drawn from this:
<ol>
 	<li>Human vision seems to pick up high frequency features more than low frequency ones, in this particular case. However, in general things are a bit <a href="https://foundationsofvision.stanford.edu/chapter-7-pattern-sensitivity/">more complicated</a>.</li>
 	<li>In this case, the distribution of GLCM features does not match human perception and this unequal weighing relative to our perception sometimes provides surprising rankings.</li>
 	<li>Overall the best matched images still remain rather stable throughout suggesting that overall the approach works well and is relatively unbiased.</li>
</ol>
Further exploration of these patterns can done with a <a href="https://en.wikipedia.org/wiki/Principal_component_analysis">principal component analysis (PCA)</a> on the features as calculated for each pixel. The first PC-score would indicated which pixels cause the majority of the variability across maps 1-6 relative to the reference (if differences are taken first). This indicate regions which are more stable or variable under different model scenarios. Furthermore, the project design lends itself to a <a href="https://en.wikipedia.org/wiki/Generalized_linear_mixed_model">generalized mixed model approach</a>, with more statistical power than a simple PCA. This could provide insights in potential drivers of this behaviour (either due to model structure errors or ecological / hydrological processes). A code snippet of the image analysis written in R using the <a href="http://azvoleff.com/articles/calculating-image-textures-with-glcm/">GLCM package</a> is attached below (slow but functional).

[caption id="attachment_1282" align="aligncenter" width="512"]<img class="wp-image-1282 size-large" src="/uploads/2016/05/map_comparison-512x1024.png" alt="map_comparison" width="512" height="1024" /> <strong>Fig 1.</strong> An image comparison based upon all GLCM features.[/caption]

[caption id="attachment_1283" align="aligncenter" width="512"]<a href="http://www.khufkens.com/?attachment_id=1283" target="_blank"><img class="wp-image-1283 size-large" src="/uploads/2016/05/map_comparison_homogeneity-512x1024.png" alt="map_comparison_homogeneity" width="512" height="1024" /></a> <strong>Fig 2.</strong> An image comparison based upon the homogeneity GLCM feature.[/caption]
<pre class="lang:r decode:true crayon-selected"># load required libs
require(raster)
require(glcm)

# set timer
ptm &lt;- proc.time()

# load the reference image and calculate the glcm
ref = raster('scenario_reference.tif',bands=1)
ref_glcm = glcm(ref) # $glcm_homogeneity to only select the homogeneity band

# create a mask to kick out values
# outside the true area of interest
mask = ref == 255
mask = as.vector(getValues(mask))

# convert gclm data to a long vector
x = as.vector(getValues(ref_glcm))

# list all maps to compare to
maps = list.files(".","scenario_map*")

# create a data frame to store the output
covariance_output = as.data.frame(matrix(NA,length(maps),2))

# loop over the maps and compare
# with the reference image
for (i in 1:length(maps)){

  # print the map being processed
  print(maps[i])

  # load the map into memory and
  # execute the glcm routine
  map = glcm(raster(maps[i],bands=1)) # $glcm_homogeneity to only select the homogeneity band

  # convert stacks of glcm features to vector
  y = as.vector(getValues(map))

  # merge into matrix
  # mask out border data and
  # drop NA values
  mat = cbind(x,y)
  mat = mat[which(mask != 1),]
  mat = na.omit(mat)

  # put the map number on file
  covariance_output[i,1] = i

  # save the x/y covariance
  covariance_output[i,2] = cov(mat)[1,2]
}

# sort the output based upon the covariance
# in decreasing order (best match = left)
covariance_output = covariance_output[order(covariance_output[,2],decreasing = TRUE),]

# stop timer
print(proc.time() - ptm)

# loop over the covariance output to plot how the maps best
# compare
png("map_comparison.png",width=800,height=1600)
par(mfrow=c(6,2))
for (i in 1:dim(covariance_output)[1]){
  rgb_img = brick(sprintf("scenario_map%s.tif",covariance_output[i,1]))
  ref = brick("scenario_reference.tif")
  plotRGB(rgb_img)
  legend("top",legend=sprintf("Map %s",covariance_output[i,1]),bty='n',cex=3)
  plotRGB(ref)
  legend("top",legend="Reference",bty='n',cex=3)
}
dev.off()</pre>