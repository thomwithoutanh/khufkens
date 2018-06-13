---
title: Odyssey caffe-SegNet installation instructions
author: Koen Hufkens
layout: post
permalink: /2016/01/29/odyssey-caffe-segnet-installation-instructions/
categories:
  - Research
  - Science
  - Software
tags:
  - caffe
  - code
  - CUDA
  - deep learning
  - GPU
  - SegNet
---
Here I provide a simple set of bash commands and settings to get started with the <a href="http://mi.eng.cam.ac.uk/projects/segnet/tutorial.html">caffe-SegNet tutorial</a> on the Harvard Odyssey cluster and it's NVIDIA CUDA capabilities. If the below setup works you can move on and start processing your own data.

First install load all necessary modules into your ~/.bashrc file
<pre class="wrap:true lang:default decode:true crayon-selected"># SegNet required modules and 
# interdependencies
module load python/2.7.6-fasrc01
module load gcc/4.8.2-fasrc01 
module load atlas/3.10.2-fasrc02
module load cudnn/6.5.48-fasrc01
module load protobuf/20151218-fasrc01
module load cmake/2.8.12.2-fasrc01
module load gflags/2.1.2-fasrc01 
module load glog/0.3.4-fasrc01 
module load hdf5/1.8.12-fasrc08 
module load opencv/3.0.0-fasrc02 
module load lmdb/20160113-fasrc01 
module load cuda/6.5-fasrc01
module load leveldb/1.18-fasrc01 
module load boost/1.59.0-fasrc01
module load gmp/5.1.3-fasrc01 
module load mpfr/3.1.2-fasrc01 
module load mpc/1.0.1-fasrc01
module load libav/0.8.17-fasrc01
module load ffmpeg/2.3.2-fasrc01
module load libvpx/v1.3.0-fasrc01
module load xvidcore/1.3.3-fasrc01
module load libtheora/1.1.1-fasrc01
module load yasm/1.3.0-fasrc01
module load opus/1.0.3-fasrc01
module load fdk-aac/0.1.3-fasrc01
module load lame/3.99.5-fasrc01
module load x264/20140814-fasrc01
module load faac/1.28-fasrc01
module load libvpx/v1.3.0-fasrc01
module load opencore-amr/0.1.3-fasrc01
module load libass/0.11.2-fasrc01
module load fribidi/0.19.1-fasrc01
module load enca/1.15-fasrc01
module load libvorbis/1.3.4-fasrc01
</pre>
Reload your .bashrc file
<pre class="lang:default decode:true">source ~/.bashrc</pre>
Start a GPU session on the cluster
<pre class="lang:default decode:true">srun --pty --gres=gpu -p gpu -t 600 --mem 8000 /bin/bash</pre>
Then download and compile all code
<pre class="wrap:true lang:default decode:true crayon-selected"># clone the tutorial data and rename the directory
git clone https://github.com/alexgkendall/SegNet-Tutorial.git
mv SegNet-Tutorial Segnet

# move into the new directory
cd Segnet

# clone the caffe-segnet code
git clone https://github.com/alexgkendall/caffe-segnet.git

# download the Odyssey specific cmake settings
wget -q https://www.dropbox.com/s/hbhzl2bwm19vtd0/FindAtlas.cmake?dl=0 -O ./caffe-segnet/cmake/Modules/FindAtlas.cmake

# create the build directory
mkdir ./caffe-segnet/build

# move into the build directory
cd ./caffe-segnet/build

# create compilation instructions
cmake -DCMAKE_INCLUDE_PATH:STRING="$CUDNN_INCLUDE;$BOOST_INCLUDE;$GFLAGS_INCLUDE;$HDF5_INCLUDE;$GLOG_INCLUDE;$PROTOBUF_INCLUDE;$SNAPPY_INCLUDE;$LMDB_INCLUDE;$LEVELDB_INCLUDE;$ATLAS_INCLUDE;$PYTHON_INCLUDE" -DCMAKE_LIBRARY_PATH:STRING="$BOOST_LIB;$GFLAGS_LIB;$HDF5_LIB;$GLOG_LIB;$PROTOBUF_LIB;$SNAPPY_LIB;$LMDB_LIB;$LEVELDB_LIB;$ATLAS_LIB;$CUDNN_LIB;$PYTHON_LIB" ..

# compile and test all code
make all
make runtest</pre>