## Overview

The goal of a wiring diagram is to demonstrate how the components are connected to each other. They are to be used as a **guide** to show which pins on the devices to connect to each other.

> [!info]  The physical components may differ from the ones shown in a wiring digram. Always check which pins are the VIN and GND etc on your hardware.


## Instructions

### Setup

1. First, access the `WiringStart.ckt` in the project.
2. Finally, open Cirkit Designer, go to File → Open and choose the `.ckt` file.
    1. You may find that you need to open Cirkit Designer first, then Select **Open File**.

![[wiringCirkitDesigner.png|Screen Shot 2022-04-18 at 11.20.46 pm.png]]

## Cirkit Designer How To

[https://youtu.be/LhXv9ngXCCU](https://youtu.be/LhXv9ngXCCU)

## Steps to complete wiring diagram

### Power

1. If not done already, wire the GND pins from the Arduino to the Ground rail (indicated in green in Cirkit Designer). 
2. If not done already, wire the 5V pin from the Arduino to the positive rails on the breadboard (indicated in red)
    
    ![[wiring1.png]]
    
3. Wire all the Ground pins of all devices that you’ll be using to one of the ground rails.
4. Next, wire the relevant power pins from the Battery pack to the 12V and Ground pins on the motor controller.
    
    ![[wiring2.png]]
    
5. Finally identify the power pins for each of the devices and connect each of them to the positive power rail. Power pins could be labeled differently. Some common labels are 5V, 3V3, VCC, etc.
6. Finally, press the Autoroute all wires button to clean up the organisation of the wires.

![[wiringAutoRoute.png]]

After this process, you should have a diagram appearing similar to the one shown here.

![[wiring3.png]]

### Device Pins

Devices have different number and purpose of pins, making simple instructions difficult, however not impossible.

Some of the devices are listed below, showing the pins identified on the device and which Arduino pin it should be connected to.

SD Card

![[modularisationMicroSDCardReader.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| MISO | 12 |
| MOSI | 11 |
| SCK | 13 |
| CS | 10 |

Servo

![[modularisationServo.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Pulse | 9 |

Motor Module

![[modularisationL298N.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| IN3 | 6 |
| IN4 | 5 |

Piezo / Buzzer

![[modularisationPiezo.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Pin2 | 8 |

Button Sensor

![[modularisationButton.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Signal | 7 |

IR Receiver

![[modularisationIRReceiver.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Data | 4 |

Line Tracker Sensor

![[modularisationLineTracker.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Out | 3 |

Sonar / HCSR04

![[modularisationSonar.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Trig | 2 |
| Echo | A4 |

Traffic Lights

![[modularisationTrafficLight.png]]

| Device Pin | Arduino Pin |
| --- | --- |
| Red | A0 |
| Yellow | A1 |
| Green | A2 |

Potentiometer

![[modularisationPot.png]]

| Device Pin | Arduino Pin |
| ---------- | ----------- |
| Leg1       | Ground      |
| Wiper      | A3          |
| Leg2       | 5V          |

> [!info] Don’t forget to Autoroute all the wires!


At the end of the wiring process, you should have a digram that appears similar to this.

![[wiringFinal.png]]
