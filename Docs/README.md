# Trackpad sensor

The trackpad sensor on the blackberry 9900 is Avago-ADBS-A320 optical finger navigation sensor.
Keywords: avago, adbs, a320, OFN, optical finger navigation.

### 0. Prior work on Blackberry sensors
  * Most comprehensive info on these trackpads I could find is here: https://hackaday.io/project/167075-thumbmouse/log/166999-of-trackballs-and-trackpads
  * Some info is on Arduino forums (I2C variant) https://forum.arduino.cc/t/blackberry-trackpad-connector-20-small-pins-plug-or-ribbon/291406
  * Some info is on Arduino forums (SPI variant) https://forum.arduino.cc/t/blackberry-joystick-navigation-key-hack/61454/13
  * Module which includes I2C trackpad but without connection details https://lectronz.com/products/bb-q20-keyboard-with-trackpad-usb-i2c-pmod

### 1. Pinout (FPC on the left, pads facing you)
The cable is 0.3mm dual-row FPC with notches (used to prevent slipping out of connector).


|Pin |Pin |Function|Wiring note|
|:---|---:|:---    |:---  |
|    | 13 |SHTDWN  |Used for powersave, connect to GND|
| 12 |    |GND     |      |
|    | 11 |i2c SCL |pull-up to VDDIO|
| 10 |    |MOTION_N|Interrupt out, active low|
|    |  9 |i2c SDA |pull-up to VDDIO|
| 8  |    |NRST    |Used on initialization, pull-up to VDDIO|
|    |  7 |GND     |      |
| 6  |    |VDDA, VDDIO, VLED+|+2.6V..3.3V|
|    |  5 |DVDD    |+1.8V |
| 4  |    |Backlight LED +|e.g. 3.3V|
|    |  3 |Backlight LED -|e.g. 200 Ohm resistor to MCU open drain output|
| 2  |    |Dome switch B||
|    |  1 |Dome switch A||

BTB connector: FH35C-17S-0.3SHW(50)

https://item.szlcsc.com/3111895.html

https://www.mouser.com/ProductDetail/Hirose-Connector/FH35C-17S-0.3SHW50?qs=zPSbg0dgZohBQpjy02xR9g%3D%3D


### 3. Interfacing the sensor

I2c address: `0x3B`  
`Product_ID (reg 0)`: `0x83`

I highly recommend reading the [datasheet](docs/Avago-ADBS-A320-datasheet.pdf) Notes on Power-up section (page 15).
It tells how to enable finger presence detection, configure speed switching, enable i2c burst mode.

Also datasheet describes how to change X/Y orientation in reported data.
