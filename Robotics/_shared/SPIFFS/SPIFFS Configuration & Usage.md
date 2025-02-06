---
tags:
  - Robotics/esp32
tag: Robotics
---

# What is SPIFFS?

> SPIFFS is a lightweight filesystem created for microcontrollers with a flash chip, which are connected by SPI bus, like the ESP32 flash memory.
[https://randomnerdtutorials.com/esp32-web-server-spiffs-spi-flash-file-system/](https://randomnerdtutorials.com/esp32-web-server-spiffs-spi-flash-file-system/)
> 

In essence, it means that the ESP32 chip (which is at the core of the Huzzah32 Board) has the ability to store files in flash memory, removing the requirement for files to be stored elsewhere (such as a SD card)

> SPIFFS lets you access the flash memory like you would do in a normal filesystem in your computer, but simpler and more limited. You can read, write, close, and delete files. At the time of writing this post, SPIFFS doesn’t support directories, so everything is saved on a flat structure.
> 
> 
> Using SPIFFS with the [ESP32 board](https://makeradvisor.com/tools/esp32-dev-board-wi-fi-bluetooth/) is specially useful to:
> 
> - Create configuration files with settings;
> - Save data permanently;
> - Create files to save small amounts of data instead of using a microSD card;
> - [Save HTML and CSS files to build a web server](https://randomnerdtutorials.com/esp32-web-server-spiffs-spi-flash-file-system/);
> - [Save images, figures and icons](https://randomnerdtutorials.com/display-images-esp32-esp8266-web-server/);
> - And much more.

![[spiffsOverview.webp]]

[https://randomnerdtutorials.com/install-esp32-filesystem-uploader-arduino-ide/](https://randomnerdtutorials.com/install-esp32-filesystem-uploader-arduino-ide/)

This tutorial will step you through how to configure the Arduino IDE to access and use SPIFFS to store and use files in Arduino Sketches.

## SPIFFS directory structure

After this process, you'll be able to upload HTML, or any other files, to the ESP32/Huzzah32 feather board. You'll create a data folder, and the process will upload anything in that folder to the ESP32 chip.

![[spiffsDirectoryStructure.png]]

# SPIFFS Libraries Install

Before you can program your ESP32 board to access the SPIFFS partition, you'll need to download and install the relevant libraries.

### File Downloads

Download the following files. 

[AsyncTCP-master.zip](https://drive.google.com/file/d/17-fgPvk_d9EYKiq8kfjL-w_vMBtDgYoM/view?usp=drivesdk)

[ESPAsyncWebServer-master.zip](https://drive.google.com/file/d/16vgWIVXm1HqTN39VjQZNXVSwqpQxnw8T/view?usp=drivesdk)

### Manual Install

Open Arduino IDE. Under *Sketch*, choose *Include Library* and then *Add ZIP Library*.

Choose the ZIP file you just downloaded - e.g.`ESPAsyncWebServer-master.zip`. 

**Repeat the process for all required libraries.**

You may receive an ‘error’ like this one to say the library already exists - that’s fine.

![[spiffsNewSketch.png]]

![[spiffsAddLibrary.png]]

# Arduino ESP32 filesystem uploader Plugin

In order to upload files to the SPIFFS partition from your computer, you will need to install the SPIFFS Plugin.

This plugin will allow you to upload files to the SPIFFs partition. This will allow you, among other things, to upload the HTML files required for this project.

### Downloads

Download this file.

[ESP32FS-1.0.zip](https://drive.google.com/file/d/17-q0cBOPsvVjNhZsH1WKRLwj3aOwDk_I/view?usp=drivesdk)

### Plugin Install

Extract the files and copy the `ESPFS` directory into **Tools** folder of your **sketchbook** location. 


💡 You find your **sketchbook** location in the Preferences window.

![[spiffsPreferences.png]]


Make sure just the `ESP32FS` folder is copied into the Tools folder, as can be seen here.

![[spiffsPluginDirectory.png]]

### Restart the IDE

Quit and restart the IDE, and you should notice that under the Tools menu, there is now an option - `ESP Sketch Data Upload`

> [!info] You don't need to click it yet - just make sure it's there.


> [!info]  If it doesn't appear, check the Tools folder that you've installed the plugin into is correct.


![[spiffsMenuOption.png]]

# Arduino Sketch

There are a number of parts of the code to update to include both Wifi and SPIFFS in the sketch.

## Wifi & Webserver code

### Includes & Global Variable Definitions

At the start of the sketch, where libraries are included, and variables defined, ensure that the Wifi block appears as:

> [!info] If the following lines of code appear, delete them!
> ```arduino
> String loginIndex, serverIndex;
> WebServer server(80);
> ```


```arduino
#define FORMAT_SPIFFS_IF_FAILED true

// Wifi & Webserver
#include "WiFi.h"
#include "SPIFFS.h"
#include <ESPAsyncWebServer.h>
#include "wifiConfig.h"
AsyncWebServer server(80);
```

### Updated `logEvent()` function.

The steps to store data into a file stored in the SPIFFS partition is different from other formats. The Code shown here.

> [!info] If the sketch already includes a `logEvent()` function, it should be replaced by this!


```arduino
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

### Updated `setup()`

Look in the setup() function for the code blocks which link the Webserver to individiual HTML files. They will look similar to the code shown below. These need to be replaced with code similar to this:

```arduino
server.on("/", HTTP_GET, [[AsyncWebServerRequest * request]] {
    Serial.println("index");
    request->send(SPIFFS, "/index.html", "text/html");
  });
  server.on("/dashboard", HTTP_GET, [[AsyncWebServerRequest * request]] {
    Serial.println("dashboard");
    request->send(SPIFFS, "/dashboard.html", "text/html");
  });
  server.on("/logOutput", HTTP_GET, [[AsyncWebServerRequest * request]] {
    Serial.println("output");
    request->send(SPIFFS, "/logEvents.csv", "text/html", true);
  });
```

This code is only an example, and will need to be modified if the pages required are different.
For instance, this code block only serve three URLs on the webserver - `/`, `/dashboard` and `/logOutput`.
Your code will need to match the HTML files you have in the data folder - explained further down the page.

You'll notice that each URL outputs information to the Serial Monitor (which is only used for debugging), and then `send`s a HTML or CSV file which sends that file, stored in the SPIFFS partition to the browser to display.

> [!info] Notice the additional parameter for the `logOutput`? The additional `true` sets the file to be downloaded, rather than displayed in the browser. 
> Test what happens if you change this to `false` and access the page.


### Remove SD Card functions

> [!info] This section should only be performed if necessary. 
> If unsure - It is recommended that the code be commented out rather than deleted. You can select the lines of code and press **CTRL + / (CMD + /** for mac**)**

**If the sketch** has any SD card libraries or code, this can be deleted as this function is being replaced by the SPIFFS partition.

This code can be removed. 

The SD card libraries are not needed as the SD card is not being used anymore.

The `readFile()` function is similar, but duplicated.

The `loadHTML()` function is only used to load particular files from the SD and stored in variables, which are not required anymore.

`setupSD()` is not required as the SD is not being used.

```arduino
// SD Card - Adalogger
#include "FS.h"
#include "SD.h"
#include "SPI.h"
```

```arduino
String readFile(fs::FS &fs, const char * path) {
  Serial.printf("Reading file: %s\n", path);
  char c;
  String tempHTML = "";

  File file = fs.open(path);
  if (!file) {
    Serial.print("Failed to open file for reading: ");
    Serial.println(path);
    return "";
  }

  while (file.available()) {
    c = file.read();
    tempHTML += c;
  }
  file.close();
  return tempHTML;
}

void loadHTML() {
  homepage = readFile(SD, "/index.html");
  dashboard = readFile(SD, "/dashboard.html");
  forgot = readFile(SD, "/forgot.html");
}
```

```arduino
void setupSD() {
  if (!SD.begin()) {
    Serial.println("Card Mount Failed");
    return;
  }

  uint8_t cardType = SD.cardType();

  if (cardType == CARD_NONE) {
    Serial.println("No SD card attached");
    return;
  }
  Serial.println("SD Started");
  //  delay(1000);
}
```

```arduino
// May be found in the loop() function.
server.handleClient();
```

### SPIFFS File Management Code

There are prewritten functions to manage standard file management - add, delete, append to etc. The code can be copied in as is:

```arduino
// SPIFFS file functions
void readFile(fs::FS &fs, const char * path) {
  Serial.printf("Reading file: %s\r\n", path);

  File file = fs.open(path);
  if (!file || file.isDirectory()) {
    Serial.println("- failed to open file for reading");
    return;
  }

  Serial.println("- read from file:");
  while (file.available()) {
    Serial.write(file.read());
  }
  file.close();
}

void writeFile(fs::FS &fs, const char * path, const char * message) {
  Serial.printf("Writing file: %s\r\n", path);

  File file = fs.open(path, FILE_WRITE);
  if (!file) {
    Serial.println("- failed to open file for writing");
    return;
  }
  if (file.print(message)) {
    Serial.println("- file written");
  } else {
    Serial.println("- write failed");
  }
  file.close();
}

void appendFile(fs::FS &fs, const char * path, const char * message) {
  Serial.printf("Appending to file: %s\r\n", path);

  File file = fs.open(path, FILE_APPEND);
  if (!file) {
    Serial.println("- failed to open file for appending");
    return;
  }
  if (file.print(message)) {
    Serial.println("- message appended");
  } else {
    Serial.println("- append failed");
  }
  file.close();
}

void renameFile(fs::FS &fs, const char * path1, const char * path2) {
  Serial.printf("Renaming file %s to %s\r\n", path1, path2);
  if (fs.rename(path1, path2)) {
    Serial.println("- file renamed");
  } else {
    Serial.println("- rename failed");
  }
}

void deleteFile(fs::FS &fs, const char * path) {
  Serial.printf("Deleting file: %s\r\n", path);
  if (fs.remove(path)) {
    Serial.println("- file deleted");
  } else {
    Serial.println("- delete failed");
  }
}
```

> [!info]  Try compiling the code at this stage and fix any errors as they appear.


# Uploading data

> [!bug] The uploader will overwrite anything you had already saved in the filesystem.


To upload files to the SPIFFS partition, you will need to create a data directory in the sketch directory. 

Open the sketch directory. Under Sketch choose `Show Sketch Folder`

![[spiffssUploadData.png]]

Create a directory called data. 

![[spiffsDataFolder.png]]

Copy files into the folder that you wish to upload. 

As an example, you could download the files found here: [https://github.com/LTC-IT/ISHP/tree/main/ISHP/data](https://github.com/LTC-IT/ISHP/tree/main/ISHP/data)

These HTML files have a index page, which includes 

- a login (username and password is `admin`),
- a forgot page, telling the user to check the HTML source of the login page to find the username and password
- And a dashboard page that is shown once the user successfully authenticates. This shows the structure for displaying arduino variables in HTML.

![[spiffsDataFolderFiles.png]]

Choose the upload tool from the Tools menu. 

> [!info] Your Serial Monitor needs to be closed for this to work.

![[spiffsDataUpload.png]]

# Upload sketch

Upload the sketch in the usual manner.

Open the serial monitor to get the IP address of your Huzzah32 Feather.

![[spiffsSerialMonitor.png]]

Open that IP in your browser or choice. For example: `http://192.168.1.116/`

![[spiffsWebsiteRunning.png]]

# How it works

When the browser attempts to access a URL on the server, in the example, it’s `http://192.168.1.116/`, the “page” it’s trying to access is `/`.

The sketch looks to find a match for this page, and it find the match in the block of code in the `setup()` function.

It matches the `/` in the URL to the `"/"` in the code shown and runs the code within the `{}`. In this case it will output to the Serial Monitor and send the `index.html` file from the SPIFFS partition to the browser to display.

If the browser attempts to access another resource on the server, it will always attempt to find a match in the setup function in the same manner.  

```arduino
server.on("/", HTTP_GET, [[AsyncWebServerRequest * request]] {
    Serial.println("index");
    request->send(SPIFFS, "/index.html", "text/html");
  });
```

# How to publish data to Website

An important function is to be able to display data from the Arduino board (in this case the Adafruit Huzzah32 Feather) in a webpage.

Start by considering this slightly updated section of the setup() function where the routes are defined.

The three routes - `/`, `/dashboard`, and `/logOutput` - have the line of code `request→send`(....), which show three different options.

![[spiffsRouting.png]]

The arguments of each of the `request→send` commands are important. 

The first one (highlighted in orange, is the File System type, which in this case is SPIFFS partition

The second (highlighted green) is the file that is sent to the browser when that route is accessed. 

The third (highlighted blue) is the HTTP content type. The default/common one is text/html but there are many more. Some more common ones can be [found here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types).

The forth (highlighted pink) is whether the file sent is to be downloaded or not. The default is false, meaning the file is to be displayed in the browser.. The `/logOutput` has this set to true, which means that when the user attempts to access that route, the file will attempt to download and save the file locally.

The last (highlighted purple) is the function that is run to process the HTML file before sending the HTML to the browser.

## Processing the HTML

The HTML file can be created to include *placeholder* text which will be replaced through the `processor()` function.

The placeholder text, which can be thought of similar to a variable, is written with % before and after. For instance - `%DATETIME%`. In this case DATETIME is referred to as a placeholder.

An example of this in HTML could be.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
Date and Time: %DATETIME%
</body>
</html>
```

Using a processor function, in this case cleverly named `processor()`, the HTML can be modified to allow for data to be retrieved from the Arduino code, and "sent" to the browser, as can be seen in this code.

In this case the `datetime` variable is created using the two functions `getTimeAsString()` and `getDateAsString()` but the value that is returned could be anything. Whatever content is returned will be replaced in the placeholder text that is being processed.

> [!info] If you're just testing, you can replace the if statement to 
> ```
> if (var == "DATETIME") {
 >   return "Today";
  > }
>  ```
> which will test and demonstrate the process of replacing placeholders with data from Arduino sketches.


```arduino
String processor(const String& var) {
  if (var == "DATETIME") {
    String datetime = getTimeAsString() + " " + getDateAsString();
    return datetime;
  }
  return String();
}
```

[[SPIFFS Configuration & Usage|Upload the HTML file to the board]], and upload the sketch in the usual fashion. After getting the IP address, visit the dashboard page, and the page should have replaced the `%DATETIME%` placeholder with the time and date. 

![[spiffsDateTime.png]]

## Multiple placeholders?

The processor function can be used multiple times, for multiple placeholders! 

In this example, it will replace `DATETIME`, `MOISTURE`, `TEMPINC` and `PUMPSTATE` placeholders with the values that are returned.

```arduino
String processor(const String& var) {
  Serial.println(var);
  
  if (var == "DATETIME") {
    String datetime = getTimeAsString() + " " + getDateAsString();
    return datetime;
  }
  if (var == "MOISTURE") {
    readSoil();
    return String(moistureValue);
  }
  if (var == "TEMPINC") {
    return String(tempsensor.readTempC());
  }
  if (var == "PUMPSTATE") {
    if (pumpIsRunning) {
      return "ON";
    } else {
      return "OFF";
    }
  }
  return String();
}
```