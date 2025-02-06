---
tag: Robotics
---
# Infrared Receiver

# Wiring

To get these modules working, you'll need to first wire them - they're pretty simple, VCC and GND as normal, and the output (or signal) pin can connect to any digital GPIO pin. In the example below, this is wired to pin 7.

![[componentIRWiring.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/infrared_bb.png]]

# Code

## Library Install

Using the Library Manager, search for IRRemote and install the library indicated here.

![[componentIRLibraryInstall.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_9.59.35_pm.png]]

## Test Module

In the examples menu (under File), find the library and load the "Minimal Receiver" demo code.

![[componentIROpenCode.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.01.15_pm.png]]

Change the Pin to pin 7 on the indicated line of code.

![[componentIRSetPin.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.02.24_pm.png]]

Run the code and then you should be able to see some results through the Serial Monitor. The code that appears next to the C= output indicates the Hex code that is sent by the remote when individual buttons are pressed.

![[componentIRReceiveCodes.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.03.50_pm.png]]

# Integration

Now that the code works as a sample, it's time to integrate the code into your larger code base.

## Pin definitions and library import

The important lines of code you will need are the pin definitions and the library import:

![[componentIRDefineLibrary.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.07.11_pm.png]]

```arduino
#define IR_INPUT_PIN    7
#include "TinyIRReceiver.cpp.h"
#define STR_HELPER(x) #x
#define STR(x) STR_HELPER(x)
```

Note the order of the lines of code. If included out of order, there may be errors.

## Setup()

Include the following lines of code into your setup() function to configure the function which will run whenever the Infrared module receives a signal.

![[componentIRInterrupt.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.10.03_pm.png]]

```arduino
// IR Configuration
initPCIInterruptForTinyReceiver();
Serial.println(F("Ready to receive NEC IR signals at pin " STR(IR_INPUT_PIN)));
```

Finally, you'll need to include the function which will run when the infrared module receives a signal. The sample code simple outputs the signal data, but you will want to change that functionality to whatever you wish. 

The important variable that you will need to access is `aCommand`, which indicates which button is being pressed. For instance, this code prints the text version of the button based on the command. 

> [!info]  This code works for a specific remote. Your remote may use different codes, so you'll need to map the codes to buttons using the Minimal Receiver code above.


![[componentIRHumanReadable.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.18.26_pm.png]]

![[componentIROutput.png|Infrared%20Receiver%20207b66f6d390402b9b18f1b587991646/Screen_Shot_2021-05-16_at_10.18.09_pm.png]]

```arduino
void handleReceivedTinyIRData(uint16_t aAddress, uint8_t aCommand, bool isRepeat) {
  if (aCommand == 70) {
    Serial.println("Up");
  }
  if (aCommand == 21) {
    Serial.println("Down");
  }
  if (aCommand == 68) {
    Serial.println("Left");
  }
  if (aCommand == 67) {
    Serial.println("Right");
  }
}
```