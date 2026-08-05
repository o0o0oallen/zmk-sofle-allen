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

## Useful OLED options

Central/left widgets:

```conf
CONFIG_NICE_OLED_WIDGET_LAYER=y
CONFIG_NICE_OLED_WIDGET_WPM=y
CONFIG_NICE_OLED_WIDGET_WPM_NUMBER=y
CONFIG_NICE_OLED_WIDGET_WPM_SPEEDOMETER=n
CONFIG_NICE_OLED_WIDGET_WPM_GRAPH=n
CONFIG_NICE_OLED_WIDGET_WPM_BONGO_CAT=y
CONFIG_NICE_OLED_WIDGET_WPM_LUNA=n
CONFIG_NICE_OLED_WIDGET_HID_INDICATORS=n
CONFIG_NICE_OLED_WIDGET_MODIFIERS_INDICATORS=n
```

Peripheral/right animations:

```conf
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL=y
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_CAT=y
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_GEM=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_HEAD=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_POKEMON=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_SPACEMAN=n
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_SMART_BATTERY=y
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_MS=960
```

Display style/position tweaks:

```conf
CONFIG_NICE_OLED_WIDGET_INVERTED=n
CONFIG_NICE_OLED_WIDGET_OUTPUT_BACKGROUND=n
CONFIG_NICE_OLED_WIDGET_BATTERY_CUSTOM_X=0
CONFIG_NICE_OLED_WIDGET_BATTERY_CUSTOM_Y=50
CONFIG_NICE_OLED_WIDGET_BONGO_CAT_CUSTOM_X=64
CONFIG_NICE_OLED_WIDGET_BONGO_CAT_CUSTOM_Y=-9
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_CUSTOM_X=18
CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_CUSTOM_Y=-18
```

Host companion widgets are available but intentionally off because they need a computer-side app:

```conf
CONFIG_NICE_OLED_WIDGET_RAW_HID=n
CONFIG_NICE_OLED_WIDGET_RAW_HID_TIME=n
CONFIG_NICE_OLED_WIDGET_RAW_HID_VOLUME=n
CONFIG_NICE_OLED_WIDGET_RAW_HID_LAYOUT=n
CONFIG_NICE_OLED_WIDGET_RAW_HID_WEATHER=n
CONFIG_NICE_OLED_WIDGET_RAW_HID_MEDIA_PLAYER_SPOTIFY_MACOS=n
```

## Battery display

Each half shows its own local battery percentage on its own OLED.

The central-half options for showing peripheral battery are intentionally disabled:

```conf
CONFIG_NICE_OLED_WIDGET_CENTRAL_SHOW_BATTERY_PERIPHERAL_ALL=n
CONFIG_NICE_OLED_WIDGET_CENTRAL_SHOW_BATTERY_PERIPHERAL_ONLY=n
CONFIG_NICE_OLED_WIDGET_CENTRAL_SHOW_BATTERY_PERIPHERAL_AND_CENTRAL=n
```

## Split reconnect stability

The current config keeps deep sleep enabled, but delays it to 1 hour to reduce cases where the right/peripheral half wakes from long sleep and does not reconnect to the left/central half.

```conf
CONFIG_ZMK_BLE_EXPERIMENTAL_CONN=y
CONFIG_ZMK_SLEEP=y
CONFIG_ZMK_IDLE_TIMEOUT=60000
CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=3600000
```

If the peripheral still fails to reconnect after very long sleep, the more aggressive option is to disable deep sleep:

```conf
CONFIG_ZMK_SLEEP=n
```

That usually improves wake/reconnect behavior, but it can reduce battery life.

## Flashing order

1. Flash `nice_nano_sofle_left_oled.uf2` to the left/central half.
2. Flash `nice_nano_sofle_right_oled.uf2` to the right/peripheral half.

If anything goes wrong, restore from the UF2 backups in `backup/current-nicenano-left-central` and `backup/current-nicenano-right-peripheral`.
