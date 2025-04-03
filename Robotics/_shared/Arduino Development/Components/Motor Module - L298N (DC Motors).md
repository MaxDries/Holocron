---
tag: Robotics
---
# Motor Module - L298N (DC Motors)

This module allows you to connect 2 DC motors and control them individually.

This module, much like other motor controller options, is best suited with an external power supply. See the wiring diagram for details.

![[componentMotorModuleAnnotated.png]]

[Website](https://www.itead.cc/wiki/L298_Dual_H-Bridge_Motor_Driver)

# Wiring Diagram

![[componentMotorModuleWiring.png]]


> [!important] Power is important! Motors require a lot of amps, usually more than a Raspberry Pi or Arduino can produce on each pin. Additionally, motors can create "noise" on the line which can feedback to the controller and cause damage. 
> 
> For these reasons, never directly connect a motor to a Raspberry Pi or Arduino. Connect them through an external motor controller with its own separate power supply. This is the reason for having the motor module controlling the motors instead of the Raspberry Pi.
> 
> [Read this for more information.](https://learn.adafruit.com/adafruit-dc-and-stepper-motor-hat-for-raspberry-pi/powering-motors)


### DC Motors

A DC motor is a simple device which accepts power and can turn in either direction. Actually, it doesn't matter which pin you attached to positive or negative, the motor will work either way.

Using [PWM](https://www.google.com/url?q=https%3A%2F%2Flearn.sparkfun.com%2Ftutorials%2Fpulse-width-modulation&sa=D&sntz=1&usg=AFQjCNFDruJOqV0l6D7qa3pQRNiEKFt9-A) you can adjust the speed that the motor turns by passing it a value between 0 and 255.

The blue and red cables connect to the Green OUT terminals on the motor module (above).

![[componentsMotorDC.jpg]]

# Code

If you have an Arduino and the L298n module you can look at [this site](https://www.google.com/url?q=https%3A%2F%2Ftronixlabs.com.au%2Fnews%2Ftutorial-l298n-dual-motor-controller-module-2a-and-arduino%2F&sa=D&sntz=1&usg=AFQjCNHSRppUGd7ySGMJQ2YD0L3PQaBEbQ).

But, an easy way to get it done, is to install the l298 library which is searchable through the Arduino software (`Sketch -> Library -> Manage Libraries`) and search for `L298`. Install the version by *Andrea Lombardo*.

![[arduinoInstallLibraryL298N.png]]

The code to test for this L298 library is here:

```arduino
#include <L298N.h>

//pin definition
#define ENA 9
#define IN1 2
#define IN2 3

#define ENB 6
#define IN3 4
#define IN4 5

L298N motorR(ENA, IN1, IN2);
L298N motorL(ENB, IN3, IN4);

void setup() {
  Serial.begin(9600);

  motorR.setSpeed(255); // an integer between 0 and 255
  motorL.setSpeed(255); // an integer between 0 and 255
}

void loop() {
  // put your main code here, to run repeatedly:
  motorR.forward();
  motorL.forward();
  delay(3000);
  motorR.stop();
  motorL.stop();
  delay(3000);

  for (int i = 100; i < 255; i++) {
    motorR.setSpeed(i); // an integer between 0 and 255
    motorL.setSpeed(i); // an integer between 0 and 255
    motorR.forward();
    motorL.forward();
    Serial.print("speed: ");
    Serial.println(i);
    delay(100);
    motorR.stop();
    motorL.stop();
  }
}
```

To modify the code to make it useful, you'll need to initially make sure you include the library (`#include <L298N.h>`) at the top of your code.

After that, when you want the motor to run, you'll need to set the speed of the motor in question (e.g. `motorR.setSpeed(255);`) and then set it to forward or backward (e.g.`motorR.forward();`). 

And stop when necessary -e.g.`motorL.stop();`.

# Demonstration

Here is a live demonstration of the wiring, power, coding and testing of the L298N module.

![https://youtu.be/W3Yya6EdYIk](https://youtu.be/W3Yya6EdYIk)