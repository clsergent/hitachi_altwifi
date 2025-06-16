# Reconnaissance

## Physical Study
### Connection

[Indoor unit installation manual](https://tyco.widen.net/content/n7a7yzqkkn/pdf/IM_RAK-DJ60-70RHAE_FR_IA2355-A.pdf?u=oknhak)  
From the manual, we extract a vector diagram of the main board. By reducing the line thickness, we obtain the names of all the connectors:  

![mainboard](https://github.com/user-attachments/assets/59810426-32b2-4894-93a4-af0fbc5f1489)

CN7B is used by the reference, but the WiFi card is connected to CN7A.

We use a similar service manual ([unit RAK-VJ18PHAE](https://www.manualslib.com/manual/3661853/Hitachi-Rak-Vj18phae.html)) to retrieve the connection diagram.  

![Wiring](https://github.com/user-attachments/assets/c69a3c13-6f56-4163-8e4a-d01f6633b136)

CN7A and CN7B are both H-LINK connectors. The pinout logic is as follows: 

![connectors](https://github.com/user-attachments/assets/d63435e9-030e-447a-91d9-11194d828c79)

- pin 3 is marked WLAN power supply (not wired for the reference) but isolated from the circuit (8.84kΩ)
- pin 6 is marked WLAN/H-LINK contact (not used by the reference) and is grounded (0Ω)

### WiFi Module
<img width="794" alt="module" src="https://github.com/user-attachments/assets/b04ef9cb-2cbe-4f0c-bdae-8fb70f516577" />


This is an **RTL8710BN** chip

- it can be flashed with [LibreTiny](https://github.com/libretiny-eu/libretiny)
- it is possible to use [Esphome](https://docs.libretiny.eu/docs/platform/realtek-ambz/)

So essentially, the goal is to get the H-Link protocol working and to flash the chip correctly.

### WiFi module pinout

Using a multimeter, it appears that several chip pins are routed to a series of through-holes:

![pins_board](https://github.com/user-attachments/assets/d0fc8c62-b088-45cd-985d-519a24d0469c)

By soldering Dupont wires, the chip can be easily accessed. Another way is to use a special probe clip.

## Connection
### Connections

| PC  | RTL8710B |
| --- | --- |
| RX  | TX2 (Log_TX / PA30) |
| TX  | RX2 (Log_RX / PA29) |
| GND | GND |

### Utility
[ltchiptool](https://docs.libretiny.eu/docs/flashing/tools/ltchiptool)  
Use the family:
Realtek AmebaZ | realtek-ambz | ambz | RTL8710B / 0x22E0D6FC | framework-realtek-amb1

### Recommendations
- use an FTDI 232RL chip to handle the handshake, which is at a VERY high speed (1.5MBaud)
- use a sufficient power source (worked with a buck converter 12-24V to 3.3V and another FTDI converter dedicated to power)
- reducing the baudrate helps avoid read/write errors (reduce to 115200 Baud)

### Download mode
### Theoretical method
1. connect CEN to GND
2. connect TX2 to GND
3. release CEN from GND
4. release TX2 from GND

### Practical method
I used the alternative method, which doesn't require CEN

1. connect TX2 to GND
2. connect VCC
3. remove TX2 from GND

Note: the USB-TTL converter can be connected before the sequence if needed (but do not wire VCC)

## Commands
**lower the baudrate is required to avoid errors**
- info: `ltchiptool flash info ambz`
- dump: `ltchiptool flash read ambz dump.bin -b 115200`
- flash: `ltchiptool flash write firmware.uf2 -b 115200`


