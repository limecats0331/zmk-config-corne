# Corne ZMK config

## Resources
- [Default corne config](https://github.com/zmkfirmware/zmk/blob/main/app/boards/shields/corne/corne.conf)
- [Corne Cherry v3 build guide](https://github.com/foostan/crkbd/blob/main/corne-cherry/doc/v3/buildguide_en.md)
- [CORNE BUILD GUIDE by devpew](https://devpew.com/blog/corne-eng/)

## My Corne Cherry v3 setup
- ✅ [Corne Cherry v3 board](https://kriscables.com/product/corne-cherry-kit/)
- ✅ [nice!nano V2 Wireless Controller](https://kriscables.com/product/nicenano/)
- ✅ [OLED Display Module 0.91″ 128×32](https://kriscables.com/product/oled-display/)
- ✅ [SK6812 MINI-E RGB LEDs 3535 for backlight](https://kriscables.com/product/sk6812mini-e-rgb-leds/)
- ✅ [WS2812B RGB LEDs 5050 for underglow](https://kriscables.com/product/ws2812b-rgb-led/)

## Tool
- [ZMK](https://zmk.dev/)
- [Keymap editor web](https://nickcoutsos.github.io/keymap-editor/)

## Backlight

- [Configurations](https://github.com/zmkfirmware/zmk/pull/1895/files#diff-cbfa08fa13ebf5e1f3e9ffbe7ac696a072575071a36bf6ab139d7a152ab38811R166)

For this set up, the backlight is actually also used as a frontlight. The SK6812 MINI-E RGB LEDs 3535 are mounted on the top of the PCB, facing the keycaps. The WS2812B RGB LEDs 5050 are mounted on the bottom of the PCB, facing the desk. Since ZMK doesn't support per-key RGB backlighting yet, I have to use the same LEDs for both backlight and frontlight.

## Troubleshooting

### No key input over USB-C
For this split build, plug USB-C into the left half running `corne_left`. The
right half running `corne_right` is the peripheral side and may not show up as a
USB keyboard on its own.

If USB still does not type:

1. Flash `settings_reset` to both halves.
2. Flash `corne_left` to the left half and `corne_right` to the right half.
3. Plug USB-C into the left half.
4. Hold `mo 2 + mo 1`, then press the `OUT_USB` binding on Adjust.

### OLED remains blank after EP_OFF
ZMK displays can stay blank after external power is cut. The external power
state is stored in flash, so it can remain off even after reflashing firmware.

The Adjust layer includes these recovery bindings:
```
Hold mo 2 + mo 1, then press top-right key:    &ext_power EP_ON
Hold mo 2 + mo 1, then press middle-left key:  &ext_power EP_OFF
Hold mo 2 + mo 1, then press middle-right key: &ext_power EP_TOG
```

Flash the firmware, press the `EP_ON` binding, wait a few seconds, then reset
the keyboard. If the OLED is still blank, flash `settings_reset` to both halves,
then flash `corne_left` and `corne_right` again.
