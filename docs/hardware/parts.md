# Parts List

This document tracks every major component used in Lemming v1.

For each part we record:

* Purpose
* Manufacturer part number
* Supplier availability
* Alternatives
* Notes

The goal is to avoid future confusion when parts inevitably go out of stock.

---

# MCU

## ESP32-S3-WROOM-1-N8R8

| Field        | Value                 |
| ------------ | --------------------- |
| Manufacturer | Espressif             |
| Part Number  | ESP32-S3-WROOM-1-N8R8 |
| Function     | Main MCU              |
| Flash        | 8 MB                  |
| PSRAM        | 8 MB                  |
| Package      | Castellated Module    |
| Status       | Selected              |

### Why

* Large community support
* Excellent Rust support
* Native USB
* Plenty of GPIO
* Built-in PSRAM

### Alternatives

| Part                   | Notes                  |
| ---------------------- | ---------------------- |
| ESP32-S3-WROOM-1-N16R8 | Drop-in replacement    |
| ESP32-S3-WROOM-2       | Larger memory variants |

### Notes

GPIO35–37 are reserved internally by octal PSRAM.

---

# Display

## 3.2" 240×320 SPI TFT

| Field       | Value     |
| ----------- | --------- |
| Resolution  | 240×320   |
| Interface   | SPI       |
| Orientation | Portrait  |
| Status      | Candidate |

### Requirements

* 3.2 inch size
* SPI interface
* 240×320 resolution
* Stable long-term availability

### Notes

Controller may vary:

* ILI9341
* ST7789

Firmware should support both through mipidsi.

### Open Questions

* Exact panel supplier
* Exact FPC pinout
* Connector footprint

---

# Storage

## microSD Socket

| Field        | Value          |
| ------------ | -------------- |
| Manufacturer | Hirose         |
| Part Number  | DM3AT-SF-PEJM5 |
| Status       | Selected       |

### Why

* Reliable
* Widely used
* Good documentation

### Alternatives

Document if needed later.

---

# Audio DAC

## PCM5102A

| Field        | Value             |
| ------------ | ----------------- |
| Manufacturer | Texas Instruments |
| Part Number  | PCM5102A          |
| Status       | Selected          |

### Why

* No MCLK required
* Excellent audio quality
* Internal PLL
* Simple I2S interface

### Alternatives

| Part     | Notes          |
| -------- | -------------- |
| PCM5101A | Pin compatible |
| PCM5100A | Pin compatible |

### Notes

Use internal PLL mode.

---

# Headphone Amplifier

## TPA6132A2

| Field        | Value             |
| ------------ | ----------------- |
| Manufacturer | Texas Instruments |
| Part Number  | TPA6132A2         |
| Status       | Selected          |

### Why

* Modern design
* No output coupling capacitors
* Low power
* Built-in pop suppression

### Alternatives

| Part    | Notes                     |
| ------- | ------------------------- |
| TDA1308 | Easier hand-solder option |
| LM4881  | Possible fallback         |

### Notes

Canonical board uses TPA6132A2.

TDA1308 remains documented for hand-built variants.

---

# Battery Charger

## BQ24074

| Field        | Value             |
| ------------ | ----------------- |
| Manufacturer | Texas Instruments |
| Part Number  | BQ24074           |
| Status       | Selected          |

### Why

* Integrated power path
* Battery or USB operation
* Better user experience than TP4056

### Charge Current

Target:

500 mA

### Alternatives

| Part     | Notes               |
| -------- | ------------------- |
| MCP73871 | Acceptable fallback |
| TP4056   | Not preferred       |

---

# Voltage Regulator

## AP2112K-3.3

| Field        | Value       |
| ------------ | ----------- |
| Manufacturer | Diodes Inc  |
| Part Number  | AP2112K-3.3 |
| Status       | Selected    |

### Why

* Widely available
* Simple
* Good enough current capacity

### Alternatives

| Part       | Notes                 |
| ---------- | --------------------- |
| XC6220B331 | Higher current option |

---

# Battery

## LiPo Cell

| Field     | Value          |
| --------- | -------------- |
| Type      | Protected LiPo |
| Capacity  | 1500–2500 mAh  |
| Connector | JST-PH 2.0     |
| Status    | Candidate      |

### Requirements

* Protection circuit required
* Rechargeable
* Easy sourcing

### Notes

JST polarity is not standardized.

Verify before connecting.

---

# Input

## Scroll Wheel Encoder

| Field  | Value                        |
| ------ | ---------------------------- |
| Type   | Mouse-style quadrature wheel |
| Status | Needs validation             |

### Requirements

* Smooth scrolling
* Good tactile feel
* PCNT compatible

### Candidate Vendors

* ALPS
* TTC
* Kailh

### Notes

Prototype before PCB release.

Input feel is a major part of the product experience.

---

# Buttons

## Tactile Switches

| Field  | Value                |
| ------ | -------------------- |
| Type   | 6×6 mm tact switches |
| Status | Selected             |

### Buttons

* Select
* Back
* Menu
* Play/Pause
* Next
* Previous

---

# USB

## USB-C Connector

| Field  | Value     |
| ------ | --------- |
| Type   | USB 2.0   |
| Status | Candidate |

### Requirements

* Through-hole shell tabs
* JLCPCB available
* USB-C charging
* USB firmware flashing

---

# Audio Output

## 3.5mm Headphone Jack

| Field  | Value      |
| ------ | ---------- |
| Type   | Stereo TRS |
| Status | Candidate  |

### Requirements

* Through-hole preferred
* Easily sourced
* Durable

---

# PCB

## Main Board

| Field      | Value       |
| ---------- | ----------- |
| Layers     | 4           |
| Finish     | ENIG        |
| Color      | Matte Black |
| Silkscreen | White       |

### Stackup

1. Signal
2. Ground
3. Power + Ground
4. Signal

### Notes

Unbroken ground plane is mandatory.

---

# Components Still Needing Selection

* Exact display panel
* Display FPC connector
* Scroll wheel encoder model
* USB-C connector part number
* Headphone jack part number
* Battery supplier
* ESD protection devices
* TVS protection devices
* Ferrite beads
* Test point footprints

---

# Last Updated

Lemming v1 hardware planning phase.
