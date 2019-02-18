---
layout: post
title:  "webSamplr: M4L plugin to sample the web"
date:   2019-01-26
desc: "Blog 6: Blog series outlining the build and use of my Max4Live plugin webSamplr"
keywords: “sampling, ableton, live, max4live, audio engineering, art, music, music technology, computer science, internet, browser, samplers,music producers, midi,"
categories: [Max]
tags: [sampler, internet, audio sources, max4live]
icon: icon-html
---

# **webSamplr, sample anything on the web**

This plugin has been my longterm pet project. I didn't have enough knowledge to complete this project when I first had the idea 8 months ago. Dwelling through Max, Max4Live, Ableton  and JavaScript, I finally feel good about this project.

<img src="{{ site.baseurl }}/static/assets/img/blog/blog6-websamplr.png" alt="websamplr. png" class="center" />

webSamplr is an active real-time sampler that samples anything from the web. It has a built in Chrome Embedded System (CES) browser in terms of Max's jweb object. The browser is supported by a SQL Lite database of sources classified into 5 types: music, samples, radio, video, social media.

Each list populates a sublist with an index and name of the source. Clicking on a source navigates the web browser to that page. The plugin also displays the sample rate of the current sample being streamed and a spectrogram of the audio.

# **How does the sound get sampled?**
This project was done from scratch on a Macbook High Sierra. For Windows, look for audio routing alternatives like Jack audio, VB-Audio.
Max4Live devices can generate sound on a track but how can the generated sound be recorded on the track? 
Solution: use 3rd party routing software. This is the **ONLY** dependency in this robust M4L device. I explored other options such as recording audio into a buffer and dragging it into Ableton Live, but the process of dragging in audio files didn't seem intuitive and LIVE for my sampling process and playing music on the fly.

# *Audio routing flow*

The audio is intially generated in the M4L device and routed as follows. To replicate the routing flow in the image

<img src="{{ site.baseurl }}/static/assets/img/blog/routing-flow.png" alt="Routing Flow" class="center" />

- Download Soundflower for Mac, Jack Audio or VB-Audio for Windows and change system 
   audio input device to Internal Microphone
   audio output device to **Soundflower (2ch)**

- Setup Ableton Live preferences  

<img src="{{ site.baseurl }}/static/assets/img/blog/audio-preferences.png" alt="Audio Preferences" class="center" />

<img src="{{ site.baseurl }}/static/assets/img/blog/output-devices.png" alt="Output Devices in Live" class="center" />

- Drag the .amxd M4L device onto ONE AUDIO track in Ableton Live. 

- Record arm all audio tracks in Ableton that are setup to record samples. webSamplr also accesses all tracks within Ableton Live using live.api. 

## Usage

The arrows  in the device or arrow keys on the keyboard are used to navigate between tracks and scenes for specific clips, and Enter on the keyboard launches/records the selected clip in session view.

## Max Patch

The Max patch ended up being quite involved due to SQL Lite database of audio sources, javascript, jweb browser and live.api scene launching. However, there is no noticeable latency on using this plugin on an audio track. Eliminating the soundflower or audio jack dependency and making this plugin cross platform is high on my priority list.

Anyone interested in adding more audio sources to the database can hit me up on github, linkedin or reddit and I'll gladly add more sources and release updated versions for the plugin. 
I am hosting this plugin for $0+ on [**Gumroad**](https://gum.co/websamplr). Donations greater than $1 are welcome to support me

1. fix bugs
2. eliminate 3rd party dependencies and release cross platform VST/AU using JUCE 
2. expand the database 
3. roll out new features/versions biweekly (Copyright free sources, quick play and sample sequencer)

Most importantly, would support me to write more plugins for artists, producers and movie scorers.
***Always cite and pay the rightful copyrights for any sample used in your music. This tool was only written to quickly explore sounds and write music on the fly. Peace.***


## Check the plugin on my gumroad page. Cheers!

<script src="https://gumroad.com/js/gumroad.js"></script>
<a class="gumroad-button" href="https://gum.co/websamplr?wanted=true" target="_blank">Get webSamplr now</a>

