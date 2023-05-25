# EmonTx v4 for Energy Measurements

Adapted from original [emontx4](https://github.com/openenergymonitor/emontx4), see `patch.diff` for modifications.


## Install

we will use platform io to compile and flash the firmware.

1. install `pio` via `python -c "$(curl -fsSL https://raw.githubusercontent.com/platformio/platformio/master/scripts/get-platformio.py)"` (might consider a conda python environment for clean install or sudo install into the host system)

2. `cd firmware/EmonTxV4`, then `pio run` to compile the firmware

3. upload firmware to the device: `pio run -t upload`

(if you make changes to the firmware code, just recompile and upload.)

4. use the command to see output: `pio device monitor -b 115200 -f time --echo --eol=LF`


## Config

when you use the command `pio device monitor -b 115200 -f time --echo --eol=LF` it also accepts command line inputs, see `firmware/EmonTxV/EmonTxV4_config.ino` file for a complete list of supported commands.

for example: 

- set json output format: `j1` or disable by `j0`, then press enter.
- set datalogging period (frequency): `d0.1` (set sampling frequency to 0.1 s) or `d1.5` (to 1.5 s).

You can also use Python serial lib to programmably set the configs.

```
Available commands: 
l - list the settings 
r - restore sketch defaults 
s - save settings to EEPROM 
v - show firmware version 
z - zero energy values 
x - exit, lock and continue 
? - show this text again 
 
w<x> - turn RFM Wireless data off: x = 0 or on: x = 1 
b<n> - set r.f. band n = a single numeral: 4 = 433MHz, 8 = 868MHz, 9 = 915MHz (may require hardware change) 
p<nn> - set the r.f. power. nn - an integer 0 - 31 representing -18 dBm to +13 dBm. Default: 25 (+7 dBm) 
g<nnn>\t- set Network Group  nnn - an integer (OEM default = 210) 
n<nn> - set node ID n= an integer (standard node ids are 1..60) 
 
d<xx.x>\t- xx.x = a floating point number for the datalogging period  
c<n> - n = 0 for OFF, n = 1 for ON, enable voltage, current & power factor values to serial output for calibration.  
j<n> - turn JSON Serial format on (1) or off (0). 
f<xx> - xx = the line frequency in Hz: normally either 50 or 60 
k<x> <yy.y> <zz.z> 
 - Calibrate an analogue input channel: 
   x = a single numeral: 0 = voltage calibration, 1 = ct1 calibration, 2 = ct2 calibration, etc 
   yy.y = a floating point number for the voltage/current calibration constant 
   zz.z = a floating point number for the phase calibration for this c.t. (z is not needed, or ignored if supplied, when x = 0) 
   e.g. k0 256.8 
        k1 90.9 2.00 
a<xx.x>\t- xx.x = a floating point number for the assumed voltage if no a.c. is detected 
m<x> <yy>\t- meter pulse counting: 
    x = 0 for OFF, x = 1 for ON, <yy> = an integer for the pulse minimum period in ms. (y is not needed, or ignored when x = 0) 
t<x> - turn temperature measurement on or of: x = 0 or on: x = 1 
```

