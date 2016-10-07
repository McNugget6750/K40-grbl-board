_**K40-grbl-board**_
A PCB layout and schematic.

Made with Eagle PCB 7.4.0 Light

This board comes from here: https://github.com/McNugget6750/K40-grbl-board
Author: Timo Birnschein (timo.birnschein at microforge.de)
Please consider giving credit where credit is due.
Please check for updates in the future.

Based on schematics from https://github.com/txjammer (seems offline)

_**DISCLAMER!**_
USE AT YOU OWN RISK!
I CANNOT BE MADE RESPONSIBLE FOR ANY PROBLEMS THAT MIGHT OCCUR WHILE USING THIS.
THIS IS A HOBBY DEVELOPMENT AND NO TESTING HAS BEEN DONE TO ENSURE THE SAVETY OF ANY
OF YOUR EQUIPMENT OR YOUR HOUSE, YOUR LIFE OR ANYTHING ELSE! DO NOT USE UNLESS YOU UNDERSTAND
THIS! WORKING WITH LASERS AND HIGH VOLTAGE POWER SUPPLIES IS DANGEROUS AND SHOULD
ONLY BE DONE BY EXPERIENCED PEOPLE! NEVER USE WITHOUT LINE OF SIGHT TO THE MACHINE!!
YOUR LASER WILL EVENTUALLY BE ON FIRE! IT HAPPENED TO EVERYONE OF US BEFORE!
ALWAYS BE CAUTIOUS! ALWAYS WATCH YOUR MACHINE WHILE OPERATING IT!


_**Requirements**_
* an arduino nano w atmega328p loaded with grbl http://github.com/grbl/grbl
* 2 Pololu A4988 Stepper motor drivers
* a ribbon connector from the original moshi board or this one: http://www.digikey.com/product-search/en?keywords=A100331-ND
* a four pole power connector: MTA04-156 with this package: 1X4MTA
* 4x1 pin header or one of the original moshi stepper motor driver connectors
* grbl form from here: https://github.com/McNugget6750/grbl


_**Software to control the GRBL Laser for cutting**_
I'm using dxf2gcode (with the configurations and source files included in this repository): https://sourceforge.net/projects/dxf2gcode

I also use GRBL-Controller (with the grbl configuration file included in this repository): http://zapmaker.org/projects/grbl-controller-3-0/

My grbl works with relative coordinates. Please keep that in mind.


_**GRBL Config Modifications**_
Modifications for config.h are required!

Modify homing cycle:
#define HOMING_CYCLE_0 (1<<X_AXIS)
#define HOMING_CYCLE_1 (1<<Y_AXIS)

Invert spindle enable pin by enabling:
#define INVERT_SPINDLE_ENABLE_PIN // Default disabled. Uncomment to enable.

Enable variable spindle speed if not done by default. This gives you hardware PWM on Pin D11.
#define VARIABLE_SPINDLE // Default enabled. Comment to disable.

Remap spindle enable pin to pin D13 by enabling (WARNING! WILL BE PULLED LOW BY BOOT LOADER DURING POWER UP! DEACTIVATE YOUR LASER DURING STARTUP OF BOARD IN ORDER TO PREVENT FULL POWER BURNS AT STARTUP.):
#define USE_SPINDLE_DIR_AS_ENABLE_PIN // Default disabled. Uncomment to enable.

In file spindle_control.c I changed the PWM prescaler for the laser to make it more precise and easier to control by changing line 94 from

TCCRB_REGISTER = (TCCRB_REGISTER & 0b11111000) | 0x02; // set to 1/8 Prescaler

to

TCCRB_REGISTER = (TCCRB_REGISTER & 0b11111000) | 0x01; // set to No Prescaling


_**GRBL Configuration that currently sort of works**_
$0=10 (step pulse, usec)

$1=25 (step idle delay, msec)

$2=0 (step port invert mask:00000000)

$3=1 (dir port invert mask:00000001)

$4=0 (step enable invert, bool)

$5=1 (limit pins invert, bool)

$6=0 (probe pin invert, bool)

$10=3 (status report mask:00000011)

$11=0.010 (junction deviation, mm)

$12=0.002 (arc tolerance, mm)

$13=0 (report inches, bool)

$20=0 (soft limits, bool)

$21=0 (hard limits, bool)

$22=1 (homing cycle, bool)

$23=3 (homing dir invert mask:00000011)

$24=200.000 (homing feed, mm/min)

$25=8000.000 (homing seek, mm/min)

$26=250 (homing debounce, msec)

$27=1.000 (homing pull-off, mm)

$100=157.575 (x, step/mm)

$101=157.575 (y, step/mm)

$102=250.000 (z, step/mm)

$110=12000.000 (x max rate, mm/min)

$111=12000.000 (y max rate, mm/min)

$112=12000.000 (z max rate, mm/min)

$120=1000.000 (x accel, mm/sec^2)

$121=1000.000 (y accel, mm/sec^2)

$122=1000.000 (z accel, mm/sec^2)

$130=200.000 (x max travel, mm)

$131=300.000 (y max travel, mm)

$132=10.000 (z max travel, mm)