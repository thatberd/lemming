# Project Overview

**Lemming** is a compact offline-first handheld music player designed around the ESP32-S3 platform.

The goal is to build a device that feels more intentional than a smartphone while remaining affordable, repairable, hackable, and completely open source.

Unlike streaming-focused devices, lemming prioritizes:

- Local music ownership
- Long battery life
- Fast startup
- Physical controls
- Open-source firmware
- Repairability
- Pocketability
- Distraction-free listening

Target audience:

- Music enthusiasts
- Students
- Makers
- Open hardware enthusiasts
- Users seeking a dedicated music device

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

lmngdsl is a standalone project intended to provide a generic declarative UI description language. It is not coupled exclusively to lemming.

## Canonical Locations

- Website: https://lemming.cc
- Documentation: https://docs.lemming.cc
- GitHub Organization: github.com/lemming-dev

---

# Design Philosophy

lemming should feel like a tool.

It should:

- Boot quickly
- React instantly
- Require minimal attention
- Encourage listening rather than screen interaction

Simplicity is achieved by removing unnecessary complexity rather than adding features.

Firmware owns behavior. Configuration owns customization.

Local files are first-class. The companion app is optional. Configuration is transparent.

Prefer open standards over proprietary formats.

If a feature requires typing on the device, it probably doesn't belong on lemming.

lemming is intentionally:

- Offline-first
- Touch-free
- Text-first
- Open-source
- User-serviceable

lemming is intentionally not:

- A smartphone
- A streaming device
- A social platform
- A notification machine

## Configuration Philosophy

Configuration is separated into three concerns: behavior, appearance, and structure.

### Behavior

Stored in TOML files on the SD card.

Examples: Bluetooth, ReplayGain, library scanning, sleep timer, audio settings, advanced options.

These describe how the player behaves.

### Appearance

Stored as themes on the SD card.

Themes define presentation only: colors, icons, fonts (future), borders, spacing.

Themes must never execute code.

### Structure

Defined by page descriptions compiled through lmngdsl.

Pages define interface structure. Pages do not contain logic. Pages do not execute code. Pages simply describe layout and composition.

Pages are compiled and cached before deployment. The firmware never interprets page source files at runtime.

The compiler validates page definitions before they reach the device.

Applications provide widgets while the DSL only describes composition.

---

# Hardware Architecture

```
                microSD (SDMMC)
                        │
                        ▼
              ESP32-S3-WROOM-1-N8R8
                 │      │      │
                 ▼      ▼      ▼
              Display Audio  Input
                 │      │      │
                 ▼      ▼      ▼
                LCD    DAC  Buttons
                 │      │      │
                 ▼      ▼      ▼
                SPI    I2S   Scroll Wheel  
```

---

# Core Hardware

## MCU

### ESP32-S3-WROOM-1-N8R8

Features:

- Dual-core LX7 @ 240MHz
- 8MB Flash
- 8MB Octal PSRAM
- Native USB
- Wi-Fi/Bluetooth capable
- USB firmware flashing
- Strong Rust support

Wi-Fi is disabled in stock firmware.

---

# Display

## Display (spec)

Size:

- 3.2"

Resolution:

- 240 × 320

Orientation:

- Portrait

Interface:

- SPI

Supported Controllers:

- ILI9341
- ST7789

Firmware uses a controller abstraction layer to support both.

---

# Storage

## microSD

Connector:

- Hirose DM3AT-SF-PEJM5

Interface:

- SDMMC 4-bit

Filesystem:

- FAT32
- exFAT

Supported capacities:

- 8GB–512GB+

Purpose:

- Music library
- Album art cache
- Settings
- Firmware updates
- Library index

---

# Audio System

## DAC

### PCM5102A

Features:

- I2S
- Internal PLL
- No MCLK required
- High-quality stereo output

---

## Headphone Amplifier

### TPA6132A2

Features:

- DirectPath architecture
- No coupling capacitors
- Pop suppression
- Enable pin for clean startup/shutdown

---

## Output

- 3.5mm stereo headphone jack

---

## Volume

Digital volume control.

Implemented entirely in firmware.

---

# Input System

## Primary Navigation

### Mouse-style Scroll Wheel

Not a rotary encoder.

Characteristics:

- Quadrature output
- PCNT decoding
- Momentum scrolling
- Velocity-based acceleration

Used for:

- Menu navigation
- Search navigation
- Queue browsing
- Playback scrubbing

---

## Buttons

Dedicated buttons:

- Select
- Back
- Menu
- Play/Pause
- Next
- Previous

System buttons:

- Boot
- Reset

---

# Battery & Charging

## Battery

Protected LiPo cell.

Target capacity:

- 1500–2500mAh

Reference configuration:

- ~2000mAh

---

## Charger

### BQ24074

Features:

- Integrated power-path management
- Charging while operating
- USB-powered operation without battery
- Thermal regulation

Charge current:

- 500mA

---

## USB

USB-C

Features:

- Charging
- Firmware flashing
- USB serial console

