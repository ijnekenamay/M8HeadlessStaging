# M8 Headless Staging

![Design Sketch](https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/head.jpg)
This is a DIY project to push the [Dirtywave M8 Headless](https://github.com/Dirtywave/M8HeadlessFirmware) into a PCB and make it handheld.
Test build was done on the fabricated PCB. MIDI I/O AUDIO OUT seems to be working well.
Kicad data has been withheld from the public due to criticism that it would damage the market. Please contact.

## Now progress

- [x] Idea sketch
- [x] Draw proto Schematics
- [x] Design proto PCB
- [x] Proto type
- [x] Design PCB
- [x] Fabrication PCB
- [x] Test Build

## Latest Status

<img src="https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image7.jpg" width="320">
<img src="https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image6.jpg" width="320">
<img src="https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image8.jpg" width="320">

# Build
It works almost perfectly, except for the audio input.
You need Partially arranged wire by hand.
## Parts list
- Raspberry Pi 3 Model B+(Raspberry Pi 4 Model B is better) *1
- Teensy 4.1 *1
- 90 deglee USB cable(You can find them at [Aliexpress](https://ja.aliexpress.com/item/10000315945874.html)) *1
- Display for the Raspberry Pi (You can find them at [Aliexpress](https://aliexpress.com/item/4000380101537.html)) *1
- PCM5102dac decorder (You can find them at [Aliexpress](https://aliexpress.com/item/1005002898278583.html)) *1
- Kailh Low profile key switch with cap (You can find them at [Aliexpress](https://ja.aliexpress.com/item/32959996455.html)) *8
- 3.7V Litium poly battery (You can find them at [Aliexpress](https://ja.aliexpress.com/item/32959996455.html)) *1
- Ajustable battery module (You can find them at [Aliexpress](https://ja.aliexpress.com/item/4000087101515.html)) *1
- Slide switch (2.54mm pich) *1
- Tact switch (6mm*6mm) *1
- 30mm long hex spacer *4
- 15mm long hex spacer *4
- 6N138 optocoupler *1
- other resistors and capacitor (STM assenbly use by JLCPCB)

## Setup Teensy
Be sure to follow the official manual.
The version you install must be 2.0.5. Otherwise, the midi i/o will not work.
https://github.com/DirtyWave/M8Docs/blob/main/docs/M8HeadlessSetup.md

## Setup Raspberry Pi
- Setup m8c
- Setup full screen
- Setup autostart
- Setup shutdown key(Use Tact switch)
- Setup key switches

### Setup m8c
The client software for Headless is [m8c](https://github.com/laamaa/m8c), which is published by laamaa.
The setup for using the m8c and Raspberry Pi can be seen in detail in the littlescale video.
[![littlescale](http://img.youtube.com/vi/CqUvGfdyEnM/0.jpg)](https://www.youtube.com/watch?v=CqUvGfdyEnM)

### Setup full screen
You can set the fullscreen in the config in the m8c installation folder.

### Setup autostart
Put in a general autostart program.It's probably faster to ask Google than to ask me.
Then KICK The m8c startup command, ". /m8c".

### Setup shutdown key(Use Tact switch)
This one also plants a commonly used shutdown program. We connected the GND of the tact switch and the "shutdown button" pad on the PCB to GPIO5 (PIN29) and GND (PIN30) respectively.
This is because pins 1 to 28 are blocked by the display.
https://www2.quartoknows.com/page/raspberry-pi-shutdown-button

### Setup key switches
Key mapping is done using a program called "GPIONext", which is often used in RetroPi.
The GND must be connected to PIN39 or PIN34, but the other pins can be configured as you like. Use a wire to arrange the wires.
https://github.com/mholgatem/GPIOnext

The key layout to be set should follow the m8c manual.

Keys for controlling the progam:
- Up arrow = up
- Down arrow = down
- Left arrow = left
- Right arrow = right
- a / left shift = select
- s / space = start
- z / left alt = opt
- x / left ctrl = edit



The following is a memorandum for development, but I leave it here just in case.
------------------------

## Design Outline

![Design Sketch](https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image1.jpg)

- M8 Headless will be installed on teensy4.1.
- The client software for Headless is [m8c](https://github.com/laamaa/m8c), which is published by laamaa.
- The client computer will be a Raspberry Pi3 Model B+.
- For the display, we will use a third party display for the Raspberry Pi. (You can find them at [Aliexpress](https://aliexpress.com/item/4000380101537.html))
- We will implement MIDI IN & OUT.
- We will use the GPIO pins of the Raspberry pi to perform key emulation and try to get close to the original key layout.
- Sandwich the above various devices on two PCBs.
- If there is enough room, I would like to implement a battery module as well.

---

### Task1 Client and Display

---

The setup for using the m8c and Raspberry Pi can be seen in detail in the littlescale video.
[![littlescale](http://img.youtube.com/vi/CqUvGfdyEnM/0.jpg)](https://www.youtube.com/watch?v=CqUvGfdyEnM)

Since this video shows video and audio output from the HDMI port, we will assume that audio output is available using a 3.5mm stereo jack if you use a touch display with audio output, available from [Aliexpress](https://aliexpress.com/item/4000380101537.html).
Or I'm thinking of getting the sound through a DAC module.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">hacked together a quick teensy 4.1 midi i/o and audio out board for making it a bit easier to use the <a href="https://twitter.com/hashtag/m8tracker?src=hash&amp;ref_src=twsrc%5Etfw">#m8tracker</a> headless version in my dawless setup <a href="https://t.co/O24RRRLSRL">pic.twitter.com/O24RRRLSRL</a></p>&mdash; Jonne Kokkonen (@laamaa) <a href="https://twitter.com/laamaa/status/1404158507188985856?ref_src=twsrc%5Etfw">June 13, 2021</a></blockquote>

## Practical notes

- Use Headless version 2.0.5, because MIDI I/O does not work well.

---

### Task2 MIDI IN&OUT

---

As has been discussed several times on Dirtywave's Discord, it seems possible to implement MIDI IN and OUT in a Headless environment.
The developer, trash80, commented "MIDI in is pin 28, MIDI out is pin 1, it's enabled".
I plan to implement them as shown in the schematic below.

![midi schematic](https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/midi_in_out.png)

---

### Task3 Key Switch

---

Key emulation (key mapping) using the GPIO pins of the Raspberry Pi is also currently in the hypothetical stage.
There are plenty of people practicing something similar with the Retropi!
For the keyswitch itself, I will use the same one as the M8 original.

[![GIPO Key Mapping](http://img.youtube.com/vi/BV_nVu8Be7M/0.jpg)](https://www.youtube.com/watch?v=BV_nVu8Be7M)

---

### Task4 Battery Module

---

I'd like to use the tiny little battery charging modules that [Aliexpress](https://ja.aliexpress.com/item/4001196670805.html) sells so that I can seamlessly connect the power supply, PSU, and charging.
I think I will agonize over where to put the batteries!

---

### Reference data

---

James' work has addressed some of the concerns of this project!
<img src="https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image2.jpg" width="320">
<img src="https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image5.jpg" width="420">
