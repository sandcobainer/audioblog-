---
layout: post
title:  "Akai MPK Mini as a Ableton Live controller using M4L"
date:   2019-01-02
desc: "Blog 5: This is a blog series chronicling my independent study work at the DX Media Lab, Kansas State University."
keywords: “sampling, ableton, live, max4live, akai, audio engineering, art, music, music technology, computer science, mpk mini, midi,"
categories: [Max]
tags: [controlsurface, max4live, akai]
icon: icon-html
---
Akai MPK Mini (currently MKII) is probably the most basic and beginner controller on market. It has served many producers starting off while making music, who later shifted to an advanced midi controller with more buttons, sliders, knobs etc. I found this Akai MPK Mini on ebay for a cheap 20 USD. This series is me documenting my Max4Live device as a control surface to control Ableton Live. I will be using the Live.API and this amazing M4L device that lets developers debug the live.api "live" within the program.

Thank you, Tyler Mazaika
**[LiveAPI-Interactive](http://tylermazaika.com/max-for-live/liveapi-interactive/)**

# **Setup AKAI MPK Mini**

The MPK Mini initially came with limited presets on the stock pads.  For this control surface , I will be mapping all Ableton Live sessions and devices only on the pads and encoders (knobs). The idea is to keep all the keys and pads free for the user to play music. To edit the existing the MPK Mini presets, you will need an editor that can edit the presets and upload them onto the MPK Mini.
Download the editor [here](http://www.akaipro.com/products/legacy/mpk-mini).
The presets are all available on my github repo ["mpkmini4Live"](https://github.com/sandcobainer/mpkmini4Live)

The USB MIDI controller is connected in the same way as any other MIDI controller. However, the AKAI MPK Mini stock presets will need to be edited using AKAI MPK Mini Editor. (inside MPKWin folder)

![AKAI MPK Mini](http://b8e57dc469f9d8f4cea5-1e3c2cee90259c12021d38ebd8ad6f0f.r79.cf2.rackcdn.com/Content_Images/akai_mpkmini_preset_editor_main.png_6b9a01d599dd9181155b81b91bba06c8.png)

**MacOS Mojave** users may not be able to run the MPK Mini editor just like me, so I've included the Windows version (.exe file). This run smoothly on Mac OS using wine. run wine MPK\ MINI\ Editor.exe from your Terminal.

Once the presets are uploaded the hardware controller is ready to go. The M4L device .amxd needs to be placed on a MIDI track with monitoring 'In'selected. This allows us to send MIDI data continuously into Max 4 Live.

On the Max patch, simple notein and ctlin are used to read the messages. All the keys and pad notes from **notein** are left untouched. I used the MIDI messages available only the control  
messages. The controller has 8 pads and 8 encoders including 2 pad banks. So that makes it *16 pads and 8 encoders* on each preset. The editor lets you upload 4 programs.  Some quick math and you can control ***64 pads and 32 encoders*** on Control Changes, independent of the keyboard and padboard notes. However, the key is to make the presets and pads intuitive so that the user is not lost while playing, looping or producing mixes.


# **Programming Max**

Initially a sub patch classifies MIDI events into 4 presets or programs setup through the Akai editor. The MIDI events are routed into one of 4 preset subpatches where all the magic happens. Patches were designed as follows:

1. Preset1: Track and Scene/Clip control:
    Pad Bank A: Select, RecordArm, Stop clips, Mute, Solo
    Pad Bank B: Select Scene/Clip, Fire 3 clips.
    Encoders: Volume, Panning, Sends, Master Volume, Master Panning

2. Preset2: Device and param control
    Encoders: Macro Bank control

Preset3 and 4 are left free for MIDI mapping. The track and scenes are accessed through the Live API with Max primarily using the live.object and live.path objects. The LiveAPI Interface is a very useful resource for prototyping and development for Ableton Live. Any parameter within the Ableton Live set can be reached using the Live API. There are smaller properties and get, set methods to affect the parameter.

For example, the volume of track 3 can be reached by the path

**path live\_set tracks 3 mixer\_device volume**. The volume slider is changed by using the ***set value $1*** message in Max where $1 is the placeholder for incoming value from the MIDI controller.
Similar patching applies for buttons and knobs. Track and Clip navigation is done by assigning 4 buttons for left, right, up and down in the clip sessions to select and fire clips.

I tried to keep the number of API calls to the minimum by reusing track and scene index calls. However, the control changes, especially the circular knobs react slower than a direct MIDI controller through Live.API. The result is a steady gradual movment of knobs in the program.

## Max Patch

<img src="{{ site.baseurl }}/static/assets/img/blog/max/mpkrecordmaxpat.png" alt="mpkrecordmaxpat.png" class="center" />

Download patch and presets from my github repo [***mpkmini4Live***](https://github.com/sandcobainer/mpkmini4Live)
