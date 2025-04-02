## Explanation Video

![Made for Semester 1, 2023](https://www.youtube.com/watch?v=vquMYicwrso)


In this stage you will be “converting” your behaviours and flowcharts into code. As you’ve already done the planning for this, it is best to keep your flowcharts on hand to refer to.

Start by creating a new Arduino sketch in the IDE, naming it after you project - such as SmartHouse. Save this sketch in your Github directory.

![[codeNewArduinoSketch.png|Screen Shot 2022-05-20 at 2.55.05 pm.png]]

![[commonBlocks#Commit & Push]]

## Coding

To code this project, you’ll be approaching each section at a time, starting with Includes and Variable Configuration, shown in this image.

Afterwards, then it’s the setup() function, then loop() then finally the Custom Functions.

![[Structure_of_an_Arduino_Sketch_example_with_code.png]]

> [!info] The pin numbers listed in this section must be individually confirmed to ensure they match your wiring diagram and other requirements.


If you need to install an external library, please see this guide.

[Installing an Arduino Library](https://learn.sparkfun.com/tutorials/installing-an-arduino-library/all)

## Common Code for All Projects

### SD and Real Time Clock


You will need to install the `RTCLib` (by Adafruit) library. Follow the instructions above.

![[codeArduinoLibrary.png|Screen Shot 2022-05-20 at 3.59.18 pm.png]]

Once the library has been installed, add the code shown here before the setup() function.

This code will enable reading and writing to the SD card through the module. The RTC library is needed to store the current date and time.

```arduino
// SD Card Module
#include <SPI.h>
#include <SD.h>

// Real Time Clock (RTC)
#include "RTClib.h"
RTC_Millis rtc;     // Software Real Time Clock (RTC)
DateTime rightNow;  // used to store the current time.
```


It’s time to configure the modules that you will be using. Depending on the module, there may be more complex code to create “objects” that can be accessed.

## Common `Setup()` code

Copy the following code and replace the contents of the default `setup()` function code.

```arduino
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
// Real Time Clock (RTC)
rtc.begin(DateTime(F(__DATE__), F(__TIME__)));
Serial.println("initialization done.");
logEvent("System Initialisation...");
```

### LogEvent() Function - don’t do this (2023).

The following function writes events to the SD card attached to your Arduino. Copy and Paste into your sketch at the bottom (after `loop()`)

**Code**

```arduino
void logEvent(String dataToLog) {
  /*
	 Log entries to a file on an SD card.
  */
  // Get the updated/current time
  DateTime rightNow = rtc.now();

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


### !!Quick Check!!

At this stage, your code should appear **similar** to this.

![[codeSDCardRTCCode.png|Screen Shot 2022-05-20 at 5.05.31 pm.png]]

## Module Specific Code

By choosing the modules you require, add the following code after the import statements. Change the pin numbers as necessary.

These code snippets go **above** the setup() function in your sketch.

After adding each code snippet, compile the code to ensure it is working correctly.

> [!info] The following code snippets may include some or all of the following:
> - Pin Definitions
> - “Object” configuration
> - Low Level Function code.


## “Includes and Variable” Configuration

The following code snippets are to be included at the top of the code, above any functions.

### Bluetooth

```arduino
// Adafruit nRF8001 Module
#include <SPI.h>
#include "Adafruit_BLE_UART.h"

// Connect CLK/MISO/MOSI to hardware SPI
// e.g. On UNO & compatible: CLK = 13, MISO = 12, MOSI = 11
#define ADAFRUITBLE_REQ 40
#define ADAFRUITBLE_RDY 2     // This should be an interrupt pin, on Uno thats #2 or #3
#define ADAFRUITBLE_RST 41
Adafruit_BLE_UART BTLEserial = Adafruit_BLE_UART(ADAFRUITBLE_REQ, ADAFRUITBLE_RDY, ADAFRUITBLE_RST);
aci_evt_opcode_t laststatus = ACI_EVT_DISCONNECTED;

void bluetoothCommandReceived(String bleCommand) {
  bleCommand.trim();
  int bleCommandInt = bleCommand.toInt();

  // TODO: Add responses to commands here.
  // These two can be used for testing purposes.
  switch (bleCommandInt) {
	case 1:
	  digitalWrite(13, HIGH);
	  logEvent("BLE - LED Off");
	  break;
	case 0:
	  digitalWrite(13, LOW);
	  logEvent("BLE - LED On");
	  break;
  }
}
void bluetoothConnectivity() {
  BTLEserial.pollACI();

  // Ask what is our current status
  aci_evt_opcode_t status = BTLEserial.getState();
  // If the status changed....
  if (status != laststatus) {
	// print it out!
	if (status == ACI_EVT_DEVICE_STARTED) {
	  Serial.println(F("* Advertising started"));
	}
	if (status == ACI_EVT_CONNECTED) {
	  Serial.println(F("* Connected!"));
	}
	if (status == ACI_EVT_DISCONNECTED) {
	  Serial.println(F("* Disconnected or advertising timed out"));
	}
	// OK set the last status change to this one
	laststatus = status;
  }

  if (status == ACI_EVT_CONNECTED) {
	// Lets see if there's any data for us!
	if (BTLEserial.available()) {
	  Serial.print("* "); Serial.print(BTLEserial.available()); Serial.println(F(" bytes available from BTLE"));
	}
	String command = "";
	// OK while we still have something to read, get a character and print it out
	while (BTLEserial.available()) {
	  char c = BTLEserial.read();
	  //      Serial.print(c);
	  command += c;
	}

	if (command != "") {
	  bluetoothCommandReceived(command);
	}

	// Next up, see if we have any data to get from the Serial console

	if (Serial.available()) {
	  // Read a line from Serial
	  Serial.setTimeout(100); // 100 millisecond timeout
	  String s = Serial.readString();

	  // We need to convert the line to bytes, no more than 20 at this time
	  uint8_t sendbuffer[20];
	  s.getBytes(sendbuffer, 20);
	  char sendbuffersize = min(20, s.length());

	  Serial.print(F("\n* Sending -> \"")); Serial.print((char *)sendbuffer); Serial.println("\"");

	  // write the data
	  BTLEserial.write(sendbuffer, sendbuffersize);
	}
  }
}
```

> [!info]  You may find you need to install a library called `Adafruit nRF8001`. Follow the instructions for using the Library Manager.


It’s important to check to make sure it works - follow instructions on this page: 

[Getting Started with the nRF8001 Bluefruit LE Breakout](https://learn.adafruit.com/getting-started-with-the-nrf8001-bluefruit-le-breakout/testing-uart)

### GPS

> [!info] The GPS unit works MUCH better when run outside. If run inside, put it near a window, and it may take a minute or two to find a GPS location. Outside, it can work instantaneously.


```arduino
// GPS
/* RXPin & TXPin
 * If a Uno, then 4 and 3
 * If a Mega/Leonardo, then 10 and 11.
 * Check the software Serial documentation for change interrupts
 * https://www.arduino.cc/en/Reference/SoftwareSerial
 */
static const int RXPin = 10, TXPin = 11;
static const uint32_t GPSBaud = 9600;

#include <SoftwareSerial.h>
#include <TinyGPS++.h>

TinyGPSPlus gps; // The TinyGPS++ object
SoftwareSerial ss(RXPin, TXPin);// The serial connection to the GPS device
```

> [!info] You may find you need to install a library called `TinyGPSPlus`. Follow the instructions for using the Library Manager.


### Infrared Remote

```arduino
// IR Remote
#include <IRremote.h>
#define IR_INPUT_PIN    2
IRrecv irrecv(IR_INPUT_PIN);
decode_results results;
```

> [!info]  You may find you need to install a library called `IRRemote`. Choose the one developed by **Armin Joachimsmeyer** Follow the instructions linked above for  installing a library.


### Traffic Light Module

```arduino
// Traffic Lights - LED Outputs
#define ledRed A0
#define ledYellow A1 
#define ledGreen A2
```

### DC Motor (Blue Button version)

> [!info]  This code works for the Motor Controller with the blue press button (on/off) on the board.

```arduino
// DC Motor & Motor Module - L298N
#include <L298N.h>

// Pin definition
const unsigned int IN1 = 7;
const unsigned int IN2 = 8;
const unsigned int EN = 9;

// Create one motor instance
L298N motor(EN, IN1, IN2);
```


> [!info] You may find you need to install a library called `L298N`. Follow the instructions for using the Library Manager.


### DC Motor (DFRobot)

> [!info]  This code works for the Motor module labelled DFRobot


```arduino
int E1 = 6;
int M1 = 7;
```

### Servo

```arduino
// Servo
#include <Servo.h>
Servo myservo;
```
### Moisture Sensor

```arduino
// Moisture Sensor
#define moisturePin A5
```

### Potentiometer

```arduino
//Potentiometer
#define pot A3
```

### Piezo

```arduino
// Piezo Buzzer
#define piezoPin 8
```

###  Sonar

```arduino
// Sonar - HC-SR04
#define echoPin 6 // attach pin D2 Arduino to pin Echo of HC-SR04
#define trigPin A4 //attach pin D3 Arduino to pin Trig of HC-SR04
```

###  Line Sensor

```arduino
// Line Sensor
#define lineSensorPin 3
```

### Crash Sensor (button)

```arduino
// Crash Sensor / Button
#define crashSensor 7
```
### RFID

> [!info]  The **MFRC522** library needs to be installed through the library manager.


```arduino
// RFID Start

#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN  21  // ES32 Feather
#define RST_PIN 17 // esp32 Feather - SCL pin. Could be others.

MFRC522 rfid(SS_PIN, RST_PIN);
```


## Module Specific Setup() Code

The following code snippets should be included in the setup() function.

### Bluetooth

```arduino
// Bluetooth - nRF8001
uart.setRXcallback(rxCallback);
uart.setACIcallback(aciCallback);
uart.setDeviceName("BLEName"); /* 7 characters max! */
uart.begin();
```

###  GPS

```arduino
// GPS
  ss.begin(GPSBaud);
```

###  Infrared Remote

```arduino
// IR Remote
irrecv.enableIRIn();
```

###  Traffic Light Module

```arduino
// Traffic Lights - LED Outputs
pinMode(ledRed, OUTPUT);
pinMode(ledYellow, OUTPUT);
pinMode(ledGreen, OUTPUT);
```

###  DC Motor (Blue Button version)

```arduino
// DC Motor & Motor Module - L298N
motor.setSpeed(70);
```

###  DC Motor (DFRobot)


‼️ This code works for the Motor module labelled DFRobot


```arduino
pinMode(M1, OUTPUT);
```

###  Servo

```arduino
// Servo
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
```

###  Moisture Sensor

```arduino
// Moisture Sensor
pinMode(moisturePin, INPUT);
```

###  Potentiometer

```arduino
//Potentiometer
pinMode(pot, INPUT);
```

###  Piezo

```arduino
// Piezo Buzzer
pinMode(piezoPin,OUTPUT);
```

###  Sonar

```arduino
// Sonar - HC-SR04
pinMode(trigPin, OUTPUT); // Sets the trigPin as an OUTPUT
pinMode(echoPin, INPUT); // Sets the echoPin as an INPUT
```

###  Line Sensor

```arduino
// Line Sensor
pinMode(lineSensorPin, OUTPUT);
```

###  Crash Sensor (button)

```arduino
// Crash Sensor / Button
pinMode(crashSensor, INPUT);
```

###  RFID

```arduino
// RFID Start
  SPI.begin(); // init SPI bus
  rfid.PCD_Init(); // init MFRC522
  // RFID End
```

