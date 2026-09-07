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

## ER-TFT032IPS-3.2 (3.2" 240×320 IPS TFT)

| Field             | Value                                      |
| ----------------- | ------------------------------------------ |
| Manufacturer      | EastRising                                 |
| Part Number       | ER-TFT032IPS-3.2                           |
| Function          | Primary display                            |
| Resolution        | 240 × 320                                  |
| IC                | ST7789V2                                   |
| Interface         | 3-wire SPI, 4-wire SPI, 8080 8/16-bit Parallel |
| Display Type      | IPS TFT-LCD Color                          |
| Diagonal Size     | 3.2"                                       |
| Outline Dimension | 55.04 × 77.50 × 2.5 mm                     |
| Visual Area       | 50.20 × 66.40 mm                           |
| Active Area       | 48.60 × 64.80 mm                           |
| Pixel Pitch       | 0.2025 × 0.2025                            |
| IC Package        | COG                                        |
| Connection        | Plug-in FPC, 40-pin 0.50mm pitch ZIF connector |
| Contrast Ratio    | 800:1                                      |
| Colors            | 65K / 262K                                 |
| Viewing Angle     | IPS, 80° all directions                    |
| Brightness        | 250 cd/m²                                  |
| Backlight Color   | White                                      |
| Backlight Current | 120 mA typical                             |
| Power Supply      | 2.8 V typical                              |
| Touch Panel       | Optional capacitive (FT6236)               |
| Operating Temp    | -20°C ~ 70°C                               |
| Storage Temp      | -30°C ~ 80°C                               |
| Status            | Selected                                   |

### Why

* IPS panel with 80° viewing angle in all directions
* 240 × 320 resolution fits the project's pixel-art album art design
* ST7789V2 is well-supported in embedded Rust display drivers
* Long-term continuity supply guaranteed until at least 2033
* Low BOM cost (~US$8.97)

### Alternatives

| Part                   | Notes                  |
| ---------------------- | ---------------------- |
| ER-TFT032A3-3-4334      | Same size/resolution, different connector footprint |
| ILI9341-based 3.2" panel | Wider software support, TN panel |

### Notes

Touch panel is included by default with capacitive controller FT6236.

Firmware should disable touch if unused to reduce power consumption.

ZIF connector is 40 pins at 0.50mm pitch. PCB footprint must match ER-CON40HT-1 drawing.

Interface: firmware should default to 4-wire SPI for simplicity. Parallel interfaces are available if bandwidth becomes a bottleneck.

Backlight current is 120 mA at full brightness. Consider a brightness limit during battery operation.

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
