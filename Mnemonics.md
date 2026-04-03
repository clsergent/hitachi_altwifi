
| **Mnemonics** | Status | Meaning | Unit | Regex | Values | STS_IDU(TX) | STS_IDU (RX) | STS_ODU (TX) | STS_ODU (RX) | CMD_IDU | Comment |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **81** | 17  | 31  |     |     |     | 41  | 34  | 35  | 3   | 14  |    |
| **4Way** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **A11** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **A12** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **ALM** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **Buzz** | ✅   | Buzzer |     |     |     | FALSE | FALSE | FALSE | FALSE | TRUE |     |
| **CapC** | ❗️  | Maximum unit power | kW  | \\d+(?:\\.\\d+) |     | TRUE | TRUE | FALSE | FALSE | FALSE | RAK-DJ18RH return 1.8  <br>RAK-DJ50RH return 5 |
| **CFq1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Comm** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **CycS** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **EVD** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **FANCD1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **FanHD** | ❓   | Fan Horizontal Direction |     |     |     | TRUE | FALSE | FALSE | FALSE | FALSE | There is also a FanVD. But no answer and doesn’t change fan orientation |
| **FanO** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **FanS** | ✅   | Fan Speed |     | \[0-5\] | auto = 0  <br>1/5 = 4  <br>2/5 = 3  <br>3/5 = 2  <br>4/5 = 1  <br>5/5 = 5 | TRUE | TRUE | TRUE | FALSE | TRUE |     |
| **FANS1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **FanSw** | ✅   | Fan Swing |     | \[0-1\] | stop = 0  <br>vertical = 1 | TRUE | TRUE | FALSE | FALSE | TRUE | Horizontal / both not tested (probably 2 / 3) |
| **FanVD** | ❓   | Fan Vertical Direction |     |     |     | TRUE | FALSE | FALSE | FALSE | FALSE | There is also a FanHD. But no answer and doesn’t change fan orientation |
| **FilS** | ✅   | Filter Status 1 reset FltT |     | \[01\] | Normal = 0  <br>reset FltT = 1 | TRUE | TRUE | FALSE | FALSE | TRUE | After sending FilS=1, FltT reset from 121 to 0  <br>FilS query return 0 even after setting to 1 |
| **FltT** | ❗️  | Filter use hours | H   | \\d+ |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **Fota** | ❓   | Factory OTA |     |     |     | TRUE | FALSE | FALSE | FALSE | FALSE | “WiFi FCT: Factory OTA” and other equivalent strings found in firmware |
| **Func1** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **Func2** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **H1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | TRUE | FALSE |     |
| **H2** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **HExT** | ❗️  | Heat Exchanger Temperature | °C  | \\d{1,2}(?:\\.5) | Indoor unit: 0.5°C steps  <br><br/>Outdoor unit: 1°C steps | TRUE | TRUE | TRUE | TRUE | FALSE | Indoor values are lower than ambiant temperature which is coherent for an evaporator  <br><br/>Outdoor temperature rise above external temperature when external unit is switched on (except one time, why?) which is coherent for a condenser |
| **HI1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Hr** | ✅   | Indoor Humidity (Humidity read) | %   | \\d{1,2} |     | TRUE | TRUE | FALSE | FALSE | FALSE | Continuous values in the right range  <br>Old Hlink protocol seemed to return humidity through P0101 so it is relevant to get such a value |
| **Hs** |     | ?   |     |     |     | TRUE | FALSE | FALSE | FALSE | TRUE | Possibly humidity set (found in firmware) |
| **iE** |     | ?   |     |     |     | TRUE | FALSE | FALSE | FALSE | FALSE |     |
| **ImOVL** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **INVCD1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **INVS1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **IsOVL** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **ItW** | ❓   | Inverter Work |     | \[01\] | 0 = AC unit not working  <br>1 = AC unit working | TRUE | TRUE | FALSE | FALSE | FALSE | When cooling or heating is set, the value turns to 1 after a short period, when power increase |
| **Maint** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE | Possible maintenance status (ie. Errors) |
| **MalC** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **MdDtl** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **Mode** | ✅   | AC mode |     |     | Cool = 64  <br>Fan = 81  <br>Dry = 33  <br>Heat = 16  <br>Auto =33024 | TRUE | TRUE | FALSE | FALSE | TRUE |     |
| **Modl** | ✅   | Model name |     | \\w+ |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **NeTU** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **oE1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **OnOf** | ✅   | Unit power on/off |     | \[01\] |     | TRUE | TRUE | FALSE | FALSE | TRUE |     |
| **Opt1** |     | Options |     | \\d+ | ECO = 1 | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **Opt2** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE | Doesn’t change (0) when setting ECO/BOOST/SILENT |
| **Opt3** | ❗️  | Options |     | \\d+ | GoodSleep = 64 (+ Opt4=128) | TRUE | TRUE | FALSE | FALSE | FALSE | GoodSleep has a timer on the remote, which has not been found so far |
| **Opt4** | ✅   | Options |     | \\d+ | Silent = 128  <br>Boost = 64  <br>Away = 1 | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **OtUT** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **PFa1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **PinR** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **PrtI** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Pwd** | ✅   | WiFi password |     | \\w+ |     | FALSE | FALSE | FALSE | FALSE | TRUE |     |
| **PwrC** | ❗️  | Power Cumulative = Total Energy | Wh  | \\d+ |     | TRUE | TRUE | FALSE | FALSE | FALSE | Value is always increasing. The high the load, the faster the increase → total increasing of energy (Wh?) |
| **PwrI** | ❗️  | Power Instantaneous | W   | \\d+ |     | TRUE | TRUE | FALSE | FALSE | FALSE | Value is coherent with actual power consumption |
| **PwrZ** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **Rair** | ✅   | Air flow | L/s | \\d+ |     | TRUE | TRUE | FALSE | FALSE | FALSE | Increase with fan speed, boost, silence, etc.  <br>Max recorded value is 101, which means it’s not %  <br><br/>Units could be L/s  <br>Spec for DJ400 5kW:  <br>265 m³/h ÷ 3,6 = 74 l/s  <br>360 m³/h ÷ 3,6 = 100 l/s  <br>528 m³/h ÷ 3,6 = 147 l/s  <br>608 m³/h ÷ 3,6 = 169 l/s  <br>663 m³/h ÷ 3,6 = 184 l/s |
| **RlMd** | ✅   | Real mode (HVAC) |     | \\d+ | 33088 = cool  <br>33024 = 0  <br>33040 = heat  <br>Other values from Mode | TRUE | TRUE | FALSE | FALSE | FALSE | RlMd has almost the same values as Mode  <br>In Heat/Cool mode (33024),  <br>33024 = 0b10000001 00000000  <br>Removing the upper byte give the real mode  <br>33040 & 0xff = 16 → heat  <br>33024 & 0xff = 0 → idle  <br>33088 & 0xff = 64 → cool |
| **RmCt** |     | ?   |     |     |     | TRUE | FALSE | FALSE | FALSE | FALSE | Maybe Remote Control Lock (but no answer) |
| **RomN** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **RunI** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **RunS** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **RuSt** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **SetT** | ✅   | Target temperature | °C  | \\d{1,2}(?:\\.5) |     | TRUE | TRUE | FALSE | FALSE | TRUE |     |
| **SetTR** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **SSID** | ✅   | Wifi SSID |     | \\w+ |     | FALSE | FALSE | FALSE | FALSE | TRUE |     |
| **Stas** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Stat** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE |     |
| **SwrZ** | ❓   | Software version |     | \\w+ |     | TRUE | TRUE | FALSE | FALSE | FALSE | Same value on all my units |
| **Ta** | ✅   | Outdoor temperature | °C  | \\d{1,2} |     | FALSE | FALSE | TRUE | TRUE | FALSE | Continuous and relevant values. |
| **Td** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Td1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Tg1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Thmo** | ❓   | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE | 1 when unit is ON and cooling, 0 when not cooling |
| **Time** | ✅   | Set indoor unit time |     | \\d{2}/\\d{2}/\\d{4} \\d{2}:\\d{2}:\\d{2} | Jj/mm/aaaa HH:MM:SS | FALSE | FALSE | FALSE | FALSE | TRUE |     |
| **To** |     | ?   |     |     |     | TRUE | FALSE | FALSE | FALSE | FALSE |     |
| **TPID** |     | ?   |     |     |     | TRUE | TRUE | FALSE | FALSE | FALSE | "%s TPID:%d(%s) IDU identified. » found in firmware |
| **tqOVL** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **Tr** | ✅   | Indoor temperature | °C  | \\d{1,2} |     | TRUE | TRUE | FALSE | FALSE | FALSE | Continuous and relevant values |
| **TYPE** |     | ?   |     |     |     | FALSE | FALSE | FALSE | FALSE | TRUE | “TYPE: 0x%x” found in firmware, which suggest a hex value |
| **UJ1** |     | ?   |     |     |     | FALSE | FALSE | TRUE | FALSE | FALSE |     |
| **WSt** | ❓   | WiFi Status LED |     | \[0-3\] | OFF = 0  <br>ON = 1 | FALSE | FALSE | FALSE | FALSE | TRUE |     |
| **WtmS** | ❗️  | Timer LED |     | \[0-2\] | OFF = 0  <br>ON = 1 \| 2 | FALSE | FALSE | FALSE | FALSE | TRUE | Meaning could be WiFi timer Status  <br>Cannot be set if WiFi LED is off |
