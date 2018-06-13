---
title: 'Caffe hack: outputting the FC7 layer'
author: Koen Hufkens
layout: post
permalink: /2016/06/05/caffe-hack-outputting-the-fc7-layer/
categories:
  - Research
  - Science
  - Software
tags:
  - deep learning
  - python
  - research
  - science
  - software
---
The <a href="http://caffe.berkeleyvision.org/">Caffe deep learning framework</a> has a nice set of python scripts to help automate classification jobs. However, I found the standard classifier.py output rather limited. The script does not output other data than the predicted result.

Some applications could benefit from outputting the final FC7 classification weights. These weights together with a classification key can then be used to assign different labels (semantic interpretations) using the same classification run.  Below you find my new python (classifier.py) script which outputs the FC7 layer.

This new script allows me to assign classification labels using both the SUN database and the Places205 database in one pass using the <a href="http://places.csail.mit.edu/">MIT places convolutional neural network (CNN)</a>.
<pre class="lang:python decode:true">#!/usr/bin/env python
"""
Classifier is an image classifier specialization of Net.
"""

import numpy as np

import caffe


class Classifier(caffe.Net):
    """
    Classifier extends Net for image class prediction
    by scaling, center cropping, or oversampling.

    Parameters
    ----------
    image_dims : dimensions to scale input for cropping/sampling.
        Default is to scale to net input size for whole-image crop.
    mean, input_scale, raw_scale, channel_swap: params for
        preprocessing options.
    """
    def __init__(self, model_file, pretrained_file, image_dims=None,
                 mean=None, input_scale=None, raw_scale=None,
                 channel_swap=None):
        caffe.Net.__init__(self, model_file, pretrained_file, caffe.TEST)

        # configure pre-processing
        in_ = self.inputs[0]
        self.transformer = caffe.io.Transformer(
            {in_: self.blobs[in_].data.shape})
        self.transformer.set_transpose(in_, (2, 0, 1))
        if mean is not None:
            self.transformer.set_mean(in_, mean)
        if input_scale is not None:
            self.transformer.set_input_scale(in_, input_scale)
        if raw_scale is not None:
            self.transformer.set_raw_scale(in_, raw_scale)
        if channel_swap is not None:
            self.transformer.set_channel_swap(in_, channel_swap)

        self.crop_dims = np.array(self.blobs[in_].data.shape[2:])
        if not image_dims:
            image_dims = self.crop_dims
        self.image_dims = image_dims

    def predict(self, inputs, oversample=True):
        """
        Predict classification probabilities of inputs.

        Parameters
        ----------
        inputs : iterable of (H x W x K) input ndarrays.
        oversample : boolean
            average predictions across center, corners, and mirrors
            when True (default). Center-only prediction when False.

        Returns
        -------
        predictions: (N x C) ndarray of class probabilities for N images and C
            classes.
        """
        # Scale to standardize input dimensions.
        input_ = np.zeros((len(inputs),
                           self.image_dims[0],
                           self.image_dims[1],
                           inputs[0].shape[2]),
                          dtype=np.float32)

        for ix, in_ in enumerate(inputs):
            input_[ix] = caffe.io.resize_image(in_, self.image_dims)

        if oversample:
            # Generate center, corner, and mirrored crops.
            input_ = caffe.io.oversample(input_, self.crop_dims)
        else:
            # Take center crop.
            center = np.array(self.image_dims) / 2.0
            crop = np.tile(center, (1, 2))[0] + np.concatenate([
                -self.crop_dims / 2.0,
                self.crop_dims / 2.0
            ])
            input_ = input_[:, crop[0]:crop[2], crop[1]:crop[3], :]

        # Classify
        caffe_in = np.zeros(np.array(input_.shape)[[0, 3, 1, 2]],
                            dtype=np.float32)
        for ix, in_ in enumerate(input_):
            caffe_in[ix] = self.transformer.preprocess(self.inputs[0], in_)
	#out = self.forward_all(**{self.inputs[0]: caffe_in}) # original

	# grab the FC7 layer in addition to the normal classification
	# data and output it to a seperate variable
	out = self.forward_all(**{self.inputs[0]: caffe_in, 'blobs': ['fc7']})
        predictions = out[self.outputs[0]]
	fc7 = self.blobs['fc7'].data

        # For oversampling, average predictions across crops.
        if oversample:
            predictions = predictions.reshape((len(predictions) / 10, 10, -1))
            predictions = predictions.mean(1) # column wise mean, rows = 0

	    fc7 = fc7.reshape((len(fc7) / 10, 10, -1))
            fc7 = fc7.mean(1).reshape(-1)

	# output both the classification as specified
	# by the current classifier, as the fc7 feature
	# to run on another feature matching set
        return predictions, fc7
</pre>
&nbsp;