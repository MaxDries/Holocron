In this stage, you’ll develop the overall logic required for the smart device, without the specific code implementation.  This stage focuses on the sequence of steps in the loop() function and ‘method stubs’. 

Stubs are functions that in effect “empty” that will be developed at a later stage.

[Method stub - Wikipedia](https://en.wikipedia.org/wiki/Method_stub)

In essence, the goal is to write the outline of the functionality of the code. The functions should have no implementation in them *at this stage*.

---

## Custom Functions - Stubs

So, you create the code which defines the function name, any parameters and any return type, but leave the implementation blank.


> [!info] [[Naming Convention]]! Your functions should describe the purpose of the function.


Using the flowcharts you created previously, create the functions in Arduino. At the current state, leave the implementation blank. 

This example shows the type of code you should have after this process.

```arduino
void readSensor() {
	
}

boolean isObjectTooClose(int distance) {

  return true;
}
```

At this stage, add the function comments.

Function comments are to appear before the function declaration, and have three sections:

1. A brief description of the goal of the function.
2. A description of any parameters. `None` if no parameters
3. A description of any return type. `Null` if none.

```arduino
/**
  Indicates whether the object is too close to the sensor.

  @param distance value read from the distance sensor
  @return true if object is too close, false otherwise.
*/
boolean isObjectTooClose(int distance) {

  return true;
}
```

## Example of final product

The goal is to have your code (without the initialisation and `setup()` function) looking something similar to this at the end of the process:

```arduino
void loop() {
  bluetoothConnectivity();
  motorDC();
  doorAlarm();
}

/*
  Reads distance value from Sonar (HC-SR04). 
  If less than threshold, activate Piezo buzzer
  @param None
  @return void
*/
void doorAlarm() {

}

/*
  Test Code for DC Motor Usage
  @param None
  @return void
*/
void motorDC() {

}

/*
  Print some information on the motor state in Serial Monitor
  @param None
  @return void
*/
void printSomeInfo()
{

}

/*
    Takes command entered from Bluetooth connection and executes functionality
    E.g. "1" - Write HIGH to builtin LED
    "0" - Write LOW to builtin LED
    @param bleCommand - string accepted from BLE Uart Connection
    @return void
*/
void bluetoothCommandReceived(String bleCommand) {

}

/*
    Handles BLE connectivity.
    Taken from library functionality.
    @param None
    @return void
*/
void bluetoothConnectivity() {

}

/*
    Log entries to a file on an SD card, and outputs to Serial Monitor
    @param dataToLog - string to save on SD card, timestamped.
    @return void
*/
void logEvent(String dataToLog) {

}
```