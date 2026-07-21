# System UI

lemming separates the user interface into two distinct domains: firmware-owned UI and page-owned UI.

## Firmware-Owned UI

The firmware owns all global system information displayed on the device.

This includes:

- Status bar
- Battery indicator
- Clock
- Bluetooth status
- Playback status
- Active page indicator
- Other device state

Firmware-owned elements are rendered by the system layer and are always visible or available according to system rules.

## Page-Owned UI

Pages own only their content area.

A page describes the layout and composition of its own content. It does not render battery indicators, clocks, or other global system information.

Pages should never directly render battery indicators or other global system information.

## Status Bar Configuration

The status bar itself is configurable while remaining firmware-owned.

Users can rearrange, show, or hide status bar elements through configuration. The firmware still renders and owns those elements.

This means:

- The status bar structure can be customized.
- The status bar behavior is fixed by firmware.
- Pages cannot add items to the status bar.

## Why This Separation Exists

Global system state is owned by the firmware because:

- It represents device state, not application state.
- It must remain accessible regardless of which page is active.
- It has strict timing and power management requirements.
- It must not depend on page-level logic.

Pages focus on content because:

- Content is what users customize.
- Content is what changes between pages.
- Content is what benefits from user-defined structure.
