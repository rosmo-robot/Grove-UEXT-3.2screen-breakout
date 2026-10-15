# Crowtail stand for ESP32 TFT Touch with Camera (ILI9341), 

A WIP breakout for [Makerfabs Esp32 with screen](https://www.makerfabs.com/esp32-3-2-inch-tft-touch-with-camera.html) Can be viewed in [Kicanvas](https://kicanvas.org/?repo=https%3A%2F%2Fgithub.com%2Frosmo-robot%2FGrove-UEXT-3.2screen-breakout%2Ftree%2Fmain%2FKicad)

Status: Early WIP draft

Built for the [open hardware electronics starter kit](https://github.com/rosmo-robot/robotics-resources/blob/main/guides/open-electronics/starter-kit.md)

## Mechanical
- 38.1 x 71 mm, 2 layers. Width equals the 15-pin header (15 x 2.54 mm).
- Right-angle 15-pin male header on the north edge, legs pointing off the edge into J1. Board lies flat behind the upright main board.
- Pin 1 (3V3) is on the LEFT, viewed from above with the header edge at the top. Match the labels on J1 before powering.
- Ports in two columns, pin rows vertical: J2 to J4 down the left edge, J5 to J9 down the right edge.

## Ports (see netlist.csv)
| Left | Crowtail module | GPIO | J1 pin | Right | Crowtail module | GPIO | J1 pin |
|---|---|---|---|---|---|---|---|
| J2 LIGHT | Light Sensor 2.0 | IO36 | 3 | J5 I2C | none in the kit | IO27 SCL, IO26 SDA | 14, 15 |
| J3 POT | Rotary Angle Sensor 2.0 | IO39 | 4 | J6 DHT11 | Temperature & Humidity 2.0 | IO18 | 12 |
| J4 STICK | RGB-LED (4x WS2812B) | IO32 | 6 | J7 BUZZER | Buzzer 2.0 | IO19 | 11 |
| | | | | J8 RELAY | Solid-State Relay 2.0 | IO23 | 9 |
| | | | | J9 BUTTON | Button 2.0 | IO25 | 8 |


J5 shares IO26 and IO27 with the on-board touch controller. Any I2C module added there must not clash with its address.

## UEXT 
2x5 male header [Olimex UEXT](https://www.olimex.com/Products/Modules/UEXT/)

| UEXT pin | Signal | GPIO | J1 pin | Shared with |
|---|---|---|---|---|
| 1 | 3V3 | n/a | 1 | all ports |
| 2 | GND | n/a | 2 | GND pour |
| 3 | TXD | IO22 | 7 | none |
| 4 | RXD | IO35 | 5 | none  |
| 5 | SCL | IO27 | 14 | J5 I2C |
| 6 | SDA | IO26 | 15 | J5 I2C |
| 7 | MISO | IO19 | 11 | J7 BUZZER |
| 8 | MOSI | IO23 | 9 | J8 RELAY |
| 9 | SCK | IO18 | 12 | J6 DHT11 |
| 10 | SSEL | IO5 | 13 | none  |

- Use UEXT or the shared Crowtail ports, not both at once.

## Electrical
- All ports 3V3 only. J1 has no 5 V.
- SIG2 is not connected except on I2C. IO21 is not connected. IO35, IO22, IO5 go to UEXT only.
- Signals on F.Cu. 3V3 on B.Cu (bus and two vertical spines). GND is a B.Cu pour.
- C1 10 uF and C2 100 nF sit near the header. I2C pull-ups are already on the main board.
- Remove the camera: the same GPIOs drive the camera bus.
- GPIO used by the main board and not available here: 12, 13, 14 (TFT and SD SPI), 15 (TFT CS), 33 (TFT DC), 4 (SD CS), 0 (touch interrupt), 1 and 3 (USB serial).

## Module notes at 3.3 V
- RGB-LED: rated 5 V. At 3.3 V it is below the WS2812B datasheet minimum supply (3.5 V). Run at low brightness and test colour accuracy.
- Solid-State Relay 2.0: Elecrow lists 3.3 V and 5 V control levels. Supply at 3.3 V is not verified. Output is rated 240 VAC / 4 A, DC switching not verified. Do not use the standard Crowtail Relay 2.0, which needs 5 V control.
- Other modules (Light, Rotary Angle, Temperature & Humidity, Buzzer, Button): not verified at 3.3 V.

