---
tag: Robotics
---
# logEvent()

The follow code can be used to write event data to a [[SD Card Module]] connected to an Arduino Uno. 

# Real Time "Clock"

Typically, in the situation below, you would use a physical Real Time Clock (RTC) which is a module that keeps the current time, and is powered by a battery so that it doesn't matter if the Arduino is powered or not. In this case, to save on components and save on pins, a workaround has been implemented - a software real time clock.

The code below uses the time of the computer **when the code is compiled** and uses that as the current time.

> [!info] If the Arduino loses power or is restarted, you will no longer have an accurate time.


# Code

This code can be used as the starting for many software solutions which require logging of data to SD cards.

> [!info]  In order for this to work, you may need to install the RTClib (made by Adafruit) library. Instructions can be found [here](https://learn.adafruit.com/adafruit-all-about-arduino-libraries-install-use/library-manager)


```arduino
#include <SPI.h>
#include <SD.h>
#include "RTClib.h"

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

  // Get the time of the Computer at compile time and store it on Arduino.
  rtc.begin(DateTime(F(__DATE__), F(__TIME__)));
  logEvent("System Initialisation...");
}

void loop() {
  // update with Loop functionality.
  logEvent("Loop");
  delay(1000);
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

When this code is run on the Arduino (with an SD card attached and working) the Serial Monitor will display an entry `"Event Logged: System Initialisation..."` and the Events.csv file will show an entry similar to this:

![[logEvent.png]]

The information in the log is the `Year, Month, Day, Hour, Minute, Second, Data`, as can be seen through analysing the code in the `logEntry()` function.