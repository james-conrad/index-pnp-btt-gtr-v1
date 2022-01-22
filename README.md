# Fork for Marlin 2.0 for using a BTT GTR V1 to drive an Index PnP

## Software Setup

Marlin configured with 5 linear axes (X, Y, Z, A, B)

X, Y, Z motor axes as is.
E0 motor is configured as A axis.
E1 motor is configured as B axis.
E2 motor is configured as Y2 axis.

Configured for 6x TMC2130 drivers in SPI mode.
Configured to use Stall Guard feature for homing X, Y and Z axes.
Configured to use SPI endstops (feature specific to TMC2130).

DIRECT_PIN_CONTROL enabled to allow driving vacuum pump and valves.

NEOPIXEL_LED defined
NEOPIXEL_PIN defined as PF13
NEOPIXEL_PIXELS set to 22 (11 for each ring light)
NEOPIXEL_TYPE set to NEO_GRB

Note: Marlin needed a small fix to support dual motor axes homing using SPI endstops.

## Hardware Setup

### Power to BTT GTR V1
24V to motor input
12V to power input
12V to bed power input (used to drive pump and solenoid valves)

Vacuum pump connected to E0 Heater output
Left Nozzle Vacuum Valve connected to E1 Heater output
Right Nozzle Vacuum Valve connected to E2 Heater output

### Controlling Pump + Valves

To track down which pin numbers to use, first find the GPIO pin in the `GTR V1.0 Pin.pdf` documentation.  Then look in the `framework-arduinoststm32\variants\MARLIN_BIGTREE_GTR_V1\variant.cpp` file for the mapping of STM32 pin to Arduino digital pin number `D?`.

Pump On: `M42 P44 S255`
Pump Off: `M42 P44 S0`

Left Nozzle Vacuum On: `M42 P36 S255`
Left Nozzle Vacuum Off: `M42 P36 S0`

Right Nozzle Vacuum On: `M42 P43 S255`
Right Nozzle Vacuum Off: `M42 P43 S0`

### Controlling LEDs

The LEDs come off a bit too pink if the Red component is set to 255, so we're using 200 here.

Ring Light 1 On:
```
M150 R200 U B P I0
...
M150 R200 U B P I10
```

Ring Light 1 Off:
```
M150 P0 I0
...
M150 P0 I10
```

Ring Light 2 On:
```
M150 R200 U B P I11
...
M150 R200 U B P I21
```

Ring Light 1 Off:
```
M150 P0 I11
...
M150 P0 I21
```



### Other Hardware Notes

USART1 (on Raspberry Pi header J48)
  TX: PA9   J48 pin 10
  RX: PA10  J48 pin 8

USART3 (on TFT display header J50)
  TX: PD8   J50 pin 3
  RX: PD9   J50 pin 2

USART6 (on ESP8266 header W1)
  TX: PC6   W1 pin 1
  RX: PC7   W1 pin 8
