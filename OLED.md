# Sofle OLED Configuration

This repo uses [`mctechnology17/zmk-nice-oled`](https://github.com/mctechnology17/zmk-nice-oled) for animated SSD1306 OLED widgets.

## Current setup

- Board: `nice_nano_v2`
- Shields: `sofle_left nice_oled`, `sofle_right nice_oled`
- Display: SSD1306 OLED
- RGB/LED: disabled/not configured

## Active OLED widgets

- Left/central half:
  - Output status
  - Local battery percentage
  - Active layer
  - WPM number
  - Bongo Cat WPM animation
- Right/peripheral half:
  - Output status
  - Local battery percentage
  - Cat animation
  - Smart battery animation

## Change animation

Edit `config/sofle.conf`.

For central WPM animation, enable only one of these:

```conf
CONFIG_NICE_OLED_WIDGET_WPM_BONGO_CAT=y
CONFIG_NICE_OLED_WIDGET_WPM_LUNA=n
```

or:

```conf
CONFIG_NICE_OLED_WIDGET_WPM_BONGO_CAT=n
CONFIG_NICE_OLED_WIDGET_WPM_LUNA=y
```

For peripheral animation, enable only one of these:

```conf
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_CAT=y
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_GEM=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_HEAD=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_POKEMON=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_SPACEMAN=n
```

## Battery display

Each half shows its own local battery percentage on its own OLED.

The central-half options for showing peripheral battery are intentionally disabled:

```conf
CONFIG_NICE_OLED_WIDGET_CENTRAL_SHOW_BATTERY_PERIPHERAL_ALL=n
CONFIG_NICE_OLED_WIDGET_CENTRAL_SHOW_BATTERY_PERIPHERAL_ONLY=n
CONFIG_NICE_OLED_WIDGET_CENTRAL_SHOW_BATTERY_PERIPHERAL_AND_CENTRAL=n
```

## Flashing order

1. Flash `nice_nano_sofle_left_oled.uf2` to the left/central half.
2. Flash `nice_nano_sofle_right_oled.uf2` to the right/peripheral half.

If anything goes wrong, restore from the UF2 backups in `backup/current-nicenano-left-central` and `backup/current-nicenano-right-peripheral`.
