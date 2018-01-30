# Final Project: Tachometer
## Overview
Our task for this project was to create a tachometer using a DC motor, a paper disk and an optointerrupter:
![](Images/Tachometer.png)

The task required exhaustive use of concepts such as Pulse Width Modultation, interrupts, timers and software debouncing. In addition, the use of a motor in the circuitry introduced a point of failure which was mitigated by separating the motor circuit and microcontroller using an optocoupler.

## Components
This project was built upon the [AVR Butterfly Board](http://www.microchip.com/webdoc/avrbutterfly/avrbutterfly.wb_AVRBFLY_Introduction.Introduction.html).

To build the tachometer itself, a wooden block secured a [DC motor](https://www.adafruit.com/product/711), a paper disk with a slit in it was attached to the motor, and an [optointerrupter](https://www.mouser.com/ProductDetail/Vishay/TCST2103/?qs=%2Fjqivxn91cc%252bOKE9BUKCsA%3D%3D&gclid=Cj0KCQiAzMDTBRDDARIsABX4AWyTWKAfty3jF5NNuhgEldwwhqwAbbydyYwv_5q-ltlf4lAUfnBXak8aAvTJEALw_wcB) was used to count the number of rotations per minute.

The circuitry most notably used [4N28 Optocoupler](https://www.vishay.com/docs/83725/4n25.pdf) to separate the motor circuit from the microcontroller circuit, paired with another transistor to handle the high current of the motor.

## Features
The features of this project are as follows:
* Displays an updated RPM count every second on the LCD
* Allows a variable duty cycle from 0-100% to be controlled via up/down on joystick
* Allows motor to be turned on/off via center press of joystick
* Temporarily displays duty cycle or power status on LCD when joystick used

## Code
All of the code for this project is detailed with comments in `main.c`.

The bulk of the logic for the code is handled in interrupt service routines toward the bottom of the code. Four such routines—including one for each of the ATmega169p's available timers—are detailed as follows:
* **PCINT1**: listens to pin changes on the joystick inputs and adjusts duty cycle, with debouncing provided in part by *TIMER1_COMPA*
* **TIMER0_COMP**: generates PWM pulse by outputting high for 25us to 250us and low for the rest
* **TIMER1_COMPA**: calculates RPM every second and debounces joystick input
* **TIMER2_COMP**: checks sensor input every 10us and debounces it to increment count

The remaining code—in the main function—simply continuously displays the RPM or status messages on the LCD.

