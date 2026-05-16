# C Black Firmware Recovery Notes

Last verified: 2026-05-16

This repository is for an AliExpress Eyelash Peripherals style Corne C black
keyboard, not a standard foostan Corne. The working target is the seller/custom
Eyelash Corne configuration.

## Final Working Combination

Use branch:

```text
codex/c-black-niceview-split-reset
```

Working downloaded UF2 files:

```text
C:\Users\k9503\Downloads\c-black-niceview-reset_c_black_reset_left.uf2
C:\Users\k9503\Downloads\c-black-niceview-reset_c_black_reset_right.uf2
C:\Users\k9503\Downloads\c-black-niceview-reset_c_black_niceview_left.uf2
C:\Users\k9503\Downloads\c-black-niceview-reset_c_black_niceview_right.uf2
```

Optional left firmware with ZMK Studio enabled:

```text
C:\Users\k9503\Downloads\c-black-niceview-reset_c_black_niceview_studio_left.uf2
```

Recommended normal flash order:

```text
1. Flash c_black_reset_left.uf2 to the left half.
2. Flash c_black_reset_right.uf2 to the right half.
3. Flash c_black_niceview_left.uf2 to the left half.
4. Flash c_black_niceview_right.uf2 to the right half.
5. Power both halves off.
6. Power the right half on.
7. Wait about 5 seconds.
8. Power the left half on and connect USB-C to the left half.
9. Wait 20-30 seconds for split pairing.
```

## What Was Wrong

The first attempts used standard ZMK Corne targets such as:

```text
nice_nano//zmk + corne_left
nice_nano//zmk + corne_right
nice_nano_v2 + settings_reset
```

Those builds can compile and the left half may appear as a USB HID device, but
they do not match this keyboard. Symptoms included:

- USB device detected but key input did not work.
- Display showed garbage, went blank, or behaved inconsistently.
- Left half eventually worked, but the right half would not send keys through
  split.

The product is closer to the seller's Eyelash Corne config:

```text
https://github.com/a741725193/zmk-new_corne
```

The board target must be:

```text
eyelash_corne_left
eyelash_corne_right
```

not standard `nice_nano` plus standard `corne_left/right`.

## Important Discovery

The screen that visually looked like an OLED-style display worked with the
`nice_view` shield in the seller's `zmk-new_corne` configuration:

```yaml
- board: eyelash_corne_left
  shield: nice_view
- board: eyelash_corne_right
  shield: nice_view
```

The separate `zmk-corne-oled` / `nice_oled` path was useful for diagnostics, but
it was not the final working display configuration. With that path, the right
half matrix could be tested, but the screen did not display correctly on the
actual keyboard.

## Split Pairing Root Cause

The key final fix was not only using `eyelash_corne_left/right + nice_view`.
Split pairing also required reset firmware built for the same custom board
targets:

```yaml
- board: eyelash_corne_left
  shield: settings_reset
  artifact-name: c_black_reset_left

- board: eyelash_corne_right
  shield: settings_reset
  artifact-name: c_black_reset_right
```

Earlier resets built for `nice_nano_v2` were not reliable for this keyboard's
custom board/storage layout. The left half could work, but the right half would
not pair correctly. After flashing the left/right-specific reset UF2s first,
then flashing the matching left/right `nice_view` firmware, both halves paired
and both displays worked.

## Diagnostic Results

Right-half USB diagnostic build:

```text
c-black-oled-test_c_black_right_usb_test.uf2
```

Result:

```text
Right half keys worked when flashed as a temporary USB-central diagnostic.
```

Meaning:

- The right half key matrix, switches, and controller pins were not the main
  problem.
- The remaining issue was split pairing / wrong reset target / firmware target
  mismatch.

## Avoid These For This Keyboard

Do not treat this keyboard as a stock Corne:

```text
nice_nano_v2 + corne_left/corne_right
nice_nano//zmk + corne_left/corne_right
```

Do not use `nice_nano_v2` settings reset as the primary recovery reset for the
final C black firmware. Use the custom left/right reset files instead:

```text
c_black_reset_left.uf2
c_black_reset_right.uf2
```

## Current Build Matrix

The final useful `build.yaml` shape is:

```yaml
include:
  - board: eyelash_corne_right
    shield: nice_view
    artifact-name: c_black_niceview_right
  - board: eyelash_corne_left
    shield: nice_view
    artifact-name: c_black_niceview_left
  - board: eyelash_corne_left
    shield: nice_view
    snippet: studio-rpc-usb-uart
    cmake-args: -DCONFIG_ZMK_STUDIO=y -DCONFIG_ZMK_STUDIO_LOCKING=n
    artifact-name: c_black_niceview_studio_left
  - board: eyelash_corne_left
    shield: settings_reset
    artifact-name: c_black_reset_left
  - board: eyelash_corne_right
    shield: settings_reset
    artifact-name: c_black_reset_right
```