Future:

- USB Mass Storage mode

---

# Mechanical Design

## Enclosure

Prototype:

- 3D printed

Material:

- PETG preferred
- PLA acceptable

Finish:

- Matte

Color:

- Smoky translucent gray

---

## Dimensions

Target:

```
115 mm × 85 mm × 18 mm
```

---

## Branding

Logo:

```
lemming
```

Typography:

```
JetBrains Mono
```

Used for:

- Logo
- PCB markings
- Boot screen
- Documentation

---

# Software Architecture

## Language

Rust

## Layer Structure

```
app/platform-device/platform-desktop/backend-st7789/
```

## Configuration Layers

### Behavior

Stored in TOML configuration files on the SD card.

Behavior settings are read by the firmware at startup and whenever the file changes.

### Appearance

Stored as themes on the SD card.

Themes define presentation only. Themes must never execute code.

### Structure

Defined by page descriptions compiled through lmngdsl.

Pages are compiled and cached before deployment. The firmware never interprets page source files at runtime.

## App Layer

Contains:

- Playback manager
- Library manager
- Queue manager
- Search system
- Page composer
- UI

Platform-independent.

## Device Layer

Contains:

- Audio drivers
- Display drivers
- SD drivers
- USB
- Power management
- System UI (status bar, clock, battery, Bluetooth)

ESP-IDF based.

---

# User Interface

## Philosophy

Text-first.

Information-dense.

Fast.

Minimal animation.

## Home Page

Users are encouraged to build their own home page by composing existing widgets.

The firmware provides widgets. Users arrange them. The firmware provides behavior. The page describes structure.

## Framework

Ratatui

Implemented using a custom embedded backend.

Desktop simulator uses standard Ratatui backend.

## Page DSL

Pages are defined using lmngdsl, a standalone declarative UI description language.

The page language is intentionally not a programming language. It contains:

- no variables
- no loops
- no conditionals
- no functions
- no scripting
- no runtime execution

Its purpose is only to describe UI structure.

Pages are compiled and cached before deployment. The firmware never interprets page source files at runtime.

The compiler validates page definitions before they reach the device.

Applications provide widgets while the DSL only describes composition.

## Themes

Theme system stored on SD card.

Themes define:

- Background
- Foreground
- Accent
- Selection

Themes must never execute code.

## System UI

The firmware owns global system elements:

- Status bar
- Battery
- Clock
- Bluetooth
- Playback status
- Active page indicator
- Other device state

Pages own only their content area. Pages should never directly render battery indicators or other global system information.

The status bar itself is configurable while remaining firmware-owned.

## Main Menu

```
MusicArtistsAlbumsFoldersPlaylistsNow PlayingSettings
```

---

## Now Playing

Displays:

- Pixel-art album art
- Song title
- Artist
- Progress bar
- Playback status

---

# Library System

## Startup Scan

On first boot or library change:

```
SD Card    ↓Metadata Scan    ↓Library Index    ↓Persistent Cache
```

---

## Index

Stored as:

```
.lemming/index.bin
```

Contains:

- Songs
- Albums
- Artists
- Metadata offsets
- Album-art references

The UI never traverses the filesystem directly.

---

# Album Art System

## Strategy

Full-resolution images are never kept permanently in RAM.

Pipeline:

```
Image ↓Resize ↓Dither ↓Thumbnail Cache
```

---

## Rendering

Pixel-art style.

Cached thumbnails stored on SD.

---

# Playback Architecture

```
microSD    ↓Compressed Ring Buffer    ↓Decoder    ↓PCM Ring Buffer    ↓I2S    ↓PCM5102A    ↓TPA6132A2    ↓Headphones
```

---

# Supported Formats

v1:

- MP3
- FLAC
- WAV

Future:

- OGG Vorbis
- Opus
- AAC

---

# Power Management

## Display Sleep

After inactivity:

- Backlight off
- Playback continues

---

## Deep Sleep

Activated via power-button hold.

---

## Resume

Persists:

- Track
- Playback position
- Queue
- Volume
- Settings

---

# Firmware Updates

Update package placed on SD card.

Process:

```
Firmware File      ↓Detect      ↓Install      ↓Reboot
```

No computer required.

---

# Boot Process

The boot animation should never artificially delay startup.

Any animation should overlap real initialization work.

The player should become usable as soon as initialization finishes.

Boot visuals exist to transition into the interface rather than hide unnecessary waiting.

---

# Open Source Requirements

The project releases:

- Firmware
- Schematics
- PCB layout
- BOM
- CAD models
- Assembly files
- Documentation

Public development.

Community contributions encouraged.

---

# Success Criteria

Lemming v1 is successful if it can:

- Boot in under 5 seconds
- Play MP3, FLAC, and WAV reliably
- Browse large libraries smoothly
- Maintain playback while navigating
- Display album art
- Achieve 10+ hours battery life
- Survive daily pocket use
- Be reproducible by hobbyists
- Remain fully open source
- Serve as a platform for future experimentation
