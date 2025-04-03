

## Video Demonstration


> [!info] The exact process and code you’ll need to implement will depend on your specific requirements.


![https://youtu.be/rPDmzrNgSn4](https://youtu.be/rPDmzrNgSn4)

## Step 1: Define function

Create a new function for the new functionality. 

In this example, the status of a button will be uploaded to the server, and the response will determine whether the fan will be turned on or not.

Decide the input sensor and the output actuator. 

![[newFunctionalityButtonFan.png]]

## Step 2: Libraries

Import the necessary libraries for your sensor and actuator. This will require researching the sensor and module you are attempting to use. 

Import the library/ies required using the Library option in the Platform IO home.

![[newFunctionalityAddLibraries.png]]

### Alternatively…

Open `platformio.ini` and add the following libraries to the end of the list.

![[newFunctionalityLibrariesAddManually.png]]

```cpp

adafruit/Adafruit ST7735 and ST7789 Library@^1.10.3
adafruit/Adafruit seesaw Library@^1.7.3
adafruit/Adafruit GFX Library@^1.11.7
adafruit/Adafruit SSD1306@^2.5.7
adafruit/Adafruit Motor Shield V2 Library@^1.1.1
```

## Step 3: Collect Input

## Step 4: Send Data to the Server and Receive Response

Reuse (copy) the code from `temperatureAndLED()` to upload the sensor data to the server and receive a response.

## Step 5: Affect Actuator

## Step 6: Test and test again

# Add new Local Functionality

Repeat the process above, however skip the step to upload the sensor data to the server and wait for a response.