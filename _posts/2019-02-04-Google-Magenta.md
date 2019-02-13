---
layout: post
title:  " Synthesizing sound with a Neural Network: Magenta NSynth-fastgen"
date:  2019-02-04
desc: "Blog 7: Setup guide for Magenta Tensorflow ai-jam-js project in Ableton Live"
keywords: “google magenta, tensorflow, javascript, ableton, live,max, audio engineering, art, music, music technology, computer science, internet, artificial intelligence, machine learning,music production, midi, ai-duet"
categories: [Max]
tags: [google magenta,tensorflow, ai-jam, ableton live]
icon: icon-html
---

# **Synthesizing sound with a Neural Network: Magenta NSynth-fastgen**

[**Magenta Tensorflow**](https://magenta.tensorflow.org/) is Google's open-source research project to explore the use of machine learning in creative spaces.

The Magenta team came up with a great project at the NIPS-2016, an AI that jams with the user in call and response style. [**Here**](https://experiments.withgoogle.com/ai-duet) is the online model of the project. 

The git repo [*magenta-demos*](https://github.com/tensorflow/magenta-demos) has all the projects documented in detail for beginner users. 

<img src="{{ site.baseurl }}/static/assets/img/blog/magenta/magenta-demos.png" alt="Magenta Demos" class="center" />

The most promising demo was hands down [**AI-Jam-Ableton**](https://github.com/tensorflow/magenta-demos/tree/master/ai-jam-ableton). The project integrated a famous DAW that is sturdy and known world-wide. After 2016 the project was opensourced and no real work went into developing it further.

The AI-Ableton-Jam uses a Max patch on an Ipad running MIRA as front end to control  the model. The project also came with a Ableton Live project. However, running this project on Ableton Live 10 and Max 8.0.3 in 2019 turned out to be cumbersome. 

Ai-jam-js project which is hosted on the google chrome browser using Flask is another promising option. Listed below are the build instructions including Ableton Live routing. 

Specifications: Macbook Pro High Sierra, Ableton Live 10
# Set up Conda environment and install requirements

<img src="{{ site.baseurl }}/static/assets/img/blog/magenta/conda-env.png" alt="Conda Environment" class="center" />

Install Anaconda3 from this [*link*](https://www.anaconda.com/distribution/). I installed the Anaconda Navigator for a clear interface. 

* Open Terminal in this new environment and install magenta. Pip2 for python 2.7 is already installed, so you can directly install Magenta (v0.1.15 or greater) and Flask. This will download and install all dependencies/requirements for this project to run.

```
pip install magenta
pip install Flask
```

* Install Node.js with brew

```
(py27) bash-3.2$ brew install node
(py27) bash-3.2$ node -v
v10.15.1
(py27) bash-3.2$ npm -v
6.4.1 
```


# Download and build Magenta ai-jam-js

* Clone the magenta-demos repo with 

```git clone https://github.com/tensorflow/magenta-demos.git```

* Run the following from the directory of ai-jam-js

```
cd static
npm install
node_modules/.bin/webpack
```

* Modify ai-jam-js/server/server.py to point to port number 4002. Simply replace localhost:8080 with localhost:4002.


* Modify the RUN\_DEMO.sh to simplify the input-output ports for the magenta model. The new script reads piano input on IAC Driver IAC Bus 1 and drums on IAC Driver IAC Bus 2. (we will create these 2 virtual midi ports in Audio MIDI setup on Mac). Magenta's output will be read on 'magenta_out' in the Ableton Live MIDI track. ***This model eliminates the use of Max completely.***

```
#!/bin/bash

python maybe_download_mags.py

cd server
python server.py &
WEB_SERVER=$!
cd ..

midi_clock\
  --output_ports="magenta_clock" \
  --qpm=120 \
  --channel=0 \
  --clock_control_number=1 \
  --log=INFO &
MIDI_CLOCK=$!

magenta_midi \
  --input_ports="IAC Driver IAC Bus 2,magenta_clock" \
  --output_ports="magenta_out" \
  --bundle_files=./drum_kit_rnn.mag\
  --qpm=120 \
  --allow_overlap=true \
  --playback_channel=9 \
  --enable_metronome=false \
  --passthrough=false \
  --clock_control_number=1 \
  --min_listen_ticks_control_number=3 \
  --max_listen_ticks_control_number=4 \
  --response_ticks_control_number=5 \
  --temperature_control_number=6 \
  --generator_select_control_number=8 \
  --loop_control_number=10 \
  --panic_control_number=11 \
  --mutate_control_number=12 \
  --log=INFO &
MAGENTA_DRUMS=$!

magenta_midi \
  --input_ports="IAC Driver IAC Bus 1,magenta_clock" \
  --output_ports="magenta_out" \
  --bundle_files=./attention_rnn.mag,./pianoroll_rnn_nade.mag,./performance.mag \
  --qpm=120 \
  --allow_overlap=true \
  --playback_channel=0 \
  --enable_metronome=false \
  --passthrough=false \
  --generator_select_control_number=0 \
  --clock_control_number=1 \
  --min_listen_ticks_control_number=3 \
  --max_listen_ticks_control_number=4 \
  --response_ticks_control_number=5 \
  --temperature_control_number=6 \
  --generator_select_control_number=8 \
  --loop_control_number=10 \
  --panic_control_number=11 \
  --mutate_control_number=12 \
  --log=INFO &
MAGENTA_PIANO=$!

trap "kill ${WEB_SERVER} ${MIDI_CLOCK} ${MAGENTA_PIANO} ${MAGENTA_DRUMS}; exit 1" INT

sleep 20

open -a "Google Chrome" "http://localhost:4002"

wait

```


# Setup IAC MIDI Drivers (Internal MIDI routing)

Setup internal MIDI routing buses. On Mac, go to Audio MIDI setup -> Window -> Show MIDI studio -> IAC Driver. Make sure your device is online.  You will want to set up two buses (for piano/drum input) with device name "IAC Driver" and bus names:

* IAC Bus 1
* IAC Bus 2

They will appear in Ableton Live as IAC Driver (IAC Bus 1) and IAC Driver (IAC Bus 2).


# Setup Ableton Live

Download and open NIPS\_2016\_Demo.als from [NeurIPS202](https://github.com/tnn1t1s/NeurIPS202). In this template, we shall get rid of the 3 sync tracks under MIDI Routing and the 2 Magenta -> Drums, Keyboard tracks.

* Go to /ai-jam-js/ and run the script setup earlier. 
```
sh RUN_DEMO.sh
```

* Inside Ableton Live project, setup the IO for each track as follows. Carefully select MIDI From and To sources. 'magenta_out', IAC Driver (IAC Bus 1), IAC Driver (IAC Bus 2) will pop up. I setup my MPK Mini for keyboard and Launchpad for drums. Replace them with your own hardware controllers.

<img src="{{ site.baseurl }}/static/assets/img/blog/magenta/live-track-routing.png" alt="MIDI routing" class="center" />


* Make sure the virtual midi tracks have 'Track' turned on in Live Preferences. 

<img src="{{ site.baseurl }}/static/assets/img/blog/magenta/midi-track-preferences.png" alt="MIDI Preferences" class="center" />


# Demo
Open [localhost:4002](http://localhost:4002) in Google Chrome and jam out. If the browser isn't opened, the model will still play with default settings. Mute the website sound and play the audio only through Live track.

<img src="{{ site.baseurl }}/static/assets/img/blog/magenta/demo.png" alt="Final demo" class="center" />

***Git Issue to be tracked***: The webpage at localhost:4002 JS keys don't work sometimes, especially when the model loads up the second time. You can still play through the MIDI keyboard setup in Live, but the notes played will only be audible, not visible. 

