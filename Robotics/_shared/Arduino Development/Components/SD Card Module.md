---
tag: Robotics
---
# SD Card Module

Unlike other components, the SD card module uses [[SPI]] to communicate with the Arduino. This means that it's not so simple to wire the component, you will need to use the SPI pins on the Arduino Uno.

![[componentSDCardPinout.jpeg|SD%20Card%20Module%20358798e6d09c425f8b2ad20e846bcec0/ds3231-sd-module-pinout_palMCsePit.jpeg]]

# Wiring

The SPI pins are, according to this site: [https://www.arduino.cc/en/reference/SPI](https://www.arduino.cc/en/reference/SPI)

| Name     | **Arduino Uno Pins** |
| -------- | -------------------- |
| **MOSI** | 11                   |
| **MISO** | 12                   |
| **SCK**  | 13                   |
| **CS**   | 10                   |


![[componentSDCardWiring.png|SD%20Card%20Module%20358798e6d09c425f8b2ad20e846bcec0/SD_Card_Module_bb.png]]

# Code

```arduino
#include <SPI.h>
#include <SD.h>
File myFile;
void setup() {
  // Open serial communications and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }
  Serial.print("Initializing SD card...");
  if (!SD.begin(10)) {
    Serial.println("initialization failed!");
    while (1);
  }
  Serial.println("initialization done.");
  // open the file. note that only one file can be open at a time,
  // so you have to close this one before opening another.
  myFile = SD.open("test.txt", FILE_WRITE);
  // if the file opened okay, write to it:
  if (myFile) {
    Serial.print("Writing to test.txt...");
    myFile.println("This is a test file :)");
    myFile.println("testing 1, 2, 3.");
    for (int i = 0; i < 20; i++) {
      myFile.println(i);
    }
    // close the file:
    myFile.close();
    Serial.println("done.");
  } else {
    // if the file didn't open, print an error:
    Serial.println("error opening test.txt");
  }
}
void loop() {
  // nothing happens after setup
}
```

To use this module in another sketch, you will first need to include the correct libraries, and define the File variable. In the setup function, you'll need to initial the SD card module by including this code

```arduino
while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }
  Serial.print("Initializing SD card...");
  if (!SD.begin(10)) {
    Serial.println("initialization failed!");
    while (1);
  }
  Serial.println("initialization done.");
```

Then initialise the myFile variable. You will need to change the filename to the name you wish.

```arduino
myFile = SD.open("test.txt", FILE_WRITE);
```

Once that's been done, you can write to the file at any point of the code that you wish, by using the `myFile.println()` command.