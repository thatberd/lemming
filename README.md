<img src="assets/logo.png" alt="lemming" width="300">

---

lemming is a focused, offline-first, open-source portable music player.

Music comes first. Everything else is secondary.

The project uses an ESP32-S3, runs Rust firmware, and is designed to be simple by removing unnecessary complexity rather than adding features.

Local files are first-class. The companion app is optional. Configuration is transparent.

## Current Status

The project is in active development. Hardware design and firmware architecture are being defined.

Current v1 hardware targets:

| Component     | Part                  |
| ------------- | --------------------- |
| MCU           | ESP32-S3-WROOM-1-N8R8 |
| Display       | 3.2" 240×320 IPS TFT (ST7789V2) — EastRising ER-TFT032IPS-3.2 |
| Storage       | microSD               |
| DAC           | PCM5102A              |
| Headphone Amp | TPA6132A2             |
| Battery       | 1500–2500 mAh LiPo    |
| Charging      | USB-C + BQ24074       |

Current v1 software goals:

- MP3 playback
- Album art
- Fast library browsing
- Resume after power loss
- USB-C charging
- 10+ hour battery life
- Physical controls only

## Design Principles

- Music comes first.
- Offline-first.
- Local files are first-class.
- Companion app is optional.
- Configuration is transparent.
- Prefer open standards over proprietary formats.
- Remove complexity before adding features.
- If a feature requires typing on the device, it probably doesn't belong on lemming.
- Firmware owns behavior.
- Configuration owns customization.

## Repository Ecosystem

lemming is organized across multiple repositories:

- **firmware** — Rust application, platform drivers, and page compiler
- **hardware** — Schematics, PCB layout, BOM, and CAD files
- **companion** — Optional desktop/mobile companion application
- **lmngdsl** — Standalone declarative UI description language and compiler
- **docs** — Project documentation

## Canonical Locations

Website: https://lemming.cc

Documentation: https://docs.lemming.cc

GitHub Organization: github.com/lemming-dev

## Why "lemming"?

Honestly, because the name was cute, and it was chosen before the project had a schematic, firmware design, or even a rough sketch. The name stuck.

## Contributing

The project is still early enough that almost everything can change.

Ideas, bug reports, hardware feedback, firmware experiments, and design criticism are all welcome.
