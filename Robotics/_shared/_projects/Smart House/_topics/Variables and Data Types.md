---
tag: Robotics
---
# Theory
![[Theory#Variables and Data Types]]

# Arduino Implementation

In Arduino, variables **must** be declared before they can be used. If they are not declared, you will get a syntax error.

Unlike some other languages, when programming in Arduino you must specify the data type of a variable using the specific keyword - `int`, `string` etc.

Arduino is a statically typed language, meaning that once a variable has been declared as a certain data type, it cannot change throughout the execution of the code. This is different from some other languages (namely Python, GDScript, PHP etc) which are dynamically typed where the data type of a variable can be changed during the execution of code.

## Declaring variables

When declaring variables in Arduino, you must indicate the data type prior to declaring the name and initial value (if any). A simple counter variable could be declared such as:

```arduino
int sensorReadCounter;
```

You can also set an initial value when declaring variables:

```arduino
string loggedInUser = "None";
```

## Arduino Data Types

In Arduino, there are a number of the standard primitive types. The syntax for each of these are:

| Syntax | Data type | Description | Possible Values (on an Arduino Uno) |
| --- | --- | --- | --- |
| int | Integer | On the Arduino Uno an int stores a 16-bit (2-byte) value. | -32,768 to 32,767 |
| bool | Boolean | Each **`bool`** variable occupies one byte of memory. | `true` or `false`.  |
| char | Character | A data type used to store a character value - use single quotes for chars. Chars are stored as integers, meaning to can perform arithmetic. E.g. 'A' + 1 would equal 'B' | 'A', 'i' |
| byte | Byte | A byte stores an 8-bit unsigned number | 0 to 255 |
| float | Float | Numbers with decimal point values | 5.3, 0.1 |
| String | String | Complex (non-primitive) data type. Comprised of a series of characters in an array. Use double quotes. An an array, Strings allow for more complex functionality, such as converting to integers, searching for substrings, converting to upper case etc. | "Lake Tuggeranong College", "32" |
| long | Long | An integer data type which allocates and uses 32-bits (4 bytes). | 2,147,483,648 to 2,147,483,647 |

This table describes some more of the data types available. 

> [!info] The sizes and ranges shown are based on the Arduino Uno. Other languages and platforms may use different sizes, and therefore ranges.


![[dataTypesSize.png]]

## Constants

When declaring constants in Arduino, use the const keyword *before* the data type.

![[variablesConstant.png|Screen Shot 2021-12-15 at 9.20.40 am.png]]

# Practical Exercises


> [!info] **Goal**
In this task, you’ll be updating the Bank Alarm Project on TinkerCAD to use more variables and constants.



Open the Bank Alarm project on [Tinkercad](https://www.tinkercad.com/). 

This code is the code at the conclusion of the Modularisation stage.

You may recall that a few variables were used throughout this process - `potReading`, `brightness` etc.

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

Start updating the code by declaring a few constants to store the pins used by the LED and the potentiometer.

> [!info] These have been declared as **constants** as there is no need for the contents to change throughout the execution of the code.


![[variablesConstDeclared.png]]

```arduino
const int pinLED = 9;
const int pinPot = A0;
```

Replace any reference to pins 9 or A0 with the corresponding constant.

> [!info] You may have noticed that A0 has been defined as a integer, despite the A. This is because the Arduino language automatically converts this A0 ‘symbol’ into the number 14 for the Arduino Uno. 
> On the Arduino Uno, pin A0 can also be referred to as pin 14. Try updating the code and checking if it works!


![[variablesUseInsteadOfPins.png]]

This has the benefit of future maintainability - if the circuit needed to be changed and the devices must change pins, the **only** section of code you would need to change is the constants at the start of the code. The remainder of the code wouldn’t need to be changed.

# Review

1. How is data stored in memory?
2. What’s the difference between primitive and non-primitive data types?
3. What is type inference?
	1. Find three programming languages that use type inference.
4. What is the opposite of type inference?
	1. Find three programming languages that don’t use type inference.
5. What does strongly typed and weakly typed languages mean?

Using tinkerCAD, create a circuit with just an Arduino Uno and write code to:

1. Declare two variables, add them together (stored in a third variable) and output the result to the Serial Monitor.

![[variablesSerialMonitorOutput.png]]

2. Read a name, as a String, from the Serial Monitor, store it in a variable and then output "Hello `<name>`" back to the Serial Monitor.

![[variablesSerialMonitorInput.gif]]