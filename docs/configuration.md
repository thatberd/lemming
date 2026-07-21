# Configuration Philosophy

lemming separates configuration into three concerns: behavior, appearance, and structure.

This separation ensures that customization is transparent, safe, and well-defined.

## Behavior

Behavior settings describe how the player operates.

Stored in TOML configuration files on the SD card.

Examples:

- Bluetooth
- ReplayGain
- Library scanning
- Sleep timer
- Audio settings
- Advanced options

Behavior settings are read by the firmware at startup and whenever the file changes.

## Appearance

Appearance settings describe how the player looks.

Stored as themes on the SD card.

Themes define presentation only:

- Colors
- Icons
- Fonts (future)
- Borders
- Spacing

Themes must never execute code. A theme is a data file, not a program.

## Structure

Structure settings describe how the interface is organized.

Defined by page descriptions compiled through lmngdsl.

Pages define interface structure. Pages do not contain logic. Pages do not execute code. Pages simply describe layout and composition.

## Principles

- Configuration is transparent. Users can read and edit configuration files directly.
- Prefer open standards. TOML for behavior. Declarative text for structure.
- Configuration owns customization. Firmware owns behavior.
- If a configuration option requires typing on the device, it probably belongs on a companion app or should be removed.
