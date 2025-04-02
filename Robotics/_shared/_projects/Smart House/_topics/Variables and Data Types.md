---
tag: Robotics
---
# Presentation

Presentation file can be found [[Variables & Data Types|here]].

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

| Syntax | Data type | Description                                                                                                                                                                                                                                               | Possible Values (on an Arduino Uno) |
| ------ | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| int    | Integer   | On the Arduino Uno an int stores a 16-bit (2-byte) value.                                                                                                                                                                                                 | -32,768 to 32,767                   |
| bool   | Boolean   | Each **`bool`** variable occupies one byte of memory.                                                                                                                                                                                                     | `true` or `false`.                  |
| char   | Character | A data type used to store a character value - use single quotes for chars. Chars are stored as integers, meaning to can perform arithmetic. E.g. 'A' + 1 would equal 'B'                                                                                  | 'A', 'i'                            |
| byte   | Byte      | A byte stores an 8-bit unsigned number                                                                                                                                                                                                                    | 0 to 255                            |
| float  | Float     | Numbers with decimal point values                                                                                                                                                                                                                         | 5.3, 0.1                            |
| String | String    | Complex (non-primitive) data type. Comprised of a series of characters in an array. Use double quotes. An an array, Strings allow for more complex functionality, such as converting to integers, searching for substrings, converting to upper case etc. | "Lake Tuggeranong College", "32"    |
| long   | Long      | An integer data type which allocates and uses 32-bits (4 bytes).                                                                                                                                                                                          | 2,147,483,648 to 2,147,483,647      |

This table describes some more of the data types available. 

> [!info] The sizes and ranges shown are based on the Arduino Uno. Other languages and platforms may use different sizes, and therefore ranges.


![[variablesDataTypesSize.png]]

## Constants

When declaring constants in Arduino, use the const keyword *before* the data type.

![[variablesConstant.png|Screen Shot 2021-12-15 at 9.20.40 am.png]]

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