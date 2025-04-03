---
tags:
  - Robotics
  - Robotics/esp32
---

# Featherwings

The goal of this section is to test all the different modules to ensure they’re working correctly with the sample code.

## Featherwing Tutorial Pages (with sample code)

Mini TFT Featherwing

[Adafruit Mini TFT with Joystick Featherwing](https://learn.adafruit.com/adafruit-mini-tft-featherwing/arduino-code)

Temperature Featherwing

[IoT Motion and Temperature Logger with the Analog Devices ADXL343 + ADT7410 Sensor FeatherWing and Adafruit IO](https://learn.adafruit.com/iot-motion-and-temperature-sensor-with-adxl343-adt7410-sensor-featherwing-and-adafruit-io/arduino-code)

Adalogger Featherwing

[Adafruit Adalogger FeatherWing](https://learn.adafruit.com/adafruit-adalogger-featherwing)

DC Motor Featherwing

[Adafruit Stepper + DC Motor FeatherWing](https://learn.adafruit.com/adafruit-stepper-dc-motor-featherwing)

## Test Components

The first thing to do is test the different featherwings to ensure they’re wired and working correctly.

To do this, use the links provided above.

1. Install any libraries (if necessary)
2. Run specific Sample Code

> [!info] **Note**: The sample code for the mini TFT featherwing appears to be incorrect. Look for the code for the ESP32 and change the pins to be shown like this:
> ```arduino
#define TFT_CS   14
#define TFT_DC   32
> ```
> ![[featherwingsMiniTFTIncorrectPins.png|Screen Shot 2022-08-27 at 2.11.17 pm.png]]


## Combine Code

Combine different sample code blocks, focusing on **only** the necessary code.

For instance, getting the Temperature from the Temperature Featherwing and displaying the output on the LCD screen on the Mini TFT, the code will be:

```arduino
#include "sensitiveInformation.h"

#define FORMAT_SPIFFS_IF_FAILED true

#include <Wire.h>

// Wifi & Webserver
#include "WiFi.h"
#include "SPIFFS.h"
#include <AsyncTCP.h>
#include <ESPAsyncWebServer.h>

AsyncWebServer server(80);

// RTC Start - Remove if unnecessary
#include "RTClib.h"

RTC_PCF8523 rtc;

// RTC End

// Temperature START

#include "Adafruit_ADT7410.h"
// Create the ADT7410 temperature sensor object
Adafruit_ADT7410 tempsensor = Adafruit_ADT7410();

// Temperature END

// MiniTFT Start
#include <Adafruit_GFX.h>    // Core graphics library
#include <Adafruit_ST7735.h> // Hardware-specific library for ST7735
#include "Adafruit_miniTFTWing.h"

Adafruit_miniTFTWing ss;
#define TFT_RST    -1     // we use the seesaw for resetting to save a pin
#define TFT_CS   14       // THIS IS DIFFERENT FROM THE DEFAULT CODE
#define TFT_DC   32       // THIS IS DIFFERENT FROM THE DEFAULT CODE
Adafruit_ST7735 tft = Adafruit_ST7735(TFT_CS,  TFT_DC, TFT_RST);

// MiniTFT End

boolean LEDOn = false; // State of Built-in LED true=on, false=off.
#define LOOPDELAY 100

void setup() {
  Serial.begin(9600);
  while (!Serial) {
    delay(10);
  }
  delay(1000);

  if (!SPIFFS.begin(FORMAT_SPIFFS_IF_FAILED)) {
    // Follow instructions in README and install
    // https://github.com/me-no-dev/arduino-esp32fs-plugin
    Serial.println("SPIFFS Mount Failed");
    return;
  }
  if (!ss.begin()) {
    Serial.println("seesaw init error!");
    while (1);
  }
  else Serial.println("seesaw started");

  ss.tftReset();
  ss.setBacklight(0x0); //set the backlight fully on

  // Use this initializer (uncomment) if you're using a 0.96" 180x60 TFT
  tft.initR(INITR_MINI160x80);   // initialize a ST7735S chip, mini display

  tft.setRotation(1);
  tft.fillScreen(ST77XX_BLACK);

  Serial.println("ADT7410 demo");

  // Make sure the sensor is found, you can also pass in a different i2c
  // address with tempsensor.begin(0x49) for example
  if (!tempsensor.begin()) {
    Serial.println("Couldn't find ADT7410!");
    while (1);
  }

  // sensor takes 250 ms to get first readings
  delay(250);

  // Wifi Configuration
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  Serial.println();
  Serial.print("Connected to the Internet");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());

  routesConfiguration(); // Reads routes from routesManagement

  server.begin();

  // RTC
  if (! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    Serial.flush();
    //    abort();
  }
  if (! rtc.initialized() || rtc.lostPower()) {
    logEvent("RTC is NOT initialized, let's set the time!");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }

  rtc.start();

  // MiniTFT Start
  if (!ss.begin()) {
    logEvent("seesaw init error!");
    while (1);
  }
  else logEvent("seesaw started");

  ss.tftReset();
  ss.setBacklight(0x0); //set the backlight fully on

  // Use this initializer (uncomment) if you're using a 0.96" 180x60 TFT
  tft.initR(INITR_MINI160x80);   // initialize a ST7735S chip, mini display

  tft.setRotation(1);
  tft.fillScreen(ST77XX_BLACK);

  // MiniTFT End

  pinMode(LED_BUILTIN, OUTPUT);

}

void loop() {

  builtinLED();
  updateTemperature();
  delay(LOOPDELAY); // To allow time to publish new code.
}

void builtinLED() {
  if (LEDOn) {
    digitalWrite(LED_BUILTIN, HIGH);
  } else {
    digitalWrite(LED_BUILTIN, LOW);
  }
}

void logEvent(String dataToLog) {
  /*
     Log entries to a file stored in SPIFFS partition on the ESP32.
  */
  // Get the updated/current time
  DateTime rightNow = rtc.now();
  char csvReadableDate[25];
  sprintf(csvReadableDate, "%02d,%02d,%02d,%02d,%02d,%02d,",  rightNow.year(), rightNow.month(), rightNow.day(), rightNow.hour(), rightNow.minute(), rightNow.second());

  String logTemp = csvReadableDate + dataToLog + "\n"; // Add the data to log onto the end of the date/time

  const char * logEntry = logTemp.c_str(); //convert the logtemp to a char * variable

  //Add the log entry to the end of logevents.csv
  appendFile(SPIFFS, "/logEvents.csv", logEntry);

  // Output the logEvents - FOR DEBUG ONLY. Comment out to avoid spamming the serial monitor.
  //  readFile(SPIFFS, "/logEvents.csv");

  Serial.print("\nEvent Logged: ");
  Serial.println(logEntry);
}

void tftDrawText(String text, uint16_t color) {
  tft.setCursor(0, 0);
  tft.setTextSize(3);
  tft.setTextColor(color);
  tft.setTextWrap(true);
  tft.print(text);
}

void updateTemperature() {
  // Read and print out the temperature, then convert to *F
  float c = tempsensor.readTempC();
  float f = c * 9.0 / 5.0 + 32;
  Serial.print("Temp: "); Serial.print(c); Serial.print("*C\t");
  Serial.print(f); Serial.println("*F");
  String tempInC = String(c);
  tftDrawText(tempInC, ST77XX_WHITE);
  delay(100);
}
```

# Incorporating Sample Code into the Codebase

In the early stages, the code base  has the basic code for running a web server on the Adafruit ESP32 Feather, and enable the website to turn the built in LED on and off.

The goal of this stage is to further develop this code by incorporating code for the Featherwings to display data on the website (such as the temperature) and allow the user to effect output (such as turn the motor on and off) via the website.

![[Screen_Shot_2022-08-27_at_2.21.56_pm.png|Screen Shot 2022-08-27 at 2.21.56 pm.png]]


> [!info] Rule of Thumb - Any web based code will go in the websiteFunctionality sketch. Any Arduino based code will go in the main sketch.
> Do not change any other files unless otherwise stated.


Remember, there are four basic parts to an Arduino Sketch as shown here.

You will need to incorporate the code into the same section that it appears in the sample code.

For instance, all the include statements in the sample code needs to be added to the include section in the project code base.

![[Structure_of_an_Arduino_Sketch_example_with_code.png]]

## Outputting Sensor Data

This section demonstrates one example of collecting and outputting sensor data through the webpage.

In this case, the project code base will be updated to include capturing the temperature, updating the LCD screen with the current temperature, and also showing it through the webpage.

### Starting Code

This code shown is the starting code for the project, and the “Temperature & LCD” code from the previous stage.


> [!info] There is no need to copy the code at this stage.


- RMS
    
    ```arduino
    #include "sensitiveInformation.h"
    
    #define FORMAT_SPIFFS_IF_FAILED true
    
    // Wifi & Webserver
    #include "WiFi.h"
    #include "SPIFFS.h"
    #include <AsyncTCP.h>
    #include <ESPAsyncWebServer.h>
    
    AsyncWebServer server(80);
    
    // RTC Start - Remove if unnecessary
    #include "RTClib.h"
    
    RTC_PCF8523 rtc;
    char daysOfTheWeek[7][12] = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
    
    // RTC End
    
    boolean LEDOn = false; // State of Built-in LED true=on, false=off.
    #define LOOPDELAY 100
    
    void setup() {
      Serial.begin(9600);
      while (!Serial) {
        delay(10);
      }
      delay(1000);
    
      if (!SPIFFS.begin(FORMAT_SPIFFS_IF_FAILED)) {
        // Follow instructions in README and install
        // https://github.com/me-no-dev/arduino-esp32fs-plugin
        Serial.println("SPIFFS Mount Failed");
        return;
      }
    
      // Wifi Configuration
      WiFi.begin(ssid, password);
      while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("Connecting to WiFi..");
      }
      Serial.println();
      Serial.print("Connected to the Internet");
      Serial.print("IP address: ");
      Serial.println(WiFi.localIP());
    
      routesConfiguration(); // Reads routes from routesManagement
    
      server.begin();
    
      // RTC
      if (! rtc.begin()) {
        Serial.println("Couldn't find RTC");
        Serial.flush();
        //    abort();
      }
    
      // The following line can be uncommented if the time needs to be reset.
      rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
      rtc.start();
      pinMode(LED_BUILTIN, OUTPUT);
    
    }
    
    void loop() {
    
      builtinLED();
    
      delay(LOOPDELAY); // To allow time to publish new code.
    }
    
    void builtinLED() {
      if (LEDOn) {
        digitalWrite(LED_BUILTIN, HIGH);
      } else {
        digitalWrite(LED_BUILTIN, LOW);
      }
    }
    
    void logEvent(String dataToLog) {
      /*
         Log entries to a file stored in SPIFFS partition on the ESP32.
      */
      // Get the updated/current time
      DateTime rightNow = rtc.now();
      char csvReadableDate[25];
      sprintf(csvReadableDate, "%02d,%02d,%02d,%02d,%02d,%02d,",  rightNow.year(), rightNow.month(), rightNow.day(), rightNow.hour(), rightNow.minute(), rightNow.second());
    
      String logTemp = csvReadableDate + dataToLog + "\n"; // Add the data to log onto the end of the date/time
    
      const char * logEntry = logTemp.c_str(); //convert the logtemp to a char * variable
    
      //Add the log entry to the end of logevents.csv
      appendFile(SPIFFS, "/logEvents.csv", logEntry);
    
      // Output the logEvents - FOR DEBUG ONLY. Comment out to avoid spamming the serial monitor.
      //  readFile(SPIFFS, "/logEvents.csv");
    
      Serial.print("\nEvent Logged: ");
      Serial.println(logEntry);
    }
    ```
    

- websiteFunctionality
    
    ```arduino
    void routesConfiguration() {
    
      // Example of a 'standard' route
      // No Authentication
      server.on("/index.html", HTTP_GET, [[AsyncWebServerRequest * request]] {
        logEvent("route: /");
        request->send(SPIFFS, "/index.html", "text/html");
      });
    
      // Duplicated serving of index.html route, so the IP can be entered directly to browser.
      // No Authentication
      server.on("/", HTTP_GET, [[AsyncWebServerRequest * request]] {
        logEvent("route: /");
        request->send(SPIFFS, "/index.html", "text/html");
      });
    
      // Example of linking to an external file
      server.on("/arduino.css", HTTP_GET, [[AsyncWebServerRequest * request]] {
        request->send(SPIFFS, "/arduino.css", "text/css");
      });
    
      // Example of a route with additional authentication (popup in browser)
      // And uses the processor function.
      server.on("/dashboard.html", HTTP_GET, [[AsyncWebServerRequest * request]] {
        if (!request->authenticate(http_username, http_password))
          return request->requestAuthentication();
        logEvent("Dashboard");
        request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
      });
    
      // Example of route with authentication, and use of processor
      // Also demonstrates how to have arduino functionality included (turn LED on)
      server.on("/LEDOn", HTTP_GET, [[AsyncWebServerRequest * request]] {
        if (!request->authenticate(http_username, http_password))
          return request->requestAuthentication();
        LEDOn = true;
        request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
      });
    
      server.on("/LEDOff", HTTP_GET, [[AsyncWebServerRequest * request]] {
        if (!request->authenticate(http_username, http_password))
          return request->requestAuthentication();
        LEDOn = false;
        request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
      });
    
      // Example of route which sets file to download - 'true' in send() command.
      server.on("/logOutput", HTTP_GET, [[AsyncWebServerRequest * request]] {
        if (!request->authenticate(http_username, http_password))
          return request->requestAuthentication();
        logEvent("Log Event Download");
        request->send(SPIFFS, "/logEvents.csv", "text/html", true);
      });
    }
    
    String getDateTime() {
      DateTime rightNow = rtc.now();
      char csvReadableDate[25];
      sprintf(csvReadableDate, "%02d:%02d:%02d %02d/%02d/%02d",  rightNow.hour(), rightNow.minute(), rightNow.second(), rightNow.day(), rightNow.month(), rightNow.year());
      return csvReadableDate;
    }
    
    String processor(const String& var) {
      /*
         Updates the HTML by replacing set variables with return value from here.
         For example:
         in HTML file include %VARIABLEVALUE%.
         In this function, have:
          if (var=="VARIABLEVALUE") { return "5";}
      */
    
      if (var == "DATETIME") {
        String datetime = getDateTime();
        return datetime;
      }
    
      // Default "catch" which will return nothing in case the HTML has no variable to replace.
      return String();
    }
    ```
    

- Temperature & LCD
    
    ```arduino
    /***************************************************
      This is a library for the Adafruit 0.96" Mini TFT Featherwing
      
      Adafruit invests time and resources providing this open source code,
      please support Adafruit and open-source hardware by purchasing
      products from Adafruit!
    
      Written by Limor Fried/Ladyada for Adafruit Industries.
      MIT license, all text above must be included in any redistribution
     ****************************************************/
    /**************************************************************************/
    /*!
      This is a demo for the Adafruit ADT7410 breakout
      ----> http://www.adafruit.com/products/4089
      Adafruit invests time and resources providing this open source code,
      please support Adafruit and open-source hardware by purchasing
      products from Adafruit!
    */
    /**************************************************************************/
    
    #include <Wire.h>
    #include "Adafruit_ADT7410.h"
    #include <Adafruit_GFX.h>    // Core graphics library
    #include <Adafruit_ST7735.h> // Hardware-specific library for ST7735
    #include "Adafruit_miniTFTWing.h"
    
    Adafruit_miniTFTWing ss;
    #define TFT_RST    -1    // we use the seesaw for resetting to save a pin
    #define TFT_CS   14
    #define TFT_DC   32
    
    // Create the ADT7410 temperature sensor object
    Adafruit_ADT7410 tempsensor = Adafruit_ADT7410();
    
    Adafruit_ST7735 tft = Adafruit_ST7735(TFT_CS,  TFT_DC, TFT_RST);
    
    void setup() {
      Serial.begin(9600);
    
      if (!ss.begin()) {
        Serial.println("seesaw init error!");
        while (1);
      }
      else Serial.println("seesaw started");
    
      ss.tftReset();
      ss.setBacklight(0x0); //set the backlight fully on
    
      // Use this initializer (uncomment) if you're using a 0.96" 180x60 TFT
      tft.initR(INITR_MINI160x80);   // initialize a ST7735S chip, mini display
    
      tft.setRotation(3);
      tft.fillScreen(ST77XX_BLACK);
    
      
      Serial.println("ADT7410 demo");
    
      // Make sure the sensor is found, you can also pass in a different i2c
      // address with tempsensor.begin(0x49) for example
      if (!tempsensor.begin()) {
        Serial.println("Couldn't find ADT7410!");
        while (1);
      }
    
      // sensor takes 250 ms to get first readings
      delay(250);
    }
    
    void loop() {
      // Read and print out the temperature, then convert to *F
      float c = tempsensor.readTempC();
      float f = c * 9.0 / 5.0 + 32;
      Serial.print("Temp: "); Serial.print(c); Serial.print("*C\t");
      Serial.print(f); Serial.println("*F");
      String tempInC = String(c);
      tftDrawText(tempInC, ST77XX_WHITE);
      delay(1000);
    }
    
    void tftDrawText(String text, uint16_t color) {
      tft.fillScreen(ST77XX_BLACK);
      tft.setCursor(0, 0);
      tft.setTextSize(3);
      tft.setTextColor(color);
      tft.setTextWrap(true);
      tft.print(text);
    }
    ```
    

### Includes Section

Copy the code which includes all the libraries to include from the Temperature & LCD code to the include section Project Code.

![[Screen_Shot_2022-08-27_at_9.11.02_pm.png|Which code to move and where]]

Which code to move and where

![[Screen_Shot_2022-08-27_at_9.11.57_pm.png|Screen Shot 2022-08-27 at 9.11.57 pm.png]]

### Variable Definition Section

Copy the section of code which defines all global variable into the Project code.

![[Screen_Shot_2022-08-27_at_9.13.13_pm.png|Which code to move and where]]

Which code to move and where

![[Screen_Shot_2022-08-27_at_9.15.06_pm.png|End Result]]

End Result

### Custom Functions

Copy all relevant custom written functions (all except for `setup()` and `loop()`) and paste them into the Project Code Base.

![[Screen_Shot_2022-08-27_at_9.16.51_pm.png|Example of custom function/s.]]

Example of custom function/s.

![[Screen_Shot_2022-08-27_at_9.18.04_pm.png|End Result]]

End Result

### `Setup()`

Copy all relevant code in `setup()`, which is required to initialise the new modules.

> [!info]  Do not copy the `Setup.begin()` line or any other code which is already in the destination setup().


![[Screen_Shot_2022-08-27_at_9.21.22_pm.png|Example of relevant module initialisation code.]]

Example of relevant module initialisation code.

![[Screen_Shot_2022-08-27_at_9.23.42_pm.png|End Result]]

End Result

> [!info] The exact code you need to copy depends on the module you’re incorporating. Check the code carefully before attempting.


### `Loop()`

There may be relevant code needed in `loop()`, however every piece of code is different. It’s important to check the code you’re copying from to understand the purpose of the code prior to copying code straight from `loop()` to `loop()`.

For example, the temperature sketch’s `loop()` is shown here.

This code works, and the placement of this code in that sketch is fine, however you’re incorporating this code into a larger code base which has multiple purposes, so it’s not wise to simply place it in the code bases `loop()`.

![[Screen_Shot_2022-08-27_at_9.26.00_pm.png|Screen Shot 2022-08-27 at 9.26.00 pm.png]]

*In this case* it would be better to take the code inside `loop()` and turn it into a custom function, naming it appropriate to the purpose, for example `updateTemperature()`. 

Create the structure of the function.

![[Screen_Shot_2022-08-27_at_10.09.09_pm.png|Screen Shot 2022-08-27 at 10.09.09 pm.png]]

Copy the code from temperature sketch’s `loop()` into this new function. 

> [!info]  Notice the change from `delay(1000)` to `delay(100)`. This is because any `delay()` call will impact the speed of the remainder of the code, possibly even the website responsiveness.


![[Screen_Shot_2022-08-27_at_10.10.51_pm.png|Screen Shot 2022-08-27 at 10.10.51 pm.png]]

You can now call this function from `loop()`.

![[Screen_Shot_2022-08-27_at_11.09.10_pm.png|Screen Shot 2022-08-27 at 11.09.10 pm.png]]

# Automatic Fan Subsystem

> [!info] This subsystem automatically turns the fan on when the temperature gets too hot. From a technical perspective, using the temperature from the featherwing, this determines whether to have the motor activated or not.


## Includes section

In the include section, add this code (if not already added):

```arduino
// Motor Shield START
#include <Adafruit_MotorShield.h>
Adafruit_MotorShield AFMS = Adafruit_MotorShield(); 
Adafruit_DCMotor *myMotor = AFMS.getMotor(3);
// Motor Shield END
```

> [!info] Note that `getMotor(3)` refers to the DC motor being attached to M3 on the board. Change this number to match where the motor is connected.  Use the image below to confirm.
> ![[feather_2927-00.jpeg]]


## Setup()

Add the following code to setup(). 

```arduino
AFMS.begin(); // Motor Shield Start
```

## Custom Function

Include this function at the end of the code (outside any other function). 

This code reads the temperature from the temperature featherwing, and if that value is below the threshold value (to be set elsewhere), the motor is turned off (`myMotor->run(RELEASE);`). If this `if` is false, then the motor is turned on (`myMotor->run(FORWARD);`)

```arduino
void automaticFan(float temperatureThreshold) {
  float c = tempsensor.readTempC();
  myMotor->setSpeed(100); 
  if (c < temperatureThreshold) {
    myMotor->run(RELEASE);
    Serial.println("stop");
  } else {
    myMotor->run(FORWARD);
    Serial.println("forward");
  }
}
```

## Activating the Functionality

To activate this functionality, update loop() by calling the function as shown here.

![[Screen_Shot_2022-09-01_at_2.20.55_pm.png|Screen Shot 2022-09-01 at 2.20.55 pm.png]]

```arduino
automaticFan(20.0);
```

# Window Blind Control Subsystem

> [!info]  This subsystem involves the manual control of blinds. From a technical perspective, Button A on the Mini TFT Featherwing will control a servo (opening and closing the blind)


## Wiring

The servo will need to be wired to VCC, GND and Pin 12 as per the diagram.

> [!info] In some servos, the VCC cable is Red. The GND is Brown and the data cable is yellow.


## Library

Using a servo motor with the ESP32 chip is a little different than using one with an Arduino Uno and requires an additional library - `ESP32Servo`.

Open Manage Libraries under Tools → Manage Libraries and search for `esp32servo`. 

Click Close when the library installation is complete.

![[Screen_Shot_2022-09-01_at_10.28.30_pm.png|Screen Shot 2022-09-01 at 10.28.30 pm.png]]

## Includes

Add the following code in the **Includes** section of the code (at the top) to include the library and configure the servo.

Notice that `servoPin` is set to 12. This means the servo needs to be attached to pin 12 in this example. If the servo is attached to another pin, then change the `servoPin` value.

```arduino
// ESP32Servo Start
#include <ESP32Servo.h>
Servo myservo;  // create servo object to control a servo
int servoPin = 12;
boolean blindsOpen = false;
// ESP32Servo End
```

## setup()

Add the following code to `setup()`.

```arduino
// ESP32Servo Start
ESP32PWM::allocateTimer(0);
ESP32PWM::allocateTimer(1);
ESP32PWM::allocateTimer(2);
ESP32PWM::allocateTimer(3);
myservo.setPeriodHertz(50);    // standard 50 hz servo
myservo.attach(servoPin, 1000, 2000); // attaches the servo on pin 18 to the servo object
// ESP32Servo End
```

## Custom Function

The functionality that is being attempted is: when the user presses Button A on the Mini TFT wing, the servo will activate, moving from 0 → 180, or 180 → 0 degrees.

Create the new function.

This function does not need to take any arguments, nor does it need to return any data. 

```arduino
void windowBlinds() {
  
}
```

Read the state of the buttons using the Seesaw library.

This will read the values (pressed or not pressed) for all the buttons on the board - each direction of the joystick and the two additional buttons (Button A and Button B)

![[Screen_Shot_2022-09-01_at_10.58.14_pm.png|Screen Shot 2022-09-01 at 10.58.14 pm.png]]

```arduino
uint32_t buttons = ss.readButtons();
```

The variable declared in the includes section - `blindsOpen` keeps track of whether the blinds are open or closed, with will equate to the servo being set to 0 or 180.

Add the an if statement to check if the value of `blindsOpen` is true or false.

![[Screen_Shot_2022-09-01_at_11.13.37_pm.png|Screen Shot 2022-09-01 at 11.13.37 pm.png]]

```arduino
void windowBlinds() {
  uint32_t buttons = ss.readButtons();
  if (! (buttons & TFTWING_BUTTON_A)) {
    if (blindsOpen) {
     
    } else {

    }
  }
}
```

Add the code to set the position of the servo depending on the state of `blindsOpen`.

![[Screen_Shot_2022-09-01_at_11.15.20_pm.png|Screen Shot 2022-09-01 at 11.15.20 pm.png]]

Finally, change the value of `blindsOpen` to the opposite of what is was using the `NOT` operator.

![[Screen_Shot_2022-09-01_at_11.23.12_pm.png|Screen Shot 2022-09-01 at 11.23.12 pm.png]]

```arduino
blindsOpen = !blindsOpen;
```

## Activating Functionality

Call the new function from `loop()`. Upload the code and the servo should alternate between the two positions.

![[Screen_Shot_2022-09-01_at_11.07.51_pm.png|Screen Shot 2022-09-01 at 11.07.51 pm.png]]

# Safe Security Subsystem

This subsystem simulates the hotel room safe for guests to store their valuables (money, passport etc). When the guests check into the hotel, they’re issued with an RFID tag or card which can be read by the safe’s locking mechanism. If the tag/card is the correct one, the safe unlocks.

The traffic light module will be used as indicators of the safe locking status. Red will be Locked. Green will be unlocked.

> [!info] This has been tested and confirmed to work with the Adafruit ESP32 Feather and the RFID-RC522 board.


![[rfid.png]]

![[traffic_light.png|traffic light.png]]

## Wiring

Wire the RFIC board to the Feather using this wiring diagram and information as a guide.

**RFID Board**

| RFID Board Pin | ESP32 Feather Pin |
| --- | --- |
| VCC | VCC |
| RST | 17 |
| GND | GND |
| MISO | 19 |
| MOSI | 18 |
| SCK | 5 |
| NSS | 21 |
| IRQ | Not connected |

**LEDs**

| LED | ESP32 Feather Pin |
| --- | --- |
| Red | 27 |
| Green | 33 |

![[rfid_leds.png|rfid leds.png]]

## Test Sample Code

Using the library manager install the following library - MFRC522

![[Screen_Shot_2022-09-08_at_9.36.35_pm.png|Screen Shot 2022-09-08 at 9.36.35 pm.png]]

Use the following sample code to test the wiring.

```arduino
/*
 * This ESP32 code is created by esp32io.com
 *
 * This ESP32 code is released in the public domain
 *
 * For more detail (instruction and wiring diagram), visit https://esp32io.com/tutorials/esp32-rfid-nfc
 */

#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN  21  // ES32 Feather
#define RST_PIN 17 // esp32 Feather. Could be others.

MFRC522 rfid(SS_PIN, RST_PIN);

void setup() {
  Serial.begin(9600);
  SPI.begin(); // init SPI bus
  rfid.PCD_Init(); // init MFRC522

  Serial.println("Tap an RFID/NFC tag on the RFID-RC522 reader");
}

void loop() {
  if (rfid.PICC_IsNewCardPresent()) { // new tag is available
    if (rfid.PICC_ReadCardSerial()) { // NUID has been readed
      MFRC522::PICC_Type piccType = rfid.PICC_GetType(rfid.uid.sak);
      Serial.print("RFID/NFC Tag Type: ");
      Serial.println(rfid.PICC_GetTypeName(piccType));

      // print UID in Serial Monitor in the hex format
      Serial.print("UID:");
      for (int i = 0; i < rfid.uid.size; i++) {
        Serial.print(rfid.uid.uidByte[i] < 0x10 ? " 0" : " ");
        Serial.print(rfid.uid.uidByte[i], HEX);
      }
      Serial.println();

      rfid.PICC_HaltA(); // halt PICC
      rfid.PCD_StopCrypto1(); // stop encryption on PCD
    }
  }
}
```

When uploaded, test it with a number of cards to check to make sure it’s working correctly. If it is, you should receive some data through the Serial Monitor that is similar to this.

> [!info]  Make note of the UID of your card! You’ll need this later.


![[Screen_Shot_2022-09-08_at_9.35.14_pm.png|Screen Shot 2022-09-08 at 9.35.14 pm.png]]

## Incorporate the subsystem into the project

To incorporate the RFID and LEDs into the system, there are a few additions that need to be made to our project code.

Open the Project code and add the following to the includes section of the code.

```arduino
#define LEDRed 27
#define LEDGreen 33
// RFID Start

#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN  21  // ES32 Feather
#define RST_PIN 17 // esp32 Feather - SCL pin. Could be others.

MFRC522 rfid(SS_PIN, RST_PIN);
bool safeLocked = true;

// RFID End
```

Add the following code to `setup()`. 

```arduino
// RFID Start
SPI.begin(); // init SPI bus
rfid.PCD_Init(); // init MFRC522
// RFID End
pinMode(LEDRed, OUTPUT);
pinMode(LEDGreen, OUTPUT);
digitalWrite(LEDRed, LOW);
digitalWrite(LEDGreen, LOW);
```

### Custom Functions

The first main custom function that is required is a slight modification of the sample code. This function reads the UID of the card that’s detected, the same as previously. The added functionality then compares that UID against a card which is considered **correct**.

Add the custom function to the code, outside any other function.

> [!info] You’ll need to change the value of `validCardUID` to match the one identified earlier.


A few items to notice with this function:

- `uidOfCardRead` is **cleared** each time the function is called.
- Some of the additional Serial.println() commands from the sample code have been removed for simplicity.
- The `for` loop, instead out outputting the card’s UID, builds a string of the cards UID.
- `uidOfCardRead.trim()` removes any whitespace (spaces, tabs etc) from the start and the end of the string.
- Depending on whether the card is valid or not, the only instructions the code executes is setting the value of `safeLocked` and logging the event.

```arduino
void readRFID() {

  String uidOfCardRead = "";
  String validCardUID = "00 232 81 25";

  if (rfid.PICC_IsNewCardPresent()) { // new tag is available
    if (rfid.PICC_ReadCardSerial()) { // NUID has been readed
      MFRC522::PICC_Type piccType = rfid.PICC_GetType(rfid.uid.sak);
      for (int i = 0; i < rfid.uid.size; i++) {
        uidOfCardRead += rfid.uid.uidByte[i] < 0x10 ? " 0" : " ";
        uidOfCardRead += rfid.uid.uidByte[i];
      }
      Serial.println(uidOfCardRead);

      rfid.PICC_HaltA(); // halt PICC
      rfid.PCD_StopCrypto1(); // stop encryption on PCD
      uidOfCardRead.trim();
      if (uidOfCardRead == validCardUID) {
        safeLocked = false;
        logEvent("Safe Unlocked");
      } else {
        safeLocked = true;
        logEvent("Safe Locked");
      }
    }
  }
}
```

The next custom function to add is the function which turns the LEDs on depending of if the safe is locked or unlocked.

This code simply checks the value of `safeLocked` and turns the required LED on and the other one off.

```arduino
void safeStatusDisplay() {
  /*
     Outputs the status of the Safe Lock to the LEDS
     Red LED = Locked
     Green LED = Unlocked.
  */
  if (safeLocked) {
    digitalWrite(LEDRed, HIGH);
    digitalWrite(LEDGreen, LOW);
  } else {
    digitalWrite(LEDRed, LOW);
    digitalWrite(LEDGreen, HIGH);
  }
}
```

The reason these two functions have been separated instead of incorporating the code to write to the LEDs into the RFID function is to allow for further functionality for the project, but also to follow the basic principle of functions - **they should only do one job.** In this case one function checks whether the card is valid or not, the other sets the LEDs based on the safe locked status.

> [!info] Separating the code as has been done, will allow the `safeLocked` variable to be set elsewhere in the code, but the LED output functionality remains the same.


## Extension

Having a safe that automatically locks after a set period would be useful. How could this be implemented? For example, after 10 seconds of the safe being unlocked the system automatically locks the door again. Hint: Look at the `BlinkWithoutDelay` example code built into Arduino.

# Code Style

At this stage, hopefully your code is functional and achieves set outcomes.  Now it’s time to consider the style of your code and the coding standards.

There are a few items to attend to:

1. **All variable and function names** should indicate what its purpose is.
    1. No single letter variable names! 
2. **Each custom function is to include a function header.** This is a comment which describes the purpose of the function, and any arguments and return types.
    - For Example
        
        ```arduino
        void waterPlant(int moistureValue) {
          /*
             Write a function which takes the moisture value,
             and if it's below a certain value, turns the pump on.
             The function is to be called waterPlant() which will
             take the moisture value as an argument, and return no value.
             @param moistureValue int measured from Moisture Sensor
             @return: void
          */
          if (moistureValue < 1000 ) {
            // motor/pump on
            myMotor->run(FORWARD); // May need to change to BACKWARD
            pumpIsRunning = true;
          } else {
            // motor/pump off
            myMotor->run(RELEASE);
            pumpIsRunning = false;
          }
        
        }
        ```
        
3. **Complex Code should be commented** to explain its purpose.
4. **Any** time there is an event in the system, the **event is to be logged**!

![[codeInspirational.png]]

---

# Arduino Website Analysis

There are a number of areas that are important for the webserver integration on the Adafruit ESP32 Feather. 

## Web Server Code

This code handles incoming requests, runs any server code and sends Web Content to the browser.
For Example: A user attempts to access the URL `/LEDOn` on the ESP32. The route (`/LEDOn`) is identified through the code, the `LEDOn` variable is set to `true`, and the ESP32 sends back the file `dashboard.html.`

![[Screen_Shot_2022-09-12_at_10.49.07_pm.png|Screen Shot 2022-09-12 at 10.49.07 pm.png]]

The majority of webserver code will be written in the websiteFunctionality tab.

![[Screen_Shot_2022-09-12_at_10.39.58_pm.png|Screen Shot 2022-09-12 at 10.39.58 pm.png]]

> [!info]  Only Routes coded will respond on the webserver. I.e. if `LEDOn` is not an identified route, that URL will time out and nothing will occur for the user.


## Where to find Webserver Content

To find the HTML and other web files, To to Sketch → Show Sketch folder.

![[Screen_Shot_2022-09-12_at_10.41.16_pm.png|Screen Shot 2022-09-12 at 10.41.16 pm.png]]

The HTML code can be found in the data folder.

![[Screen_Shot_2022-09-12_at_10.41.30_pm.png|Screen Shot 2022-09-12 at 10.41.30 pm.png]]

## How to upload Webserver Content

To update the Arduino with the latest web code, you will need to Select Tools → ESP32 Sketch Data Upload.

> [!important] This uploads the entire data folder
> This erases anything in the SPIFFS partition on the Arduino ESP32 Feather.
> The Serial Monitor **cannot** be open for this process to run.


![[Screen_Shot_2022-09-12_at_10.43.13_pm.png|Screen Shot 2022-09-12 at 10.43.13 pm.png]]

## Route Analysis

The default `/dashboard.html` route is:

This contains a lot of information in the few short lines. Let’s analyse it.

First this route will execute (run) if the user attempts to access `dashboard.html`. 

The first code that is executed is to check to see if the user has not authenticated yet.

The authentication is based on username and password, which are defined by the `http_username` and `http_password` variables defined in `sensitiveInformation`.

If the user hasn’t been authenticated with `http_username` and `http_password`, the server will send a message to the browser to collect that information.

Once authenticated, the system logs the event by running logEvent()

The server then sends `dashboard.html` to the browser.

False here indicates the the file is **not** set to download. If you wish the file to prompt to download (such as with the log file), this would be set to `true`.

Finally, processor is a function that will be run before the file (dashboard.html) is sent back to the browser.

## Processor() Analysis

The processor function is used to modify aspects of the webpage before the file is sent to the users browsers. This can be used to output data on the webpage.

In this initial version, you’ll see that the argument `var` is checked to see if it equals `DATETIME` (line 86).

If the variable is DATE TIME, then the function returns `datetime` which is the response to the function `getDateTime().` 

In essence, `processor` will search through the HTML file given and replace any instance of `%DATETIME%` with the current date and time. 

This can be used for many different pieces of data.

![[Screen_Shot_2022-09-13_at_9.55.20_am.png|Screen Shot 2022-09-13 at 9.55.20 am.png]]

---

# Website Development

The website on the Adafruit ESP32 Feather needs to serve the homepage and dashboard, as well as offer some security, and need to receive and display sensor data (such as the current temperature) and be able to send commands to hardware (such as turn motors on and off).

## Page Not Found (404) route

[HTTP 404](https://en.wikipedia.org/wiki/HTTP_404) is a standard error code for web servers to indicate that the page requested (on the browser) does not exist on the server. Currently, the Webserver if a user attempts to access a page that doesn’t exist, the server will timeout and give a non-helpful message. These steps show how to implement the functionality for the webserver to serve an error page, while still keeping the user ‘in’ the site (use the same basic website design, navbar etc).

Open the `websiteFunctionality` tab and add the following route within `routesConfiguration()`. 

![[Screen_Shot_2022-09-12_at_11.19.58_pm.png|Screen Shot 2022-09-12 at 11.19.58 pm.png]]

```arduino
server.onNotFound([[AsyncWebServerRequest * request]] {
    request->send(SPIFFS, "/404.html");
  });
```

Open the `data` folder within the sketch folder and create a new file called `404.html`.  Enter the following HTML code in the file and save the file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="arduino.css">
    <meta charset="UTF-8">
    <title>Homepage</title>
</head>
<body>
<nav>
    <ul>
        <li class="active"><a href="index.html">Home</a></li>
        <li><a href="/dashboard.html">Dashboard</a></li>
        <li><a href="/admin">Admin</a></li>
    </ul>
</nav>
<h1>Page not Found!</h1>
<p>Please go back to the Home page</p>

</body>
</HTML>
```

Run the `ESP32 Sketch Data Upload` menu option. When this has completed compile and upload the code to the Arduino.

When this processes have completed, find the IP address in the Serial Monitor and attempt to access a page you know doesn’t exist.

![[Screen_Shot_2022-09-12_at_11.24.15_pm.png|Screen Shot 2022-09-12 at 11.24.15 pm.png]]

## Displaying the Temperature

> [!info] This process can be following to display any sensor data. The only changes needed would the variable names, and how to format the variables to output as a String in the processor() function.


### Update the Dashboard

One of the simple updates that can be made to the website is to display the temperature. These steps will show how to output the current temperature in the empty box on the dashboard.

![[Screen_Shot_2022-09-13_at_9.59.21_am.png|Screen Shot 2022-09-13 at 9.59.21 am.png]]

The first step is to edit the HTML file. Open `dashboard.html` in the sketch data folder.

> [!info]  This file can be accessed by selecting Sketch→Show Sketch Data Folder in the Arduino IDE.


Open the file to edit in your favourite text editor. It is recommended to use Visual Studio Code, but not mandatory.

![[Screen_Shot_2022-09-13_at_10.03.08_am.png|Screen Shot 2022-09-13 at 10.03.08 am.png]]

Find the empty cell, indicated by `<div></div>`. 

![[Screen_Shot_2022-09-13_at_10.04.03_am.png|Screen Shot 2022-09-13 at 10.04.03 am.png]]

Between the div tags, enter the following code.

```html
<p>Current Temperature</p><p>%TEMPERATURE%</p>
```

![[Screen_Shot_2022-09-13_at_10.06.10_am.png|Screen Shot 2022-09-13 at 10.06.10 am.png]]

Save the File. 

Return to the Arduino IDE and select **Tools→ESP32 Sketch Data Upload** to upload the updated HTML file on the Adafruit ESP32 Feather.

![[Screen_Shot_2022-09-13_at_10.07.59_am.png|Screen Shot 2022-09-13 at 10.07.59 am.png]]

### Update the Arduino Code

Open `websiteFunctionality` and look for the `processor` function.

Edit the function to look for the TEMPERATURE placeholder created in the HTML file.

![[Screen_Shot_2022-09-13_at_10.16.36_am.png|Screen Shot 2022-09-13 at 10.16.36 am.png]]

```arduino
if (var == "TEMPERATURE") {
  return String(tempsensor.readTempC());
}
```

Save the file.

### Test

Compile and Upload the code to the Adafruit ESP32 Feather. Access the website (check the serial monitor for the IP address if needed).

You should now see the temperature being displayed in the cell that was previously empty.

![[Screen_Shot_2022-09-13_at_10.18.09_am.png|Screen Shot 2022-09-13 at 10.18.09 am.png]]

### Extension

Modify the code (Arduino or HTML) to display the temperature **without** the decimal point part of it, and with ℃ following the value. 

![[Screen_Shot_2022-09-13_at_10.27.51_am.png|Screen Shot 2022-09-13 at 10.27.51 am.png]]

![[commonBlocks#Commit & Push]]


## Opening and closing the safe

There is already a hardware solution to this problem (reading the correct RFID card) however it may be useful to allow for a web-based solution, so guests can lock and unlock the safe in case the card is unavailable.

### Update the Dashboard

Open the edit `dashboard.html` in your text-editor of choice.

Add a new row by creating two new `<div></div>` tags.

> [!info] The configuration of the CSS in this HTML file allows for cells to be added into the layout of the site with each new `<div></div>` tag. The grid layout has been set to 2 columns, meaning a row is 2 sets of `<div></div>` tags.


![[Screen_Shot_2022-09-13_at_3.04.54_pm.png|Screen Shot 2022-09-13 at 3.04.54 pm.png]]

The site will now look like this.

> [!info] You can open the page locally, meaning off the hard drive you’re developing on, to see how the page will render (what it will look like).


![[Screen_Shot_2022-09-13_at_3.07.09_pm.png|Screen Shot 2022-09-13 at 3.07.09 pm.png]]

In the first new div tag, add links to lock and unlock the safe.

![[Screen_Shot_2022-09-13_at_3.08.35_pm.png|Screen Shot 2022-09-13 at 3.08.35 pm.png]]

```html
<p><a href="/SafeLock">Lock Safe</a></p>
<p><a href="/SafeUnlock">Unlock Safe</a></p>
```

The HTML page will now look like this.

![[Screen_Shot_2022-09-13_at_3.09.48_pm.png|Screen Shot 2022-09-13 at 3.09.48 pm.png]]

Save the File.

Upload the Website content using the **ESP32 Sketch Data Upload** menu option.

### Update the Arduino Code

The website has been coded so that there are two links that need to be handled - `/SafeLock` and `/SafeUnlock`. New routes will need to be added to handle these requests. 

Both functions will require similar configurations - authentication, log the event and to finally serve `dashboard.html` back to the client. This will mean that the user will be able to lock and unlock the safe, and the website will effectively reload the page. 

At this stage the server won’t actually lock and unlock the safe (red vs green LED), but will only log the event and reload the page.

![[Screen_Shot_2022-09-13_at_3.24.59_pm.png|Screen Shot 2022-09-13 at 3.24.59 pm.png]]

```arduino
server.on("/SafeLock",  HTTP_GET, [[AsyncWebServerRequest * request]] {
  if (!request->authenticate(http_username, http_password))
    return request->requestAuthentication();
  logEvent("Safe Locked via Website");
  request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
});

server.on("/SafeUnlock",  HTTP_GET, [[AsyncWebServerRequest * request]] {
  if (!request->authenticate(http_username, http_password))
    return request->requestAuthentication();
  logEvent("Safe Unlocked via Website");
  request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
});
```

The locking functionality is done through the use of the already coded `safeLocked` boolean variable defined and used in the main code.

> [!info] The code in `websiteFunctionality` can access and change the values of global variables defined in the main sketch.
See the LEDOn and LEDOff routes for previous examples.


![[Screen_Shot_2022-09-13_at_3.27.51_pm.png|Screen Shot 2022-09-13 at 3.27.51 pm.png]]

Update the two new routes to change the value of `safeLocked` depending on the route accessed.

![[Screen_Shot_2022-09-13_at_3.30.27_pm.png|Screen Shot 2022-09-13 at 3.30.27 pm.png]]

### Test

Compile and upload the Arduino code, and ensure that the website code has been uploaded as well.

Access the website and see if you can lock and unlock the safe!

![[safeLockUnlock.gif]]

### Extension

Update the website to additional show the current status of the safe - Locked or Unlocked.

![[safeLockUnlockHeading.gif]]

![[commonBlocks#Commit & Push]]

## Controlling the Fan

Currently the fan (DC motor) is controlled automatically via the temperature - if the temperature is above a set threshold, then the fan turns on. If it’s below that threshold, the fan turns off. Controlling the fan via the website requires some modification of the code. The logic will remain the same, however how it achieves this will be align closer to the safe opening and closing.

The fan control subsystem will need to have an **automatic** mode and a **manual** control mode. 

### Main Code Changes

Start by creating two global boolean variables in the Initialisation section of code. 

`fanEnabled` which controls if the fan is to be on or off.

`automaticFanControl` indicates if the fan is to be operated automatically (via temperature control) or manually (via the website)

![[Screen_Shot_2022-10-02_at_11.24.46_pm.png|Screen Shot 2022-10-02 at 11.24.46 pm.png]]

```arduino
bool fanEnabled = false;            // If the fan is on or off.
bool automaticFanControl = true;    // Automatic or manual control
```

Look for `automaticFan()`  and modify it so that instead of directly turning the motors on and off, it instead sets the value of the `fanEnabled` boolean.

This will allow the subsystem to not cause conflicts between automatic or manual mode. 

> [!info] As with other subsystems, there are other approaches to this problem, however this is simple and is similar to the approaches made in other parts of the code.


![[Screen_Shot_2022-10-02_at_11.26.33_pm.png|Screen Shot 2022-10-02 at 11.26.33 pm.png]]

Create a new function - `fanControl()` - which will check the status of the `automaticFanControl` variable (indicating if the fan subsystem is in automatic or manual mode). If `automaticFanControl` is true, then it will **call** `automaticFan()`. 

Regardless of the status of `automaticFanControl`, the function will then turn the motor on or off depending on the value of `fanEnabled`. 

This will allow the automatic control to run as has been done previously, but also add the ability to run in manual mode, with `fanEnabled` being set via the website (see below)

![[Screen_Shot_2022-10-02_at_11.30.56_pm.png|Screen Shot 2022-10-02 at 11.30.56 pm.png]]

```arduino
void fanControl() {
  if (automaticFanControl) {
    automaticFan(25.0);
  }
  if (fanEnabled) {
    myMotor->run(FORWARD);
  } else {
    myMotor->run(RELEASE);
  }
}
```

Finally, at least in the main code, change `loop()` to call `fanControl()` **instead** of `automaticFanControl()`.

![[Screen_Shot_2022-10-02_at_11.33.29_pm.png|Screen Shot 2022-10-02 at 11.33.29 pm.png]]

> [!info] At this stage it would be advisable to compile and upload your code to ensure that the automatic functionality works as it did previously.


### dashboard.html Changes

Open `dashboard.html` in your text editor of choice (e.g. Visual Studio Code) and find the ‘empty’ cell on the website - `<div></div>`

This cell will show the status of the Fan Control (automatic or manual). It will also allow the user to turn the fan on and off (when in manual control), and also change the control mode.

Replace `<div></div>`  with the code to allow these inputs and output.

![[Screen_Shot_2022-10-02_at_11.36.40_pm.png|Screen Shot 2022-10-02 at 11.36.40 pm.png]]

```html
<div>
    <p>Fan Control</p>
    <p>%FANCONTROL%</p>
    <p><a href="/FanManualOn">Turn Fan On</a></p>
    <p><a href="/FanManualOff">Turn Fan Off</a></p>
    <p><a href="/FanControl">Fan Automatic/Manual</a></p>
</div>
```

Save the file.

Upload these changes to the SPIFFS partition by selecting Tools → ESP32 Sketch Data Upload. 

### websiteFunctionality Changes

Finally, open the `websiteFunctionality` tab in Arduino. 

Three new routes will need to be added, as indicated in the HTML changes - `/FanManualOn`, `/FanManualOff` and  `/FanControl`. 

The routes to turn the fan on and off in manual mode are very similar to the safe lock and unlock routes. They perform a number of functions:

1. Check for authentication
2. Set the value of the variable appropriately
3. Log the event and 
4. Sends `dashboard.html` back to the browser.

![[Screen_Shot_2022-10-02_at_11.54.34_pm.png|Screen Shot 2022-10-02 at 11.54.34 pm.png]]

```arduino
server.on("/FanManualOn",  HTTP_GET, [[AsyncWebServerRequest * request]] {
  if (!request->authenticate(http_username, http_password))
    return request->requestAuthentication();
  fanEnabled = true;
  logEvent("Fan Manual Control: On");
  request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
});

server.on("/FanManualOff",  HTTP_GET, [[AsyncWebServerRequest * request]] {
  if (!request->authenticate(http_username, http_password))
    return request->requestAuthentication();
  fanEnabled = false;
  logEvent("Fan Manual Control: Off");
  request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
});
```

The final route to add is `/FanControl`.  This route is similar to the Manual control routes, however it sets the value of the `automaticFanControl` variable instead of `fanEnabled`.

The value is set by taking the value of `automaticFanControl` and ‘flips’ it by using the not operator - `!`. 

This means that if `automaticFanControl` is set to `true`, the value becomes `false` and is stored back in `automaticFanControl.`

![[Screen_Shot_2022-10-02_at_11.56.13_pm.png|Screen Shot 2022-10-02 at 11.56.13 pm.png]]

```arduino
server.on("/FanControl",  HTTP_GET, [[AsyncWebServerRequest * request]] {
  if (!request->authenticate(http_username, http_password))
    return request->requestAuthentication();
  automaticFanControl = !automaticFanControl;
  logEvent("Fan Manual Control: On");
  request->send(SPIFFS, "/dashboard.html", "text/html", false, processor);
});
```

As the HTML page was coded to show the Fan mode (automatic or manual), `processor()` needs to be updated to account for this. 

Add a new if statement to check for `FANCONTROL` and return Automatic or Manual as appropriate.

This is similar to how the safe status is outputted.

![[Screen_Shot_2022-10-02_at_11.59.42_pm.png|Screen Shot 2022-10-02 at 11.59.42 pm.png]]

```arduino
if (var == "FANCONTROL") {
  if (automaticFanControl) {
    return "Automatic";
  } else {
    return "Manual";
  }
}
```

Save and upload the code to the Adafruit Feather board. The website will now allow you to set the fan to automatic or manual mode, and turn the fan on and off.

![[fanAutomaticVsManual.gif|2022-10-03 00-01-29.2022-10-03 00_02_27.gif]]

Continue developing the site in similar methods to above - outputting sensor data, and affecting the hardware components.

When affecting the hardware components, ensure that you’re configuring the code in a similar way to `safeLocked` and `LEDOn`, so that the web and the hardware codes aren’t overriding and interfering with each other.

# Logging

Your project code should log events that occur throughout the life of the system. These events should include when the system turns on and off, the IP address that is assigned to the device, when fans are turned on and off etc.

**Update** the code to log all of these events.

You may need to update the main sketch as we as `websiteFunctionality`. 

Make the data useful and identity changes in the system. An example shown.

![[Screen_Shot_2022-10-09_at_9.53.15_pm.png|Screen Shot 2022-10-09 at 9.53.15 pm.png]]

## Why Log?

> Logging is an important data source for troubleshooting issues, business intelligence, and meeting compliance. Logs give records of precisely what your application is doing when.
[https://devcenter.heroku.com/articles/writing-best-practices-for-application-logs](https://devcenter.heroku.com/articles/writing-best-practices-for-application-logs)
> 

Any system you develop should log events and actions. This can be as simple as identifying the time the system turns on and turns off.

Importantly, logging can be used to identify errors and security issues (such as authentication attempts).

Logging can be done in may ways - this application uses the `logEvent()` function.

## logEvent() Analysis

Currently, `logEvent()` performs the processes to log events into the SPIFFS partition.

As a general overview - this function takes data - `dataToLog` - and the current date and time and appends that to the end of the current log, named `logEvents.csv`. 

![[Screen_Shot_2022-10-09_at_9.26.46_pm.png|Screen Shot 2022-10-09 at 9.26.46 pm.png]]

The file is formatted as a Comma Separated Values (CSV) file. This format is a text-based file format separating data by commas. An example of a logEvents function can be seen here.

```
2022,10,09,20,35,01,seesaw started
2022,10,09,20,35,24,route: /
2022,10,09,20,35,29,Dashboard
2022,10,09,20,35,32,Log Event Download
2022,10,09,20,36,34,Safe Unlocked via Website
2022,10,09,20,36,40,Fan Manual Control: On
2022,10,09,20,36,47,Fan Manual Control: Off
2022,10,09,20,36,48,Fan Manual Control: On
2022,10,09,20,36,49,Fan Manual Control: On
2022,10,09,20,36,51,Fan Manual Control: Off
2022,10,09,20,36,52,Fan Manual Control: On
2022,10,09,20,36,56,Log Event Download
```

The data logged is formatted in the following way - `YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, DATA`.


> [!info] CSV files are useful to store data in as they can be easily opened in a spreadsheet application.
> ![[Screen_Shot_2022-10-09_at_9.35.51_pm.png]]


## What to Log?

The exact events and data to log depends on the system requirements. But as a general rule, loggable events should include:

1. Authentication success and failures
2. Authorisation (access control) failures
    1. For instance if a regular user attempted to access administrator functions
3. Errors

All events should be time-stamped. Ideally with the correct time using a Real Time Clock (RTC) or other system.

The more information that is logged, the better for diagnosing issues. You could, for instance, log the user who performed the action. Additionally, you could log the client IP address where the request originated from. **These would require changes to `logEvent()`.** 

## Logging Cheat Sheet

This extensive Logging Cheat Sheet is maintained by [Open Web Application Security Project®
 (OWASP)](https://owasp.org/)

[Logging Cheat Sheet](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Logging_Cheat_Sheet.md#event-attributes)

# Adafruit MiniTFT Updates

## tftDrawText()

Currently, to display text on the Adafruit MiniTFT, the code is similar to the code shown.

As a high overview:

- The screen is completely filled with black.
    - The text passed to the function is displayed in white in the top lefthand corner of the window.

![[Screen_Shot_2022-10-09_at_10.06.21_pm.png|Screen Shot 2022-10-09 at 10.06.21 pm.png]]

This works, however each time the function is run, the existing data is cleared off screen. This is needed for *some* of the data, but not all. It would be better to only replace the data that needs to be changed.

Importantly in solving this problem, the `setTextColor()` function has another option, giving the text colour and the background colour.

The function can be updated to use this functionality.

![[Screen_Shot_2022-10-09_at_10.09.37_pm.png|Screen Shot 2022-10-09 at 10.09.37 pm.png]]

```arduino
void tftDrawText(String text, uint16_t color) {
  tft.setCursor(0, 0);
  tft.setTextSize(3);
  tft.setTextColor(color, ST77XX_BLACK);
  tft.setTextWrap(true);
  tft.print(text);
}
```

Update any and all function calls to `tftDrawText()` to include the new colour argument.

![[Screen_Shot_2022-10-09_at_10.11.47_pm.png|Screen Shot 2022-10-09 at 10.11.47 pm.png]]

## Display IP Address

Now that the TFT is not cleared regularly, the IP Address can be displayed once the system connects to the network.

![[Screen_Shot_2022-10-09_at_10.15.57_pm.png|Screen Shot 2022-10-09 at 10.15.57 pm.png]]

```arduino
String ip = WiFi.localIP().toString();

// Display IP on TFT
tft.setCursor(0, 60);
tft.setTextSize(2);
tft.setTextColor(ST77XX_WHITE, ST77XX_BLACK);
tft.setTextWrap(true);
tft.print(ip);
```

The IP Address is being displayed at coordinates `0,60`.  This is at the bottom of the TFT display, as can be seen here.

![[MiniTFT_Coordinates.png|MiniTFT Coordinates.png]]

## Feedback

One of the updates that can be easily implemented is feedback to the user when the user is connecting to the router. This can be done with the updated function.

![[Screen_Shot_2022-10-09_at_10.26.14_pm.png|Screen Shot 2022-10-09 at 10.26.14 pm.png]]

```arduino
tftDrawText("Connecting to WiFi..", ST77XX_RED);
```

# User eXperience Improvements

There are a number of improvements that can be made, focusing on the user experience (UX) rather than feature upgrades.

## Website Improvements

### Update `onNotFound()`

One of the biggest issues with including images in the website is that currently, the web server needs to be coded to respond to each individual specific route. For instance `/index.html` and `/dashboard.html` etc. These need to be done because specific routes may need to perform specific actions. 

However loading and displaying images are pretty standard and it would be frustrating to code a new route for each image that needs to be displayed on the website. Luckily, there’s an easier solution.

The function that handles HTTP 404 errors (file not found) can be modified to check to see if the file that’s not found is a JPG and then load that specific file.

Open `websiteFunctionality` and replace the server.onNotFound() function with this one.

The function still runs if the route is not found (specified in other routes). 

Before sending the `404.html` file it checks to see if the route attempted ends with `.jpg`. If so, it will attempt to load that file from the SPIFFS partition and send that to the brows

![[Screen_Shot_2022-10-24_at_10.20.23_pm.png|Screen Shot 2022-10-24 at 10.20.23 pm.png]]

```arduino
server.onNotFound([[AsyncWebServerRequest * request]] {
    if (request->url().endsWith(F(".jpg"))) {
      // Extract the filename that was attempted
      int fnsstart = request->url().lastIndexOf('/');
      String fn = request->url().substring(fnsstart);
      // Load the image from SPIFFS and send to the browser.
      request->send(SPIFFS, fn, "image/jpeg", true);
    } else {
      request->send(SPIFFS, "/404.html");
    }
  });
```

### Homepage image

Many of the website User Interface improvements can be done in the HTML and CSS files in the sketch data folder.

Download the image shown and store it in the images folder.

![[LakeBurleyGriffin.jpg]]

LakeBurleyGriffin.jpg

Open the data folder of the sketch. In Arduino, select Sketch → Show Sketch Folder and open the data folder. 

Save the image into the data folder.

![[Screen_Shot_2022-10-24_at_10.02.17_pm.png|Screen Shot 2022-10-24 at 10.02.17 pm.png]]

Open `index.html` in your editor of choice, such as Visual Studio Code and add the following code to display the image.

> [!info] Remove the “TODO” line of code as an image has been added.


![[Screen_Shot_2022-10-24_at_10.02.44_pm.png|Screen Shot 2022-10-24 at 10.02.44 pm.png]]

```html
<img src="LakeBurleyGriffin.jpg">
```

More images can be added in the same manner throughout the website.

![[Screen_Shot_2022-10-24_at_10.32.39_pm.png|Screen Shot 2022-10-24 at 10.32.39 pm.png]]

### Colour Scheme

If you wish to modify the colours of the website, you can edit the `arduino.css` file in the data folder. Experiment with changing the colours, identified with the HEX code. 

> [!info]  Visual Studio code has a colour picker embedded to choose the colour you wish.


To find some complimentary colours, you can use sites that match colours to the style you wish.

[Create a Palette - Coolors](https://coolors.co/generate)

[Color Palettes for Designers and Artists - Color Hunt](https://colorhunt.co/)

[Color Designer - Simple Color Palette Generator](https://colordesigner.io/)

[Color Palette Generator - Create Beautiful Color Schemes](https://colors.muz.li/)

[](https://color.adobe.com/create/color-wheel)

Change the HEX code foe each colour to the desired one.

**CSS Selectors**

Changing these values effect different sections of the page:

![[Screen_Shot_2022-10-24_at_10.46.04_pm.png|Screen Shot 2022-10-24 at 10.46.04 pm.png]]

![[Screen_Shot_2022-10-24_at_10.57.37_pm.png|Screen Shot 2022-10-24 at 10.57.37 pm.png]]

The border of the cell layout is the colour selected, and the ‘fill’ colour is that colour at 80% transparency.

![[Screen_Shot_2022-10-24_at_10.46.11_pm.png|Screen Shot 2022-10-24 at 10.46.11 pm.png]]

![[Screen_Shot_2022-10-24_at_11.00.18_pm.png|Screen Shot 2022-10-24 at 11.00.18 pm.png]]

Changing this colour changes the cell ‘fill’ colour and transparency. 

0.8 is the transparency as a decimal value (80%).

![[Screen_Shot_2022-10-24_at_10.46.19_pm.png|Screen Shot 2022-10-24 at 10.46.19 pm.png]]

![[Screen_Shot_2022-10-24_at_11.01.01_pm.png|Screen Shot 2022-10-24 at 11.01.01 pm.png]]

This changes the colour ‘behind’ the cell content.

![[Screen_Shot_2022-10-24_at_10.46.22_pm.png|Screen Shot 2022-10-24 at 10.46.22 pm.png]]

![[Screen_Shot_2022-10-24_at_11.04.29_pm.png|Screen Shot 2022-10-24 at 11.04.29 pm.png]]

Changes the background colour of the Header Cell.

![[Screen_Shot_2022-10-24_at_10.46.27_pm.png|Screen Shot 2022-10-24 at 10.46.27 pm.png]]

![[Screen_Shot_2022-10-24_at_11.06.27_pm.png|Screen Shot 2022-10-24 at 11.06.27 pm.png]]

Sets the colour of the navbar background.

![[Screen_Shot_2022-10-24_at_10.46.34_pm.png|Screen Shot 2022-10-24 at 10.46.34 pm.png]]

![[Screen_Shot_2022-10-24_at_11.07.49_pm.png|Screen Shot 2022-10-24 at 11.07.49 pm.png]]

Changes the colour of the links in the navbar.

![[Screen_Shot_2022-10-24_at_10.46.40_pm.png|Screen Shot 2022-10-24 at 10.46.40 pm.png]]

![[navbarHighlight.gif|2022-10-24 23-09-42.2022-10-24 23_10_13.gif]]

Changes the colour the navbar entry changes to when the mouse hovers over it.

![[Screen_Shot_2022-10-24_at_10.46.43_pm.png|Screen Shot 2022-10-24 at 10.46.43 pm.png]]

![[Screen_Shot_2022-10-24_at_11.11.00_pm.png|Screen Shot 2022-10-24 at 11.11.00 pm.png]]

Sets the colour of the current page in the navbar.

# Administration functions

## Administrator Page

To enable the administrator functionality, a number of areas need to be updated. Initially, separate usernames and passwords are required for guests and administrators.

Open `sensitiveInformation.h` and replace the following code block:

```arduino
const char* http_username   = "admin";
const char* http_password   = "admin";
```

with this one:

```arduino
const char* usernameGuest   = "guest";
const char* passwordGuest   = "123";

const char* usernameAdmin  = "root";
const char* passwordAdmin   = "password";
```

> [!info] The usernames and passwords can be different to the ones shown.


Save the file.

Open `websiteFunctionality` and Find and Replace all instances of `http_username` with `usernameGuest`. Repeat the process for replacing `http_password` with  `passwordGuest`.

And example of a route once this has been done will be similar to this, with only the authentication if statement changed.

![[Screen_Shot_2022-10-25_at_12.08.56_am.png|Screen Shot 2022-10-25 at 12.08.56 am.png]]

Add a new route for the admin.html.

This route is very similar to the dashboard.html route, however changing a few parts. 

First the authentication requires usernameAdmin and passwordAdmin to continue loading the route.

Secondly, if the authentication fails, then this is logged for security purposes.

![[Screen_Shot_2022-10-25_at_12.18.10_am.png|Screen Shot 2022-10-25 at 12.18.10 am.png]]

```arduino
server.on("/admin.html", HTTP_GET, [[AsyncWebServerRequest * request]] {
    if (!request->authenticate(usernameAdmin, passwordAdmin)){
      logEvent("Administrator Access Attempt failed");
      return request->requestAuthentication();
    }
    logEvent("Admin access");
    request->send(SPIFFS, "/admin.html", "text/html", false, processor);
  });
```

To create the `admin.html` page, initially just duplicate (or copy and paste) `dashboard.html` and rename the new file `admin.html`.  Edit the file so that the heading and title is Administration instead of Guest.

> [!info] The navbar will need to be updated on all HTML pages to reflect the addition of the Admin page.


![[Screen_Shot_2022-10-25_at_12.21.01_am.png|Screen Shot 2022-10-25 at 12.21.01 am.png]]

## Safe Unlock - Administrator

The Admin page *could* use the already existing Safe unlocking and locking routes and code, however currently if the user accesses `/SafeLock` this will unlock the safe, but redirect the user to `dashboard.html` and also log that the safe was unlocked, but doesn’t indicate that it was unlocked by the administrator. So one solution would be to duplicate the `/SafeLock` and `/SafeUnlock` routes and modify them slightly.

Duplicate the two routes - `/SafeLock` and `/SafeUnlock` - and modify the route names to include “Admin” and to log appropriately, and then to redirect them back to `admin.html`. The appropriate username and password will need to be updated as well.

![[Screen_Shot_2022-10-31_at_12.56.48_pm.png|Screen Shot 2022-10-31 at 12.56.48 pm.png]]

Admin.html will need to be updated to use these new routes.

```html
<div>
  <p>%SAFESTATUS%</p>
  <p><a href="/SafeLockAdmin">Lock Safe</a></p>
  <p><a href="/SafeUnlockAdmin">Unlock Safe</a></p>
</div>
```

## Setting the Temperature Threshold

One feature that extends the current functionality is allowing the administrator to remotely set the value where the automatic fan will turn on or off. Currently, the value is fixed in code, which is not very flexible or user friendly. 

To enable this functionality, there are some changes needed to various parts of the code:

1. The Webpage needs to allow the submission of a number via a HTML form.
2. The Web*server* needs to accept and read that value.
3. The value read by the web server is then stored in a global variable.
4. The Automatic Fan process then uses that global variable to calculate if the fan should be on or off.

First, modify the main sketch to include the global variable to store the threshold value.

![[Screen_Shot_2022-10-31_at_1.50.04_pm.png|Screen Shot 2022-10-31 at 1.50.04 pm.png]]

```arduino
float fanTemperatureThreshold = 25.0;
```

In `fanControl()` edit the function call to `automaticFan(25.0)` to instead use the new global variable as the argument.

![[Screen_Shot_2022-10-31_at_1.51.51_pm.png|Screen Shot 2022-10-31 at 1.51.51 pm.png]]

```arduino
automaticFan(fanTemperatureThreshold);
```

Save the file.

Edit `admin.html` and add or modify a ‘cell’ to include the HTML form to prompt the user to set the Threshold Value.

This will display a simple form where the administrator can choose a number value and press submit. The attribute - `value="%CURRENTTHRESHOLD%"` will be set later to pre-populate the form with the current threshold value.

Save the file.

[https://www.notion.so](https://www.notion.so)

```html
<div>
    <p>Set Temperature Threshold</p>
    <form action="/setTemperatureThreshold">
        Temperature Threshold <br>
        <input type="number" name="tempThreshold" value="%CURRENTTHRESHOLD%">
        <input type="submit" value="Submit">
    </form><br>
</div>
```

> [!info] At this stage, the updated HTML file/s can be updated on the Feather board by choosing Tools→ESP32 Sketch Data Upload


Open `websiteFunctionality` and add a new global constant to define the website parameter the code will look for and respond to.

![[Screen_Shot_2022-10-31_at_1.10.03_pm.png|Screen Shot 2022-10-31 at 1.10.03 pm.png]]

```arduino
const char* PARAM_INPUT_1 = "tempThreshold";
```

Create a new route called `/setTemperatureThreshold` which will process the value entered on the form and then sets `fanTemperatureThreshold` to be that value.

![[Screen_Shot_2022-10-31_at_1.58.54_pm.png|Screen Shot 2022-10-31 at 1.58.54 pm.png]]

```arduino
server.on("/setTemperatureThreshold", HTTP_GET,  [[AsyncWebServerRequest * request]] {
  int newThreshold; 
  if (request->hasParam(PARAM_INPUT_1)) {
    fanTemperatureThreshold = request->getParam(PARAM_INPUT_1)->value().toFloat();
    String logMessage = "Administrator set Automatic Fan Theshold to" + String(fanTemperatureThreshold);
    logEvent(logMessage);
  }
  request->send(SPIFFS, "/admin.html", "text/html", false, processor);
});
```

---

**How does all this work?**

When the form on the website is submitted, this generates a URL in the following format:

`<IP>/setTemperatureThreshold?tempThreshold=25`

The new route created will check if the URL has a parameter (after the ?). If that parameter matches `tempThreshold`, then it will perform the necessary tasks.  

Inside the IF statement (if the tempThreshold parameter is in the URL), the code will then get the value of the parameter `getParam(PARAM_INPUT_1)->value()` - which in the example shown is 25 - and store it in the `fanTemperatureThreshold` global variable.

---

Modify `processor()` to detect `CURRENTTHRESHOLD` and return with the variable (to be set later).

![[Screen_Shot_2022-10-31_at_1.48.21_pm.png|Screen Shot 2022-10-31 at 1.48.21 pm.png]]

```arduino
if (var == "CURRENTTHRESHOLD") {
  return String(fanTemperatureThreshold);
}
```

Save all files. Compile and upload the Sketch and HTML files. And try to set low and high values in the form.