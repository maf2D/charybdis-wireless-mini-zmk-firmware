# Charybdis Wireless Mini ZMK Firmware

## Hardware

- **Keyboard:** Bastard Keyboards Charybdis Wireless Mini (3x6 split + trackball)
- **MCU:** nice!nano on both halves
- **Configuration:** Dongle format (3 devices: left half, right half, dongle)
- **Trackball sensor:** PMW3610 (low-power driver by badjeff)

## Project Structure

```
config/
  keymap/
    colemak_dh.keymap   ← active keymap (main file to edit for key changes)
    qwerty.keymap        ← alternate layout
    behaviors.dtsi       ← custom behaviors (home-row mods, tap-dance, etc.)
    combos.dtsi          ← combo definitions
    macros.dtsi          ← macro definitions
  charybdis_pointer.dtsi ← trackball speed, scroll, acceleration config
  charybdis_pmw3610.dtsi ← sensor driver config
  charybdis-layouts.dtsi ← physical key positions (for ZMK Studio)
  charybdis.conf         ← global firmware config flags

boards/shields/
  charybdis_dongle/      ← dongle shield files
  charybdis_bt/          ← bluetooth shield files (not used in dongle setup)

build.yaml               ← GitHub Actions build matrix (shields, keymaps, formats)
local-build/             ← Docker-based local build (docker-compose run --rm builder)
keymap-drawer/           ← SVG visual renders of the keymap
extra-keymaps/           ← additional layout options (canary, focal, graphite)
```

## Layers (colemak_dh.keymap)

| # | Name    | Purpose                                          |
|---|---------|--------------------------------------------------|
| 0 | BASE    | Colemak DH with timeless home-row mods           |
| 1 | NUM     | Numbers + F-keys                                 |
| 2 | NAV     | Arrows, paging, TMUX, mouse pointer              |
| 3 | SYM     | Symbols and punctuation                          |
| 4 | GAME    | Gaming layer (no mods)                           |
| 5 | EXTRAS  | BT profiles, media, shortcuts, macros            |
| 6 | SLOW    | Slow pointer speed (pass-through layer)          |
| 7 | SCROLL  | Scroll mode (pass-through layer)                 |

## Key Layout Reference (position indices)

```
╭──────┬──────┬──────┬──────┬──────┬──────╮  ╭──────┬──────┬──────┬──────┬──────┬──────╮
   00     01     02     03     04     05        06     07     08     09     10     11
├──────┼──────┼──────┼──────┼──────┼──────┤  ├──────┼──────┼──────┼──────┼──────┼──────┤
   12     13     14     15     16     17        18     19     20     21     22     23
├──────┼──────┼──────┼──────┼──────┼──────┤  ├──────┼──────┼──────┼──────┼──────┼──────┤
   24     25     26     27     28     29        30     31     32     33     34     35
╰──────┴──────┴──────┼──────┼──────┼──────┤  ├──────┼──────┼──────┴──────┴──────┴──────╯
                        36     37     38        39     40
                     ╰──────┴──────┴──────╯  ╰──────┴──────╯
```

Left half: keys 00–05, 12–17, 24–29, thumb cluster 36–38
Right half: keys 06–11, 18–23, 30–35, thumb cluster 39–40

## Building Firmware

**Local (Docker):**
```sh
cd local-build
docker-compose run --rm builder
```

**GitHub Actions:** push changes → Actions tab → download artifacts

## Flashing Order (dongle setup)

1. Flash `settings_reset` to all 3 devices first (when switching configs)
2. Flash `charybdis_left` uf2 → left half
3. Flash `charybdis_right` uf2 → right half
4. Flash dongle uf2 → dongle

Double-press reset button on a device to enter bootloader (mounts as USB storage).

## Common Edit Locations

- **Change a key binding:** `config/keymap/colemak_dh.keymap`
- **Add/edit a macro:** `config/keymap/macros.dtsi`
- **Add/edit a combo:** `config/keymap/combos.dtsi`
- **Add/edit a behavior (tap-dance, hold-tap):** `config/keymap/behaviors.dtsi`
- **Adjust trackball speed/scroll:** `config/charybdis_pointer.dtsi`
