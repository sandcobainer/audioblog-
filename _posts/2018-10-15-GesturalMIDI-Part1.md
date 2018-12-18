---
layout: post
title:  "Gestural MIDI interface: Part 1"
date:   2018-10-15
desc: "Blog 4: This is a blog series chronicling my independent study work at the DX Media Lab, Kansas State University."
keywords: “kansas state university, leap motion, audio engineering, art, music, music technology, computer science, machine learning, gesture recognition, midi, audio filters, research, independent study, ableton live, mapping"
categories: [Art822]
tags: [leap motion, midi, gesture recognition, max4live, control changes]
icon: icon-html
---
*All patches for blog posts are available under the [github:older-v](https://github.com/sandcobainer/gesturalmusicinterfaces/tree/master/theremingesture/older-v) folder.*

**Can we use this interface with other instruments?**

The interface currently does not connect to any external device. All the sound generated is directly from the max patch output. But if the max patch could act as an extra MIDI input to a DAW like Ableton Live, the gestural interface can easily be used with a vareity of instruments within a DAW. These instruments are called ***virtual instruments***.

Ableton Live, the DAW has tons of virtual instruments built into the program. Also Ableton Live has support for Max patches through Max4Live or **M4L** devices. Converting a regular Max patch into an M4L device is a breeze. 
Click [here](https://www.ableton.com/en/blog/build-max-live-device-beginner-tutorials-point-blank/) for a beginner tutorial on building M4L devices.

<img align="middle" src="https://1cyjknyddcx62agyb002-web-assets.s3.amazonaws.com/mfl_update518/intro-hp.png" alt="m4ldevice.jpg" class="center"/>

For the MIDI instrument to generate MIDI events, we will need an object like midiout. Midiout acts as any other regular hardware midiout and relays the MIDI events to the connected source, which in this case would be Ableton Live.


This design builds over the same gesture recognition model used in the [previous post](). However the gestures are used to create MIDI notes and control changes.
MIDI events can be notes or control changes. 

1. Notes usually are synonymous to button presses, like the keys of the piano.
2. Control changes on the other hand are continuous values between 0 - 127 like the rotation of a knob

# **Use gestures for notes and control changes**

The 7 existing gestures can be used to play invidual notes and CC's if they are mapped geospatially. That is the interaction area of the device is divided into quadrants and 4 notes are placed in 4 quadrants along the **X-Z** plane. 

<img src="http://blog.leapmotion.com/wp-content/uploads/2014/07/leap-motion-interaction-area.png" alt="leapinteraction_area.png" class="center">

We now have 4 MIDI notes on 4 quadrants of a single gesture. Similarly adding 4 notes each for gestures 1, 2 and 3 gives us 12 MIDI notes to play with. These notes can be used to play a note, trigger a sample, trigger an effect or a track within a Digital Audio Workstation or can be routed out to play a MIDI sequencer or synthesizers. *MIDI allow the artist to creatively taylor a new instrument or express a new sound. Applications of MIDI are endless. [Here](https://www.musicradar.com/news/tech/7-crazy-but-cool-midi-controllers-586479) are some amazing MIDI projects*.

## **Control Changes**
Control Changes were emulated by tracking the Z-axis value of the "palmposition" object. As the palm moves forward or backward in the Z-axis, the movement is mapped between 0-127 of a CC value. As the user moves on the X-axis, 3 sliders can be interacted with based on the movement. Thus, each of the Gestures 4, 5, 6 have 3 sliders each, giving us 9 sliders to interact with.

## **Pitch Bend and Tremolo**
By calculating the roll and pitch of the hand, the hand movements can be mapped to control pitch bend and tremolo simultaneously. These 2 functions were mapped exclusively only to Gesture 7 which is the open palm gesture. The open palm is the to go safety gesture to stop all the midi notes or CCs.

***It's important not to mistake pitch of the hand with pitch of the sound or note. Pitch and Roll here refer to the movement of the hand. More on this [here](https://howthingsfly.si.edu/flight-dynamics/roll-pitch-and-yaw)***


## *Presentation Mode*
The overall instrument is very effective when mapped to a virtual instrument in a DAW. Also the theremin is integrated along with a MIDI keyboard on 2 different channels as show in the image below. The corresponding max patch is listed as **gestural-midi-interface.maxpat** under ***[github:theremingesture/older-v](https://github.com/sandcobainer/gesturalmusicinterfaces/tree/master/theremingesture/older-v)***

<img src="{{ site.baseurl }}/static/assets/img/blog/art822/blog4presentation.png" alt="blog4.png" class="center" />

The next post (***Part2***) will cover more about how to visulize these different modes to a visualization and route the MIDI event data into Ableton Live through a virtual 'midiport'.