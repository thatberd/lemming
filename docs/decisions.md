# Design Decisions

This document records major design decisions made during Lemming's development.

The goal is to preserve the reasoning behind decisions, not just the decisions themselves.

If a future contributor wants to change something, they should understand why the current solution exists first.

---

# DD-001: Use ESP32-S3-WROOM-1-N8R8 Instead of XIAO ESP32-S3

**Date:** June 2026

## Decision

Use the ESP32-S3-WROOM-1-N8R8 as the primary MCU for Lemming v1.

## Why

The original design used a XIAO ESP32-S3.

As the hardware matured, the required peripheral count exceeded the practical GPIO budget available on the XIAO.

Required peripherals include:

* SPI display
* SD card
* I2S audio
* Scroll wheel
* Multiple buttons
* Battery monitoring
* USB

The WROOM provides significantly more GPIO while retaining:

* ESP32-S3 compatibility
* Native USB
* PSRAM
* Good Rust support

## Consequences

### Positive

* Simpler hardware
* No GPIO expanders
* SDMMC support
* More future expansion options

### Negative

* Slightly larger PCB footprint
* Requires custom charging circuitry

## Alternatives Considered

* XIAO ESP32-S3
* GPIO expanders
* ADC button ladder

---

# DD-002: Scroll Wheel Instead of Rotary Encoder

## Decision

Use a mouse-style scroll wheel as the primary navigation input.

## Why

The original concept used a rotary encoder.

A scroll wheel feels faster and more natural for navigating large music libraries.

Target interactions:

* Browsing albums
* Browsing artists
* Scrubbing tracks
* Adjusting volume

The desired experience is closer to an iPod wheel than a traditional encoder knob.

## Consequences

### Positive

* Faster navigation
* More distinctive identity
* Better one-handed use

### Negative

* More mechanical complexity
* Requires validation of encoder feel

## Alternatives Considered

* Standard rotary encoder
* Touch wheel
* Touchscreen

---

# DD-003: Physical Controls Only

## Decision

Lemming v1 will not include a touchscreen.

## Why

Touchscreens increase:

* Cost
* Complexity
* Power consumption

Physical controls are a core part of the product vision.

The device should be operable without looking directly at the screen.

## Consequences

### Positive

* Lower cost
* Better battery life
* Better pocket usability

### Negative

* UI design constraints

---

# DD-004: MP3 First

## Decision

Focus on MP3 support for v1.

## Why

MP3 remains the most common format in personal music collections.

Supporting fewer codecs initially allows development effort to focus on:

* Playback stability
* UI quality
* Library performance

## Consequences

### Positive

* Simpler firmware
* Faster development

### Negative

* Limited format support initially

## Future Formats

* FLAC
* WAV
* OGG
* AAC

---

# DD-005: Offline-Only Design

## Decision

Lemming does not require internet connectivity.

## Why

The project exists partly as a reaction against streaming-first music experiences.

Users should own their music files.

The device should remain functional forever without external services.

## Consequences

### Positive

* Simpler software
* Better privacy
* No account requirements

### Negative

* No streaming support

---

# DD-006: Ratatui-Based User Interface

## Decision

Build the UI using Ratatui concepts and architecture.

## Why

The project already has a desktop simulator target.

Using Ratatui enables:

* Shared UI code
* Fast iteration on desktop
* Snapshot testing
* Consistent layouts

The device backend will implement Ratatui's `Backend` trait directly.

## Consequences

### Positive

* Faster development
* Better testing
* Desktop parity

### Negative

* Custom rendering backend required

---

# DD-007: Pixel-Art Album Art

## Decision

Album art is displayed as low-resolution pixel art.

## Why

The display resolution and hardware limitations make full-resolution album art expensive.

Pixel-art thumbnails:

* Fit the visual style
* Use less memory
* Render faster

## Consequences

### Positive

* Distinctive appearance
* Lower memory usage

### Negative

* Less detailed artwork

---

# DD-008: Cached Music Library

## Decision

Maintain a persistent library index.

## Why

Scanning large SD cards on every boot is slow.

The device should feel instant.

The library cache stores:

* Titles
* Artists
* Albums
* Track metadata
* Artwork references

## Consequences

### Positive

* Fast startup
* Smooth browsing

### Negative

* Index maintenance required

---

# DD-009: PCM5102A DAC

## Decision

Use the PCM5102A DAC.

## Why

The PCM5102A provides:

* Excellent audio quality
* No MCLK requirement
* Simple I2S integration
* Good availability

Audio quality exceeds the practical limits of the rest of the hardware.

## Alternatives Considered

* MAX98357A
* ES8388
* ES9018K2M

---

# DD-010: TPA6132A2 Headphone Amplifier

## Decision

Use the TPA6132A2 headphone amplifier.

## Why

Earlier designs used the TDA1308.

The TPA6132A2:

* Eliminates large coupling capacitors
* Includes pop suppression
* Reduces board area
* Improves audio performance

