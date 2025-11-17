# Instructions

## Overview
These instructions are intended to replace the stock firmware of the Hitachi AirHome 400 wifi module with a custom one based on esphome to integrate into Home Assistant.

## Initial checks
Check if your setup applies. It *will* work your if your device passes all checks. Otherwise it *may* work. Open an issue if necessary.

1. WiFi board: 
check your onboard wifi module. It should be labelled **Model: RTL8710BN**.

<img width="700" alt="module" src="https://github.com/user-attachments/assets/f69a4524-050b-4a30-9bd2-61f0850b6206" />


2. Protocol:
check in your installation documentation that the CN7(X) port is dedicated to the H-LINK protocol.

3. Pinout:
check that your wifi module is connected to a six-pins CN7(X) port on the mainboard, like on the following scheme from RAK-DJ60-70RHAE (which has 2 ports: CN7A and CN7B). 

![mainboard](https://github.com/user-attachments/assets/ed16aa07-6168-479a-b28b-9ec6bc10143d)


## Connections
Flashing the board requires to connect a USB-TTL converter (3.3V logic level) to the WiFi chip (a RTL8710BN). It can achieved using the series of holes on the right of the board.

![mainboard_pins](https://github.com/user-attachments/assets/eb7b162b-9434-492c-9c91-3cad8495fbbd)



Connect the USB-TTL converter (FT232RL is known to work, PL2303 *will probably* fail) :
| Converter  | RTL8710B |
| --- | --- |
| RX  | TX2 (Log_TXD / PA30) |
| TX  | RX2 (Log_RXD / PA29) |
| GND | GND |

Connect a **dedicated** 3V3 source (GND to GND and 3V3 to 3V3) as the USB-TTL converter source *may* be insufficient during heady read-write operations.

Connections can be done by soldering Dupont wires to the holes on the right of the board. Be cautious as  spacing is only 0.05″. Otherwise use a special probe.

## Entering download mode
[ltchiptool website](https://docs.libretiny.eu/docs/platform/realtek-ambz/#wiring) explains how to enter download mode. It can be achieved using the CEN pin, or more easily through a special power-up sequence:
1. ground LOG_TXD
2. power the board
3. unground LOG_TXD

## Backup 
1. Download and install [ltchiptool](https://docs.libretiny.eu/docs/flashing/tools/ltchiptool)
2. Get chip information using `ltchiptool flash info ambz`.  Response should be as follows (check that chip version and flash size match the example) :
```
I: Transmission successful (ACK received).
I: Transmission successful (ACK received).
I: |-- Success! Chip info: RTL8710BN
I: Reading chip info...
I: Chip: RTL8710BN
I: Transmission successful (ACK received).
I: Transmission successful (ACK received).
I: +---------------------+--------------------------------+
I: | Name                | Value                          |
I: +---------------------+--------------------------------+
I: | Chip Type           | RTL8710BN                      |
I: | MAC Address         |               |
I: |                     |                                |
I: | Flash ID            | EF 40 15                       |
I: | Flash Size (real)   | 2 MiB                          |
I: |                     |                                |
I: | OTA2 Address        | 0x80D3000                      |
I: | RDP Address         | 0x81FF000                      |
I: | RDP Length          | 0xFF0                          |
I: | Flash SPI Mode      | QIO                            |
I: | Flash SPI Speed     | 100MHZ                         |
I: | Flash ID (system)   | FFFF                           |
I: | Flash Size (system) | 2 MiB                          |
I: | LOG UART Baudrate   | 115200                         |
I: |                     |                                |
I: | SYSCFG 0/1/2        | 40000200 / 02010301 / 00000001 |
I: | ROM Version         | V0.1                           |
I: | CUT Version         | 0                              |
I: +---------------------+--------------------------------+
I: |-- Finished in 16.036 s
```
3. Dump the stock firmware as a backup: `ltchiptool flash read ambz dump.bin -b 115200`

Lowering baudrate is *advised* to avoid read-write errors (default baudrate is as high as 1.5MB).

## Prepare the custom firmware
1. Choose the right device in Esphome interface : 

| parameter  | value |
| --- | --- |
| device type | RTL87xx |
| version board  | Generic - RTL8710BN (2MB/788k) |

2. Build your firmware according to the instructions from this [repository](https://github.com/lumixen/esphome-hlink-ac). Set parameters as follows:

| parameter  | value |
| --- | --- |
| tx_pin | GPIO23 (UART_TXD) |
| rx_pin  | GPIO18 (UART_RXD) |
| baud_rate | 9600 |
| parity | ODD |

## Flash the custom firmware
1. Grab the UF2 version of the firmware
2. Enter download mode once again
3. Flash the firmware (set the correct filename): `ltchiptool flash write firmware.uf2 -b 115200`

Next updates will be done OTA.
