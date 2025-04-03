---
tag: Robotics
---
# Theory
![[Theory#Algorithm Design - Modularisation]]


# Arduino Implementation

💡 `setup()` and `loop()` are functions that are required by the Arduino language.


With Arduino, you will invariably be using some of the inbuilt functions as part of the language itself. For instance - `pinMode()`, `Serial.println()`, `digitalWrite()` etc. When you see brackets - () you are usually dealing with a function, or a condition.

## Custom Written Functions

Organising your code with functions is an important skill to have when coding in any language.

In Arduino, writing your own function requires the specific syntax of a C-based language. You can see in this example that the function declaration starts with the defining the return type, then the function name, and then any parameters.

> [!info] If your function doesn’t return any data, the first keyword is void. Otherwise, you **must** declare the data type of the variable that will be returned.


The curly brackets - `{}` - determine the start and the end of the function code block, which is run when the function is called.  

### Returning Data

`return` is the keyword to end the function and “send” data back to where the function was called from.

[https://docs.arduino.cc/learn/programming/functions](https://docs.arduino.cc/learn/programming/functions)

### Returning Data Example 1

Here is code to multiply two numbers together and return the result:

```arduino
int myMultiplyFunction(int x, int y){
  int result;
  result = x * y;
  return result;
}
```

The function is called such as `myMultiplyFunction(10, 5);` but the result will need to be used once the function has been completed. You could store the result in a variable, or use it as part of another function call, such as printing it to the serial monitor. For example:

```arduino
void setup() {
  // This block....
  int answer = myMultiplyFunction(10, 5);
  Serial.println(answer);
  
  // is functionally the same as this one
  Serial.println(myMultiplyFunction(10,5));
}

void loop() {
  // Nothing...
}

int myMultiplyFunction(int x, int y) {
  int result;
  result = x * y;
  return result;
}
```

![https://youtu.be/JPRBzRKqJ0I](https://youtu.be/JPRBzRKqJ0I)

### Returning Data Example 2

It is possible, and often highly encouraged to have multiple return statements in a function. 

Let’s say that you want a function to check to see if a sensor is reading a value over a set amount. This could be if a potentiometer is reading a value over 500. If the condition is correct (i.e. the value is over 100), the function will return `true`, otherwise it will return `false`. The function would be written as:

```arduino
void loop()
{
  isValid = validPotentiometerReading();
  if (isValid) {
	Serial.println("The sensor value is valid");
  } else {
	Serial.println("The sensor value is NOT valid");
  }
}

boolean validPotentiometerReading() {
  if (analogRead(A0) > 500) {
	return true;
  } else {
	return false;
  }
}
```

> [!info]  The first return function executed will immediately end the function. I.e. only one return function will be executed for each function call.


![[modularisationPotReadDemo.gif]]

# Practical Exercises


> [!info] **Goal**
> 
> In this task, you’ll be programming an Arduino Uno to do the following:
>
> 1. Set the brightness of the LED to be the value read by a variable input. In essence, you’re building a dimmable light switch.
>
> This task will involve writing and interfacing with two functions, the first to read the value set by the potentiometer, and the second to set the brightness of the LED.


Log into [Tinkercad](https://www.tinkercad.com/) and access the classroom used previously. Open the activity `Bank Alarm`.

> [!info]  Remember to sign in with your school google account.


With the Activity open, Under Shared with You, choose Copy and Tinker.

![[modularisationCopyAndTinker.png]]

Open the Code tab to view the initial code.

Currently, the code:

- configures pin A0 to be INPUT
- configures pin 9 to put OUTPUT, and
- enables the Serial Monitor.

![[modularisationCodeStart.png]]

> [!info] **Explanation.**
> 
> With the predefined circuit there are two components attached to the Arduino Uno
> 
> 1. an LED on Pin 9 which is a simple light bulb, and
> 2. a Potentiometer on Pin A0, which provides the ability to read analog values that changes based on the position of the dial.
> 
> The LED is an OUTPUT device and the Potentiometer is an INPUT device, hence the code configuring those pins as such. 


![[modularisationLEDAndPot.png]]

## Reading the Potentiometer

After `loop()` create a new function to read the value from the potentiometer.

> [!info]  The function header is made up of a number of sections:
> 
> `int` indicates that this function will *return* a value as an **integer** (whole number).
> 
> `readPotentiometer` is the name given to the function. Functions can be named **almost** anything, but they must not start with a number, and have no spaces. It is  advisable to name functions descriptively to indicate what their purpose is (more on that later in the project).
> 
> `()` indicate what (if any) parameters are required by the function. As there are no parameters, the code requires just the open and close brackets.


![[modularisationCodeReadPotStart.png]]

```arduino
int readPotentiometer() {
  
}
```

Read the value of the potentiometer and store in a local variable. 

> [!info]  More on variables later in the project. Just think of the code temporarily storing the value to use later.


![[modularisationStorePotValue.png]]

```arduino
int potValue = analogRead(A0);
```

Before making the code more complex, it’s advisable to check at regular intervals to ensure that smaller sections of code are working. In this case, you can print the value to the serial monitor to make sure the Potentiometer is reading correctly, and so you can see the range of values that are produced.

Add code to output the value of the potentiometer, stored in `potValue` to the serial monitor.

![[modularisationCodeSerialPrintPot.png]]

```arduino
Serial.println(potValue);
```

The function is semi-complete, however it won’t execute the code contained within `readPotentiometer()` until you **call** the function, which is when you instruct the code when to execute (run) the code.

Call the function in from `loop()` and then add a short delay into the code.

![[modularisationCodeMainLoopPot.png]]

```arduino
int potReading = readPotentiometer();
delay(250);
```

Press **Start Simulation** and expand the Serial Monitor tab.

![[modularisationCodeSerialMonitorOutput.png]]

Change the position of the potentiometer and notice the output change.

> [!info] Note how the values range from 0 to 1023. This will become important later.


Stop the Simulation.

![[modularisationPotOutput.gif|Jan-11-2023 16-02-45.gif]]

The last modification needed for `readPotentometer()` is to **return** `potValue` back to where the function was called from.

![[modularisationCodeReturnPotValue.png]]

```arduino
return potValue;
```

## Rescaling the value

As noted previously, the potentiometer provides values from 0-1023. The LED brightness can be set from 0-255, meaning that the value read by the potentiometer needs to be manipulated in some way for it to be useful and not cause unexpected behaviour.

One possible solution to this issue is to use a prebuilt Arduino function called `map()`. This function takes a value and scales it from one range to another and keeps the position of the value the same.

![[modularisationLEDMapping.png|Pot -_ LED Mapping.png]]

Update `loop()`to map the potentiometer values from 0-1023 to 0-255.

![[modularisationMapPotReading.png]]

```arduino
int brightness = map(potReading, 0, 1023, 0, 255);
```

`brightness` is the value that will be used to set the brightness of the LED.

## Setting the LED brightness

Create a new function to control the brightness of the LED - `setLEDBrightness(int LEDBrightness)`. 

> [!info]  This function does not need to return any data, so the keyword used is `void`.


![[modularisationCodeLEDBrightnessStub.png]]

```arduino
void setLEDBrightness(int LEDBrightness) {
  
}
```

Set the LED brightness using the built-in `analogWrite` function, passing it the Pin to output and the `LEDBrightness` variable.

![[modularisationCodeLEDBrightnessAnalogWrite.png]]

```arduino
analogWrite(9, LEDBrightness);
```

Finally, call `setLEDBrightness()` from `loop()`, passing it `brightness`.

![[modularisationCodeLEDBrightnessCall.png]]

```arduino
setLEDBrightness(brightness);
```

## Run the Circuit

Run the simulation and you should see the LED brightness change depending on the value of the potentiometer.

![[modularisationPotLEDPWM.gif|Jan-11-2023 16-48-38.gif]]

### Final Code

```arduino
void setup()
{
  pinMode(A0, INPUT);
  pinMode(9, OUTPUT);
  Serial.begin(9600);
}

void loop()
{
  int potReading = readPotentiometer();
  int brightness = map(potReading, 0, 1023, 0, 255);
  delay(250);
  setLEDBrightness(brightness);
}

int readPotentiometer() {
  int potValue = analogRead(A0);
  Serial.println(potValue);
  return potValue;
}

void setLEDBrightness(int LEDBrightness) {
  analogWrite(9, LEDBrightness);
}
```

# Review