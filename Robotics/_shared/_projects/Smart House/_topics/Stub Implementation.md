## Explanation Video

![Made for Semester 1, 2023](https://www.youtube.com/watch?v=QRXc9KyWyz8)


## Instructions

Using your flowcharts created in the Programming Logic stage, complete the implementation for each function stub of your project.

At the end of the process, you should have a partially or fully working implementation of your project.

### Example Code

```arduino
#include <SPI.h>
#include <SD.h>
#include "RTClib.h"
#include <L298N.h>                // Motor Controller

#include "pitches.h" // Buzzer tones

// notes in the melody:
int melody[] = {
  NOTE_C4, NOTE_G3, NOTE_G3, NOTE_A3, NOTE_G3, 0, NOTE_B3, NOTE_C4
};

// note durations: 4 = quarter note, 8 = eighth note, etc.:
int noteDurations[] = {
  4, 8, 8, 4, 4, 4, 4, 4
};

// Output
int redPin   = A0;   // Red LED,   connected to digital pin 6
int greenPin = A2;  // Green LED, connected to digital pin 10
int yellowPin  = A1;  // Blue LED,  connected to digital pin 11
int piezoPin = 6;

// Input pins
int crashSensorPin = 8;   // Crash Sensor (button). HIGH or LOW values.
int lineSensorPin = 2;    // Line Sensor (light). HIGH or LOW values.
int echoPin = A3;
int trigPin = A4;

#define IR_INPUT_PIN    7
#include "TinyIRReceiver.cpp.h"
#define STR_HELPER(x) #x
#define STR(x) STR_HELPER(x)

// Control booleans
boolean isLogged = false;
boolean engineOn = false;
boolean motorOn = false;
boolean keyState = false;
boolean lastKeyState = false;

// Motor Pin definition
const unsigned int IN1 = 3;
const unsigned int IN2 = 4;

// Create one motor instance
L298N engine(IN1, IN2);
// Create a IR Receiver instancce
//Adafruit_NECremote remote(IRpin);

RTC_Millis rtc;     // Software Real Time Clock (RTC)
DateTime rightNow;  // used to store the current time.

void setup() {
  Serial.begin(9600);           // Open serial communications and wait for port to open:
  while (!Serial) {
	delay(1);                   // wait for serial port to connect. Needed for native USB port only
  }

  // SD Card initialisation
  Serial.print("Initializing SD card...");
  if (!SD.begin(10)) {
	Serial.println("initialization failed!");
	while (1);
  }
  Serial.println("initialization done.");

  // LED Outputs
  pinMode(redPin,   OUTPUT);   // sets the pins as output
  pinMode(greenPin, OUTPUT);
  pinMode(yellowPin,  OUTPUT);

  //Sonar Pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  // Get the time of the Computer at compile time and store it on Arduino.
  rtc.begin(DateTime(F(__DATE__), F(__TIME__)));
  logEvent("System Initialisation...");
  logEvent("Infrared Subsystem enabled");
  logEvent("Motor Subsystem enabled");
  pinMode(lineSensorPin, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(lineSensorPin), engineOnOff, FALLING);

  // IR Configuration
  initPCIInterruptForTinyReceiver();
  Serial.println(F("Ready to receive NEC IR signals at pin " STR(IR_INPUT_PIN)));
}

void loop() {
  carMotorAndBrake();
  detectAuthorisedAccessInRadius(5);
  delay(100);
}

void detectAuthorisedAccessInRadius(int distanceThreshold) {
  /*
	 Uses the sonar to detect distance to object. When object is within a given distance, it triggers the alarm (buzzer).
	 @param distanceThreshold: Integer to determine at what distance to trigger alarm.
  */
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) / 29.1;
  //  Serial.print(distance);
  //  Serial.println(" cm");
  if (distance < 10) {
	logEvent("Alarm");

	for (int thisNote = 0; thisNote < 8; thisNote++) {

	  // This sends a PWM signal to the pieze, and uses the melody array to pick the note to play.
	  // This could be done another way by ....  

	  // to calculate the note duration, take one second divided by the note type.
	  //e.g. quarter note = 1000 / 4, eighth note = 1000/8, etc.
	  int noteDuration = 1000 / noteDurations[thisNote];
	  tone(piezoPin, melody[thisNote], noteDuration);

	  // to distinguish the notes, set a minimum time between them.
	  // the note's duration + 30% seems to work well:
	  int pauseBetweenNotes = noteDuration * 1.30;
	  delay(pauseBetweenNotes);
	  // stop the tone playing:
	  noTone(piezoPin);
	}
  }
  delay(50);

}

void handleReceivedTinyIRData(uint16_t aAddress, uint8_t aCommand, bool isRepeat) {
  logEvent("Infrared Code received: " + aCommand);
  if (aCommand == 70) {
	logEvent("IR Command - Up Pressed - Light Off");
	digitalWrite(redPin, HIGH);

  }
  if (aCommand == 21) {
	logEvent("IR Command - Down Pressed - Light Off");
	digitalWrite(redPin, LOW);
  }
  if (aCommand == 68) {
	Serial.println("Left");
  }
  if (aCommand == 67) {
	Serial.println("Right");
  }
}

void engineOnOff() {
  engineOn = !engineOn;
  digitalWrite(13, engineOn);
}

void carMotorAndBrake() {
  if (engineOn) {
	boolean brakeState = digitalRead(crashSensorPin);
	digitalWrite(greenPin, HIGH);
	if (!brakeState) {   // if the crash sensor is pushed in
	  digitalWrite(yellowPin, HIGH);
	  motorOn = false;
	} else {
	  digitalWrite(yellowPin, LOW);
	  motorOn = true;
	}
  } else {
	digitalWrite(greenPin, LOW);
  }

  if (engineOn && motorOn) {
	engine.forward();
	engine.setSpeed(70);
	if (!isLogged) {
	  logEvent("Motor On");
	  isLogged = true;
	}

  } else {
	engine.stop();
	isLogged = false;
  }

  //debugMotor();

}

void debugMotor() {
  Serial.print("Engine State :");
  if (engineOn) {
	Serial.print("On");
  } else {
	Serial.print("Off");
  }

  Serial.print("   Key State :");
  if (keyState) {
	Serial.print("On");
  } else {
	Serial.print("Off");
  }

  Serial.print("   Motor State :");
  if (motorOn) {
	Serial.print("On");
  } else {
	Serial.print("Off");
  }
  Serial.println();
}

void motorOnOff() {
  boolean buttonState = digitalRead(crashSensorPin);
  if (buttonState == LOW) {
	logEvent("Motor On");
	engine.forward();
	engine.setSpeed(70);

	delay(1000);
	engine.stop();
	logEvent("Motor Off");
  }

}

void logEvent(String dataToLog) {
  /*
	 Log entries to a file on an SD card.
  */
  // Get the updated/current time
  rightNow = rtc.now();

  // Open the log file
  File logFile = SD.open("events.csv", FILE_WRITE);
  if (!logFile) {
	Serial.print("Couldn't create log file");
	abort();
  }

  // Log the event with the date, time and data
  logFile.print(rightNow.year(), DEC);
  logFile.print(",");
  logFile.print(rightNow.month(), DEC);
  logFile.print(",");
  logFile.print(rightNow.day(), DEC);
  logFile.print(",");
  logFile.print(rightNow.hour(), DEC);
  logFile.print(",");
  logFile.print(rightNow.minute(), DEC);
  logFile.print(",");
  logFile.print(rightNow.second(), DEC);
  logFile.print(",");
  logFile.print(dataToLog);

  // End the line with a return character.
  logFile.println();
  logFile.close();
  Serial.print("Event Logged: ");
  Serial.print(rightNow.year(), DEC);
  Serial.print(",");
  Serial.print(rightNow.month(), DEC);
  Serial.print(",");
  Serial.print(rightNow.day(), DEC);
  Serial.print(",");
  Serial.print(rightNow.hour(), DEC);
  Serial.print(",");
  Serial.print(rightNow.minute(), DEC);
  Serial.print(",");
  Serial.print(rightNow.second(), DEC);
  Serial.print(",");
  Serial.println(dataToLog);
}
```

