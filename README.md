# OtterControl
Hardware files for OtterControl BLDC motor controller.

![3D render of the board](https://raw.githubusercontent.com/cyber-murmel/ottercontrol/master/images/kicad_render.png)

## What is OtterControl?
OtterControl is a open source, open hardware DC / BLDC motor controller.
The basic idea was to make the well-known VESC (http://vedder.se/2015/01/vesc-open-source-esc/) even cheaper - making hoverboard-powered armchairs or crazycarts affordable.

While these 350W hoverboards motors are great and quiet affordable (only 40€ in Germany),
you need a powerful ESC to get them running. Until now, the only open ESC I'm aware of is the VESC - which is great and all, but with ~100-120€ per piece quiet expensive.
That's why I decided to save on some parts:

* Single-Chip Gatedrivers
* Mosfets in D2PAK package
* Chinese HV buck converter
* STM32f3 instead of f4: still powerful enough, and with more internal analog peripherals

The STM32f303 has lots of internal peripherals such as OpAmps, PGAs etc.
Using this chip reduces external components and system complexity - saving about 10€.
Also, using D2PAK Mosfets allows more flexible part selection - saving another 20€ with almost the same specs.

The final BOM for 50pcs of OtterControl turns out to be ~19€ per piece.

## Specs
* max. 50A continous motor current (~90A peak)
* max. 80V (with 63V capacitors = 14S LiIon)
* STMBL Codebase ("easy to use", configurable)
* no additional programming device (USB DFU support)

* UART, RS485, CAN, USB
* PPM (to be implemented)

* 3 GPIOs, sharing one Timer (eg. as hall sensor input, quadrature encoder)
* 5V max. 200mA out (eg. for hall sensors)
* Otter

## Versions

There are currently two different versions of OtterControl V1.3:
One with 1mOhm current-sensing shunts, and one with 3mOhm.
The reason for this:

To measure the phase current, the f303 internal PGA is set to 16x gain.
This gives a input range of 3.2AVDD / 16 = 200mV.
It is necessary to measure positiv and negative currents, so the shunt is pre-biased to 100mV, giving +/- 100mV measurable voltage drop.

* With 1mOhm, this means 100A full scale
* With 3mOhm, this means 33A full scale

Depending on your application, you want as much current resolution as possible.
For a 5A DC motor, or a small BLDC, 33A is way enough, archiving good current regulation.
However, for applications with more current, 1mOhm increases the current scale to the necessary range.

## Software

OtterControl is based on the stmbl codebase (https://github.com/rene-dev/stmbl).
A ported version can be found in my fork [ottercontrol branch](https://github.com/NiklasFauth/stmbl/tree/ottercontrol-sessel).
Essentially, the code on the f303 is the same as in stmbl, but without the f4 this mcu now also handles encoder input, position regulation, user settings etc.

## Documentation

### Schematic

Download the PDF by clicking the image
[![](https://raw.githubusercontent.com/cyber-murmel/ottercontrol/master/images/schematic.png)](https://github.com/cyber-murmel/ottercontrol/raw/master/ottercontrol.pdf)
