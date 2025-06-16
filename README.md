# Hitachi_altwifi

## Overview
Replacing the vendor firmware of the Hitachi AirHome 400 (and possibly other models) WiFi module with a custom version based on ESPHome.

Since recent versions of Hitachi AC are shipped with integrated WiFi connectivity, it is not necessary to add custom hardware. Instead, it is possible to flash a custom firmware onto the WiFi module.

<img width="600" alt="module" src="https://github.com/user-attachments/assets/8613b39a-e10b-45de-9213-25daf84a22f0" />


## Usage
Follow [the instructions](https://github.com/clsergent/hitachi_altwifi/blob/main/Instructions.md) to flash your custom firmware.

Once done, next updates can be done OTA using Esphome. No special instructions are required then.

[Libretiny documentation](https://docs.libretiny.eu/docs/platform/realtek-ambz/#flashing) states: "Because the UART uploading code is programmed in the ROM of the chip, it can't be software-bricked, even if you damage the bootloader". This means that it should be safe to flash custom firmware. However, you remain responsible if any problems occur.

Use [hitachi_altwifi.yaml](https://github.com/clsergent/hitachi_altwifi/blob/main/hitachi_altwifi.yaml) as a package in your main esphome yaml file (set the climate name using the custom var `ac_name`):
```
packages:
  hitachi_ac: !include
    file: hitachi_altwifi.yaml
    vars:
      ac_name: ${ac_name}
```


## Compatibility list
This list references tested models only. If another model is compatible, please report it to help extend this list.
- RAK-DJ18RH
- RAK-DJ50RH

## Credits
Custom firmware is based on [esphome-hlink-ac](https://github.com/lumixen/esphome-hlink-ac).
This repository is not affiliated with Hitachi in any way.

## License
This repository and its content are licensed under the EUPL-1.2-or-later.

Check https://joinup.ec.europa.eu/collection/eupl/eupl-text-eupl-12