## Consequences

### Positive

* Cleaner design
* Better user experience

### Negative

* QFN package

---

# DD-011: BQ24074 Charger

## Decision

Use the BQ24074 battery charger and power-path controller.

## Why

The original design used a TP4056.

The BQ24074:

* Supports simultaneous charge and operation
* Includes power-path management
* Simplifies the power subsystem

## Consequences

### Positive

* Better user experience
* Cleaner power architecture

### Negative

* More expensive component

---

# DD-012: Open Hardware

## Decision

Publish all design files.

## Includes

* Firmware source
* PCB files
* CAD files
* Documentation
* Build instructions

## Why

The project should be reproducible and modifiable by anyone.

Users should not depend on a single company or individual to keep the project alive.

## Consequences

### Positive

* Community contributions
* Long-term sustainability
* Repairability

### Negative

* Forks are inevitable

---

# DD-013: Page DSL Over Hardcoded UI

## Decision

Use a declarative page description language (lmngdsl) instead of hardcoding all UI layouts in Rust.

## Why

Hardcoding every screen in Rust creates friction when iterating on UI design.

A compiled page DSL allows:

- Faster UI iteration
- Clearer separation between structure and behavior
- Safer changes through compiler validation
- Reusable widgets across pages

Pages describe structure only. They contain no variables, loops, conditionals, functions, scripting, or runtime execution.

Pages are compiled and cached before deployment. The firmware never interprets page source files at runtime.

## Consequences

### Positive

- Faster UI development
- Safer UI changes
- Reusable page definitions
- Build-time validation

### Negative

- Additional tooling required
- Compile step added to workflow

## Alternatives Considered

- Hardcoded Rust layouts
- JSON/YAML configuration
- Lua scripting

---

# DD-014: Three-Layer Configuration

## Decision

Separate configuration into behavior, appearance, and structure.

## Why

Mixing configuration concerns creates coupling between unrelated systems.

Behavior settings (TOML) describe how the player operates.

Themes describe how the player looks.

Page descriptions (lmngdsl) describe how the interface is organized.

Each layer has a distinct format, storage location, and ownership model.

## Consequences

### Positive

- Clear ownership boundaries
- Safer configuration editing
- Easier to document and validate

### Negative

- More configuration files to manage

## Alternatives Considered

- Single monolithic config file
- Registry-based settings
- Environment variables

---

# DD-015: Firmware-Owned System UI

## Decision

The firmware owns all global system UI elements. Pages own only their content area.

## Why

Global system state (battery, clock, Bluetooth, playback status) is device state, not application state.

It must remain accessible regardless of which page is active.

It has strict timing and power management requirements.

It must not depend on page-level logic.

## Consequences

### Positive

- Consistent system UI across all pages
- Clearer separation of concerns
- Simpler page definitions

### Negative

- Less flexibility for page-level system UI experimentation

## Alternatives Considered

- Page-owned system UI
- Hybrid system where pages can inject system elements

---

# DD-016: No Artificial Boot Delays

## Decision

Boot animations must not artificially delay startup.

## Why

The player should become usable as soon as initialization finishes.

Any animation should overlap real initialization work.

Boot visuals exist to transition into the interface rather than hide unnecessary waiting.

Artificial delays create the impression that the device is slower than it is.

## Consequences

### Positive

- Faster perceived startup
- Honest user experience
- Simpler boot code

### Negative

- Less dramatic boot sequence

---

# DD-017: EastRising ER-TFT032IPS-3.2 Display

**Date:** July 2026

## Decision

Use the EastRising ER-TFT032IPS-3.2 as the primary display for Lemming v1.

## Why

The original design left the exact display panel and supplier as open questions.

The ER-TFT032IPS-3.2 meets all requirements:

* 3.2 inch diagonal
* 240 × 320 resolution
* IPS panel with 80° viewing angle in all directions
* ST7789V2 controller with well-supported SPI interface
* Long-term continuity supply guaranteed until at least 2033
* Low BOM cost (~US$9)

The parallel interface options (8080 8/16-bit) provide headroom if SPI bandwidth becomes a bottleneck.

The controller is already supported by existing Rust display driver crates.

## Consequences

### Positive

* IPS panel fits the pixel-art aesthetic and viewing-angle requirements
* Stable long-term supply
* Inexpensive
* Optional capacitive touch panel (FT6236) can be disabled in firmware to save power

### Negative

* Touch panel is included by default even if unused
* Backlight draws 120 mA at full brightness

## Alternatives Considered

* ER-TFT032A3-3-4334 - Same size, different connector footprint
* ILI9341-based 3.2" panel - Wider software support, TN panel with poorer viewing angles

---

# Future Decisions To Make

* Final scroll wheel model
* Battery supplier
* Theme format specification
* Library database format
* Firmware licensing
* Hardware licensing
* Manufacturing strategy

---

*"Write down the reason while it's still fresh."*
