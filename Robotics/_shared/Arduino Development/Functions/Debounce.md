---
tag: Robotics
---
# Debounce

# Push button issues

Here is a video I made to explain the debounce code for Arduino.

The debounce sample code can be found in the Arduino IDE, under Files, then Examples, then 0.2 Digital.

[https://www.youtube.com/watch?v=ZRXf_v6cBHU](https://www.youtube.com/watch?v=ZRXf_v6cBHU)

# Debounce and Servo Sweep

[https://www.youtube.com/watch?v=wiU2KJbHGZ8](https://www.youtube.com/watch?v=wiU2KJbHGZ8)

## Final Code

```arduino
// LED & BUTTON INITILISATION

// constants won't change. They're used here to set pin numbers:
const int buttonPin = 2;    // the number of the pushbutton pin
const int ledPin = 13;      // the number of the LED pin

// Variables will change:
int ledState = HIGH;         // the current state of the output pin
int buttonState;             // the current reading from the input pin
int lastButtonState = LOW;   // the previous reading from the input pin

// the following variables are unsigned longs because the time, measured in
// milliseconds, will quickly become a bigger number than can be stored in an int.
unsigned long lastDebounceTime = 0;  // the last time the output pin was toggled
unsigned long debounceDelay = 50;    // the debounce time; increase if the output flickers

// SERVO INITIALISATION
#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards
int pos = 0;    // variable to store the servo position

// RANDOM NUMBER
long randNumber;
int servoAngle; // Angle to set the servo to.

void setup() {
  Serial.begin(9600);
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);

  // set initial LED state
  digitalWrite(ledPin, ledState);
  myservo.attach(8);  // attaches the servo on pin 9 to the servo object
}

void loop() {
  // read the state of the switch into a local variable:
  int reading = digitalRead(buttonPin);

  // check to see if you just pressed the button
  // (i.e. the input went from LOW to HIGH), and you've waited long enough
  // since the last press to ignore any noise:

  // If the switch changed, due to noise or pressing:
  if (reading != lastButtonState) {
    // reset the debouncing timer
    lastDebounceTime = millis();
  }

  if ((millis() - lastDebounceTime) > debounceDelay) {
    // whatever the reading is at, it's been there for longer than the debounce
    // delay, so take it as the actual current state:
    // if the button state has changed:
    if (reading != buttonState) {
      buttonState = reading;
      // only toggle the LED if the new button state is HIGH
      if (buttonState == HIGH) {
        ledState = !ledState;
      }

      randNumber = random(1, 5); // random number between 1 and 4
      if (randNumber == 1) {
        servoAngle = 0;
      } else if (randNumber == 2) {
        servoAngle = 60;
      } else if (randNumber == 3) {
        servoAngle = 120;
      } else {
        servoAngle = 180;
      }

      Serial.println(servoAngle);
      myservo.write(servoAngle);
    }
  }

  // set the LED:
  digitalWrite(ledPin, ledState);

  // save the reading. Next time through the loop, it'll be the lastButtonState:
  lastButtonState = reading;
}
```