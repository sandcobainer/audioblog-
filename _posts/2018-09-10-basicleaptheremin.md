---
layout: post
title:  "Setup a basic Leap Motion theremin in Max/MSP"
date:   2018-09-10
desc: "Blog 2: This is a blog series chronicling my independent study work at the DX Media Lab, Kansas State University."
keywords: “kansas state university, leap motion, audio engineering, art, music, music technology, computer science, machine learning, media arts, research, independent study"
categories: [Art822]
tags: [leap motion, max/msp, delay theremin]
icon: icon-html
---
As the last post estabilishes the requirement for a reliable and robust sensor, this post will cover setting up a Leap Motion sensor with my workstation (MacOS High Sierra). The Leap Motion is pretty straightforward to setup in a few minutes, but setting up the Leap to send data to a programming language like Max/MSP can be tricky for the novice user.

**Wikipedia**: “[Max](https://cycling74.com/products/max/), also known as Max/MSP/Jitter, is a visual programming language for music and multimedia developed and maintained by San Francisco-based software company Cycling '74.” I used Max 7 for the entire project.

# **Setup Leap Motion and Max/MSP**
1. Install Leap Motion SDK v2.3.0 from [here](https://developer.leapmotion.com/sdk/v2/). (*Have to stick to v2.3 if using Max*)
2. Install Max 7 from [here](https://cycling74.com/downloads). You can get a free trial version for 30 days.
3. Install the Leap Motion external for Max from the [github release](https://github.com/JulesFrancoise/leapmotion-for-max/releases) by JulesFrancoise.
4. Now the “leapmotion” object should be available in Max to relay messages from an active Leap Motion controller connected via USB.
5. Open leapmotion.maxhelp patch (written by the same github developers group, IRCAM) to watch live functioning of the sensor. 

<img align="middle" src="http://ismm.ircam.fr/wp-content/uploads/2014/11/leapmotion-screenshot.jpg" alt=“leapmotion.maxhelp”>

## Sensor Data

The leapmotion.maxhelp clearly lists all the data that the leap motion detects and sends to Max. Despite being v2.3, the data is impressive and accurate in most situations. There are still problems like occlusion which are beyond the reach of this beginner setup. But, this setup provides us a lot of data to build a simple theremin quickly.

Diving into the data to make some sound. In order to synthesize a sound wave, all we need is a frequency and an amplitude of the wave. If we can map the distances of the 2 hands to volume and pitch, that should start making some interesting sounds.

## Max Patch: Basic Theremin: [git](https://github.com/sandcobainer/gesturalmusicinterfaces/tree/master/theremingesture/older-v) 
The max patch for this tutorial is named basictheremin.maxpat. I tried to keep this patch as simple as possible, the patch will open in “Presentation Mode” in Max. However to edit the patch, you’ll have unlock the patch and get into “Patching mode” (*all the modes are displayed at the bottom left corner)* .

This patch uses the **palmposition** object from the leapmotion external to calculate distance of the hand. The palmposition is the big red dot in the screenshot above. Following this, the right hand will be mapped to pitch or frequency of a sound wave and the left hand will be mapped amplitude of the sound wave. 

**Mapping** : The vertical distances of the right hand from the leapmotion are mapped to frequencies between 130Hz - 4700Hz using the ‘zmap’ object in Max. The vertical distance of the left hand is mapped to a volume slider between 0 - 127.

However the numbers alone do not generate the sound and we will need a sound generator or an oscillator. We will choose a basic sinusoidal oscillator. A simple delay effect (*tapin delay*), an audio meter, a dac~ and spectro graph to observe the waves are added and bundled into a small usable plugin in presentation mode. 

[![Smells Like Teen Spirit Theremin](http://img.youtube.com/vi/ELpzCcuoYn8/0.jpg)](https://www.youtube.com/watch?v=ELpzCcuoYn8 "Smells Like Teen Spirit")

The next post will explore other data from Leap Motion, machine learning, advanced designs and limitations of this approach.
