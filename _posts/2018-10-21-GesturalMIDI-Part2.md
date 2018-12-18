---
layout: post
title:  "Gestural MIDI interface: Part 2"
date:   2018-10-15
desc: "Blog 4: This is a blog series chronicling my independent study work at the DX Media Lab, Kansas State University."
keywords: “kansas state university, leap motion, audio engineering, art, music, music technology, computer science, machine learning, gesture recognition, midi, audio filters, research, independent study, ableton live, mapping"
categories: [Art822]
tags: [midi, max4live, abletonlive]
icon: icon-html
---
*All patches for blog posts are available under the [github:older-v](https://github.com/sandcobainer/gesturalmusicinterfaces/tree/master/theremingesture/older-v) folder.*

**What do we have so far?**

We have a robust gestural interface sending out MIDI signals that can be mapped to any other virtual instrument or interface. However, the medium to transmit MIDI signals has not been setup. This post explains how to setup a midiout object and have it detected within Ableton Live. Also, the layout of the instrument will be strengthened and made more intuitive for the novice user. 

## **MIDI OUT** 

<img src="https://www.elmvideotechnology.com/store/image/cache/catalog/midi/input-output-mio/MIO-fnt-lrg-midi-input-output-module-500x500.jpg" alternate="midiout.jpg" class="center"/>

MIDI out is a standard component of any MIDI instrument to transmit MIDI events over the years. Max/MSP provides a very useful object for transmitting MIDI called "midiout". The midiout object usually has a source to relay the signals on. All available sources can be reached by double clicking on the 'midiout'. For this instrument, we will be using the 'fromMax1' output source in midiout. It can be selected in Max as shown below. In Ableton Live, the same source fromMax1 will be chosen under the 'MIDIFrom' under track inputs.  More on this on the Cycling'74 [website](https://docs.cycling74.com/max7/vignettes/max_and_other_apps).

For the MIDI instrument to generate MIDI events, we will need an object like midiout. Midiout acts as any other regular hardware midiout and relays the MIDI events to the connected source, which in this case would be Ableton Live.

<img src="https://docs.cycling74.com/max7/vignettes/images/max_and_other_apps4.png" alternate="maxmidiout.png" class="center"/>

# **Patch Layout**

The patch was designed to broadcast MIDI messages on 2 channels. One channel is for attaching a keyboard and using the gestures are used only for the knobs and pitch bends. This channel is activated by pressing the 'keyb' button. There are total of 12 notes and 9 CC's that can be controlled in this mode along with the keyboard notes simultaneously. The keyboard mode lets the user play notes and pitch bend or add tremolo by the movement of the left hand hovering over the Leap Motion sensor.

<img src="{{ site.baseurl }}/static/assets/img/blog/art822/blog5keyb.png" alt="blog5keyboard.png" class="center" />

Channel Two allows the user to play notes is a Theremin styled synth device. The movement of the palm across X-axis plays notes. 5 different gestures are used to play chord shapes.  (*The design is limited to triads currently*)
The possible triads are Major, Minor, Augmented and Diminished. The free gesture plays a simple note. This feature when used with an arpeggiator  is very useful to play flowing transitions in music by simply moving the hand and forming gestures as needed.
*Given Ableton Live's extensive library of arpeggiators and MIDI effects, this is a very exciting tool to explore complex melodies and sequences*

<img src="{{ site.baseurl }}/static/assets/img/blog/art822/blog5theremin.png" alt="blog5theremin.png" class="center" />

# Presentation UI

Both modes have a gesture visualizer, a MIDI note visualizer, a Tremolo Button, a Pitchbend slider. The pitchbend parameters can be controlled with the invidual knobs. The overall output bar at the bottom shows all the midievents going out of the system. 

**Start Up**: The artist opens the instrument, selects the mode and has to read and train the machine learning model. The machine learning model needs to be used in the following order. 

1. train : set in train mode
2. READ : read gesture data from file setup
3. DUMP : dump data into model
4. START : start model training (*takes around 20 seconds to train*)
5. map : map mode to recognize gestures and play!

Will be playing with this till I make some good sounds.  addio~