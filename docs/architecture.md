# Architecture

## Hardware Architecture

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

## Software Architecture

```
app/ (platform-independent)
├─ pages/           (UI structure — compiled from lmngdsl)
├─ widgets/         (reusable UI components)
├─ playback/        (audio pipeline, queue, state)
├─ library/         (scan, index, search)
└─ settings/        (settings models)

platform/ (device-specific)
├─ display/         (SPI driver, framebuffer)
├─ audio/           (I2S, DAC, amplifier control)
├─ storage/         (SDMMC, filesystem)
├─ input/           (buttons, scroll wheel, PCNT)
├─ power/           (battery, charging, sleep)
└─ system/          (boot, status bar, clock, Bluetooth)

lmngdsl/ (standalone)
├─ parser/          (page definition parser)
├─ compiler/        (validation, optimization)
└─ codegen/         (Rust struct generation)
```

## Configuration Layers

lemming separates configuration into three distinct layers:

### Behavior

Stored in TOML files on the SD card.

Examples: Bluetooth, ReplayGain, library scanning, sleep timer, audio settings, advanced options.

These describe how the player behaves.

### Appearance

Stored as themes on the SD card.

Themes define presentation only: colors, icons, borders, spacing.

Themes must never execute code.

### Structure

Defined by page descriptions compiled through lmngdsl.

Pages define interface layout and composition. Pages do not contain logic. Pages do not execute code at runtime.

## UI Ownership

### Firmware-Owned

- Status bar (battery, clock, Bluetooth, playback status, active page indicator)
- System behaviors (power management, sleep, wake)
- Widget implementations
- Input handling

### Page-Owned

- Page content layout
- Widget composition
- Structural organization

Pages should never directly render battery indicators or other global system information.

## Compilation Model

Page descriptions are compiled and cached before deployment. The firmware never interprets page source files at runtime.

The compiler validates page definitions before they reach the device.

## Home Page

Users are encouraged to build their own home page by composing existing widgets.

The firmware provides widgets and behavior. The page describes structure.
