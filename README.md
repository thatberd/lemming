# Lemming

Lemming is a portable music player I'm creating using an ESP32-S3.

The idea came about because I got tired of using my phone for music. Phones are convenient, but they bring notifications, messaging apps, and many distractions that compete for my attention.

I wanted something simpler.

Put some MP3s on a microSD card. Plug in headphones. Press play.

This project is completely open source. The firmware, PCB, enclosure, and documentation can all be found in this repository.

## Current Status

It's very much a work in progress.

Right now, I am designing the hardware and slowly developing the firmware. Nothing has been built yet.

Current goals for v1:

* MP3 playback
* Album art
* Fast library browsing
* Resume after power loss
* USB-C charging
* 10+ hour battery life
* Physical controls only

## Hardware

Current v1 hardware targets:

| Component     | Part                  |
| ------------- | --------------------- |
| MCU           | ESP32-S3-WROOM-1-N8R8 |
| Display       | 3.2" 240×320 SPI TFT  |
| Storage       | microSD               |
| DAC           | PCM5102A              |
| Headphone Amp | TPA6132A2             |
| Battery       | 1500–2500 mAh LiPo    |
| Charging      | USB-C + BQ24074       |

The exact bill of materials will likely change before the first PCB revision.

## Why "Lemming"?

Honestly, because the name made me laugh, and I chose it before the project had a schematic, firmware design, or even a rough sketch.

The name stuck. 

## Contributing

The project is still early enough that almost everything can change.

Ideas, bug reports, hardware feedback, firmware experiments, and design criticism are all welcome.
