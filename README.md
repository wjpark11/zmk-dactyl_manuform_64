# ZMK Dactyl Manuform 64 Configuration

This repository contains the ZMK firmware configuration for a Dactyl Manuform 64-key compatible split keyboard using a SuperMini nRF52840 controller.

This keyboard is not confirmed to be an official TaikoHub unit. It uses the same compatible Dactyl Manuform 5x6 shield configuration and has been tested successfully with this firmware.

The keymap has been ported from a previous Keyball61 layout, keeping the main QWERTY-style layer, navigation/media/macro layers, Korean language toggle, and Python/Django macro combos where possible.

## Hardware

- Keyboard: Dactyl Manuform 64-key / 5x6 split-compatible keyboard
- Controller: SuperMini nRF52840
- ZMK board target: `nice_nano//zmk`
- Shields:
  - `dactyl_manuform_5x6_left`
  - `dactyl_manuform_5x6_right`
- Reset firmware target:
  - `settings_reset`

The shield uses a Pro Micro-compatible pinout. SuperMini nRF52840 works with this configuration when wired as a Pro Micro/nice!nano-compatible controller.

## Current Hardware Support

Confirmed working:

- left/right split keyboard scanning
- BLE firmware built through GitHub Actions
- SuperMini nRF52840 flashing
- Keyball61-derived keymap layers, combos, and macros

Not supported or not verified on this keyboard:

- LED/RGB underglow
- OLED display
- encoders
- physical trackball or pointing sensor hardware

## Files To Edit

For normal key layout changes, edit only:

```text
config/dactyl_manuform_5x6.keymap
```

This file contains:

- `DEFAULT` layer
- `NAV` layer
- `MEDIA` layer
- tap-hold settings for `&mt` and `&lt`
- combos
- macros

For firmware behavior settings such as BLE strength, sleep, or macro queue size, edit:

```text
config/dactyl_manuform_5x6.conf
```

LED/RGB options may appear in the config comments, but they are not expected to work on this keyboard unless the actual LED hardware and wiring are confirmed and the matching ZMK overlay/config is added.

Do not edit the shield overlays unless the physical wiring, matrix pins, diode direction, or hardware features have changed.

## Build

Firmware is built by GitHub Actions on every push.

The build matrix is defined in:

```text
build.yaml
```

It currently builds:

- left half firmware
- right half firmware
- settings reset firmware

After pushing changes, open the repository's Actions tab and download the generated firmware artifact when the workflow completes.

## Flashing

1. Download the firmware zip from GitHub Actions artifacts.
2. Extract the zip.
3. Put the left half into bootloader mode and copy the left UF2 file to it.
4. Put the right half into bootloader mode and copy the right UF2 file to it.
5. If pairing or BLE state becomes inconsistent, flash `settings_reset` first, then flash the left and right firmware again.

## Common Workflow

```powershell
# Edit the keymap
code config\dactyl_manuform_5x6.keymap

# Review changes
git diff

# Commit and push
git add config\dactyl_manuform_5x6.keymap
git commit -m "Update keymap"
git push origin main
```

## Notes

- The repository tracks ZMK `main`, so upstream ZMK changes can affect build behavior.
- The current board target uses the newer ZMK board id `nice_nano//zmk`, not the older `nice_nano_v2` name.
- This repo is tuned for the tested keyboard, not for every Dactyl Manuform variant.
