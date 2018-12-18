---
layout: post
title:  "Multi Wave Synthesizer with Gesture Recognition"
date:   2018-09-24
desc: "Blog 3: This is a blog series chronicling my independent study work at the DX Media Lab, Kansas State University."
keywords: “kansas state university, leap motion, audio engineering, art, music, music technology, computer science, machine learning, gesture recognition, synthesizer, audio filters, research, independent study"
categories: [Art822]
tags: [leap motion, synthesizer, gesture recognition,filters]
icon: icon-html
---
**What do we have so far?**

We have a basic theremin device that works robustly calculating palm distances to control pitch and volume on each hand. However, the device is very limiting as note transitions and accuracy are very difficult to master on the instrument. We could make it better by adding simple note visualizations but the sound of the device never changes. What can be done to shape the sound much like a synthesizer? 

<img align="middle" src="https://cdn.pixabay.com/photo/2015/08/26/10/03/synthesizer-908298_1280.jpg" alt="leapmotion.maxhelp" class="center"/>

I tried emulating the basic functionality of wave shape forming with gestures on a theremin. This post (*long one*) gives a detailed rundown of my max patch “GestureWaveTheremin”. The instrument is a live synthesizer that generates and shapes wave forms while the user is playing the theremin. The instrument allows the player to control the volume of a specific waveform by selecting a gesture. 

The max patch has a similar structure as the previous patch. Leap Motion sends palm distances for volume and pitch. A gesture recognition patch(*p left-gesture*) is used to select a specific gesture or wave form and manipulate the amplitude of that waveform. (***Will be explained in detail***)

The left hand will control the ampltiude sliders and the right hand will control the frequencies/notes. The patch also accomodates 6 octaves of notes by placing 3 columns of 24 notes on the right hand.


# **Gesture Recognition using Machine Learning**
The problem of teaching a system to recognize gestures can be solved with multiple approaches. This problem is a classification problem. Depending on the complexity of the problem, the solution can be chosen. If the classification involves simple finger count or a fist/ palm detection, computer vision algorithms that process matrix data (OpenCv, Matlab) are powerful enough to solve the problem. However, we eventually want to classify complex gestures and have the artist map creative gestures for various parameters. For this purpose, Machine Learning is a powerful and efficient tool.

<img src="{{ site.baseurl }}/static/assets/img/blog/art822/left-gesture.png" alt="left-gesture" class="center">

This patch is the machine learning model for gesture recognition. I chose the Support Vector machines model for efficient classification using less amount of data. To read more about SVM, click [here](https://medium.com/@LSchultebraucks/introduction-to-support-vector-machines-9f8161ae2fcb). The data is collected in a collection or *coll* object in Max and fed to the *ml.svm* object from the *ml-lib* package. The [*ml-lib*] package is a collection of machine learning algorithms and frameworks within Max/MSP. The patch has a train and a map phase. During the train phase, the SVM model fits the data and during the map phase, the SVM sends out gestures of the left hand out through the outlet.

*The fact that we can add gestures and change the way an instrument behaves adds an artistic choice to the musician.*

## **Synthesis Patch**
<img src="{{ site.baseurl }}/static/assets/img/blog/art822/gesturewavetheremin.jpg" alt="gesturewavetheremin.jpg" class="center" />

The patch looks complicated but is simple in functioning. Pitch generated from the "pitch-gen" subpatch is visualized as a MIDI note and sent into 4 waveform oscillators. These 4 oscillators will genereate 4 waves and merge the ways to together. The waves generated are modulated with a classic Low Frequency Oscillator. 

The wave forms have a gain slider that controls and mixes the amount of each wave. These 4 sliders are controlled with the left hand. **Gestures 1-4** will be used to control the amplitude of each wave being mixed. The generated waves are further mixed with a noise generator (VCO1 which feeds in white noise or pink noise). **Gestures 5 and 6** will control the noise mixers and the open palm (***Gesture 7***) will control the overall gain of the system (***right bottom corner***). 

The whole synthesized finally passes through a filter module which has 4 commonly used filters, *Low Pass, High Pass, Band Pass and Notch Pass*, a final spectograph to view the wave being played out with a gain slider connected to dac~

## *Presentation Mode*
Packing the whole design into a simplified presentation mode helps the user to easily interact with the system. I added a few knobs and UI labels to make it interactive and easy for the player to understand the gestures. 

<img src="{{ site.baseurl }}/static/assets/img/blog/art822/blog3presentation.png" alt="blog3.png" class="center" />

This gesture recognition model gives us a way to interact with the instrument live mapping 7 gestures to 7 sliders in the system. However, there is inaccuracy in recognizing a gesture for 20 - 500ms which causes unexpected changes in sounds. Overall, the patch is an interesting instrument to play and experiment with if synth style audio synthesis and theremin were brought together. *Ideal for movie/background scores?*

The patch "waveformTheremin.maxpat" can be downloaded from my github repo: [**gesturalmusicinterfaces**](https://github.com/sandcobainer/gesturalmusicinterfaces/tree/master/theremingesture/older-v).