---
layout: post
title:  "Magenta NSynth-fastgen"
date:  2019-02-12
desc: "Blog 8:Synthesizing sound with a Neural Network"
keywords: “google magenta, tensorflow, neural network, ai, deep learning, python, google, audio engineering, art, music, music technology, computer science, internet, artificial intelligence, machine learning, music production"
categories: [Max]
tags: [google magenta,tensorflow, nsynth-fastgen, python]
icon: icon-html
---

# **Synthesizing sound with a Neural Network**

[**Magenta Tensorflow**](https://magenta.tensorflow.org/) is Google's open-source research project to explore the use of machine learning in creative spaces.

## Magenta NSynth

NSynth is the first neural audio synthesizer designed to aid the creative process of synthesizing new sounds from existing samples. Google Magenta's initial project NSynth features a 

- huge dataset of musical notes
- a Wavenet-styled auto encoder

The project was made extremely accessible with a model of pretrained sounds with some great features like interpolation. Google also came up with an open-source Max For Live plugin within Ableton Live with the pre-trained sounds. However, encoding/decoding your own sample on this project took > 8 hrs for a 6 second sample. 

## Nsynth - fastgen 

However, some music technologists like Parag Mittal came along and figured out make the decoder faster. The result of which is nsynth-fastgen. This blog covers how to use the fastgen model to encode your own samples in detail including installing, building and setting up file directories. This is aimed at beginner to intermediate music producers, artists looking to explore sound programmatically.

## Setup Conda environment and install dependencies

<img src="{{ site.baseurl }}/static/assets/img/blog/magenta/conda-env.png" alt="Conda Environment" class="center" />

Install Anaconda3 from this [*link*](https://www.anaconda.com/distribution/). Download the latest Python3 (**recommended**). I installed the Anaconda Navigator for a clear interface. 

- Setup environment with Python 2.7

- Open Terminal in this new environment and install magenta. Pip2 for python 2.7 is already installed, so you can directly install Magenta (v0.1.15 or greater) and Flask. This will download and install all dependencies/requirements for this project to run.

```
pip install magenta
pip isntall matplotlib
conda install jupyter
```
Installing the Jupyter notebook locally inside this environment is recommended.

## Setting up the project for generating sounds

There will be run time warnings in this project specific to numpy and it's deprecated versions, but the project will run perfectly fine and in time.

Before we start generating sounds, clone my github repo to run this experiment in a convenient way. 

* Clone the magenta-demos repo with 

```git clone https://github.com/sandcobainer/nsynth-fastgen.git ```


I decided to organize the file directories in the project as follows

- wavenet-ckpt : contains model downloaded from [Google Magenta downloads](http://download.magenta.tensorflow.org/models/nsynth/wavenet-ckpt.tar)
Download this file manually and place it inside the nsynth-fastgen repo before starting the run. I couldn't upload this in the github repo due to size.

- audio folder contains the samples fed as input to the model or the samples to be encoded. The generated audio files will be saved under this same folder with a postfix of '_gen.wav'.

Follow along with Nsynth.ipynb from here to understand each step of the process. I have made small changes to the existing setup of this .ipnyb to help easy usage and consistent 

## Run Jupyter 

- To run the jupyter notebook, type 
```jupyter notebook``` from Terminal. Make sure you're in the root directory of the 'nsynth-fastgen'

Jupyter opens up a notebook in your browser. Open Nsynth.ipynb and run each code block.

- After encoding you can listen to the generated sample either from the file system or within .ipynb.