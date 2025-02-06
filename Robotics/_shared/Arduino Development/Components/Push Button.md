---
tag: Robotics
---
# Push Button

The crash sensor used acts as a simple button, but with a wire protruding from the front. This type of sensor can be used in Robots to detect if the robot has collided with another object.

There are different types of crash sensors, so you must be careful when you wire them to the arduino. The one used in this tutorial is the one shown below.

![[componentButtonCrash.png]]

# Wiring

In the wiring diagram below, the button is wired to a digital I/O pin to the OUT pin on the module. 

![[componentButtonCrashWiring.png]]

> [!important] The crash sensor, or standard push button, is a basic Input device, and therefore can be connected to any available Digital I/O pin. The pin connected in the diagram above, may not be the correct pin for your project - check your requirements and wire appropriately. If the pin is different, you will need to change the code below.


# Code

```arduino
/*
  Button

  Turns on and off a light emitting diode(LED) connected to digital pin 13,
  when pressing a pushbutton attached to pin 2.

  The circuit:
  - LED attached from pin 13 to ground
  - pushbutton attached to pin 2 from +5V
  - 10K resistor attached to pin 2 from ground

  - Note: on most Arduinos there is already an LED on the board
    attached to pin 13.

  created 2005
  by DojoDave <http://www.0j0.org>
  modified 30 Aug 2011
  by Tom Igoe

  This example code is in the public domain.

  http://www.arduino.cc/en/Tutorial/Button
*/

// constants won't change. They're used here to set pin numbers:
const int buttonPin = 2;     // the number of the pushbutton pin
const int ledPin =  13;      // the number of the LED pin

// variables will change:
int buttonState = 0;         // variable for reading the pushbutton status

void setup() {
  // initialize the LED pin as an output:
  pinMode(ledPin, OUTPUT);
  // initialize the pushbutton pin as an input:
  pinMode(buttonPin, INPUT);
}

void loop() {
  // read the state of the pushbutton value:
  buttonState = digitalRead(buttonPin);

  // check if the pushbutton is pressed. If it is, the buttonState is HIGH:
  if (buttonState == LOW) {
    // turn LED on:
    digitalWrite(ledPin, HIGH);
  } else {
    // turn LED off:
    digitalWrite(ledPin, LOW);
  }
}
```

![[componentButtonCrashDemo.gif]]