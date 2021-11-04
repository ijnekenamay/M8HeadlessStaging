# M8 Headless Staging
This is a DIY project to push the [Dirtywave M8 Headless](https://github.com/Dirtywave/M8HeadlessFirmware) into a PCB and make it handheld.
We are currently in the idea sketch stage.
Any help with beta testing and design would be greatly appreciated!

## Now progress
- [x] Idea sketch
- [ ] Proto type
- [ ] Design PCB
- [ ] Fabrication PCB
- [ ] Test Build

## Design Outline
![Design Sketch](https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/image1.png)
* M8 Headless will be installed on teensy4.1.
* The client software for Headless is [m8c](https://github.com/laamaa/m8c), which is published by laamaa.
* The client computer will be a Raspberry Pi3 Model B+.
* For the display, we will use a third party display for the Raspberry Pi. (You can find them at [Aliexpress](https://aliexpress.com/item/4000380101537.html))
* We will implement MIDI IN & OUT.
* We will use the GPIO pins of the Raspberry pi to perform key emulation and try to get close to the original key layout.
* Sandwich the above various devices on two PCBs.
* If there is enough room, I would like to implement a battery module as well.


***
### Task1 Client and Display
***
The setup for using the m8c and Raspberry Pi can be seen in detail in the littlescale video.
[![littlescale](http://img.youtube.com/vi/CqUvGfdyEnM/0.jpg)](https://www.youtube.com/watch?v=CqUvGfdyEnM)

Since this video shows video and audio output from the HDMI port, we will assume that audio output is available using a 3.5mm stereo jack if you use a touch display with audio output, available from [Aliexpress](https://aliexpress.com/item/4000380101537.html).


***
### Task2 MIDI IN&OUT
***
As has been discussed several times on Dirtywave's Discord, it seems possible to implement MIDI IN and OUT in a Headless environment.
The developer, trash80, commented "with TX1 being pin 1, and RX1 being pin 28".
I plan to implement them as shown in the schematic below.

![midi schematic](https://github.com/ijnekenamay/M8HeadlessStaging/raw/main/midi_in_out.png)


***
### Task3 Key Switch
***
Key emulation (key mapping) using the GPIO pins of the Raspberry Pi is also currently in the hypothetical stage.
There are plenty of people practicing something similar with the Retropi!
For the keyswitch itself, I will use the same one as the M8 original.

[![GIPO Key Mapping](http://img.youtube.com/vi/BV_nVu8Be7M/0.jpg)](https://www.youtube.com/watch?v=BV_nVu8Be7M)


***
### Task4 Battery Module
***
I'd like to use the tiny little battery charging modules that [Aliexpress](https://ja.aliexpress.com/item/4001196670805.html) sells so that I can seamlessly connect the power supply, PSU, and charging.
I think I will agonize over where to put the batteries!



