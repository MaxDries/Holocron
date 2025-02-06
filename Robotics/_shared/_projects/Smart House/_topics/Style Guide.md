---
tag: Robotics
---
# Theory
![[Theory#Style Guide]]


# Arduino Implementation

## Naming Conventions

**Summary:** In Arduino, the conventions for naming items are:

| Variables | lowerCamelCase |
| --- | --- |
| Constants | ALLCAPITALS |
| Functions | lowerCamelCase |

It is also standard convention to include comment blocks at the top of the code to explain the code, but also to include function header comments to explain the purpose of each function.

Taken from : [https://www.arduino.cc/en/Reference/StyleGuide](https://www.arduino.cc/en/Reference/StyleGuide)

### Variables

Avoid single letter variable names. Make them descriptive

Avoid variable names like `val` or `pin`. Be more descriptive, like `buttonState` or `switchPin`

If you want to define pin names and other quantities which won't change, use const ints. They're less messy than `#define`s, yet still give you a way to teach the difference between a variable and a constant.

Use the wiring/Processing-style variable types, e.g. 

- `boolean`,
- `char`,
- `byte`,
- `int`,
- `unsigned int`,
- `long`,
- `unsigned long`,
- `float`,
- `double`,
- `string`,
- `array`,
- `void`

when possible, rather than `uint8_t`, etc. The former are explained in the documentation, and less terse names.

Avoid numbering schemes that confuse the user, e.g.:

```arduino
pin1 = 2
pin2 = 3
etc.
```

If you need to renumber pins, consider using an array, like this:

```arduino
int myPins[] = { 2, 7, 6, 5, 4, 3 };
```

This allows you to refer to the new pin numbers using the array elements, like this:

```arduino
  digitalWrite(myPins[1], HIGH);  // turns on pin 7

```

It also allows you to turn all the pins on or off in the sequence you want, like this:

```arduino
for (int thisPin = 0; thisPin < 6; thisPin++) {
   digitalWrite(myPins[thisPin], HIGH);
   delay(500);
   digitalWrite(myPins[thisPin], LOW);
   delay(500);
}
```

## Example - Bad

What does this code do?

```arduino
void fc(float tt) {
  float c = ts.readTempC();
  m->setSpeed(100);
  if (c < tt) {
	fe = false;
  } else {
	fe = true;
  }
}
```

## Example - Good

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

More Information:

[5.12. Arduino Coding Style Guide - 16-223 Introduction to Physical Computing](https://courses.ideate.cmu.edu/16-223/f2016/text/resrc/coding-style-guide.html)

## Commenting / Internal Documentation

Each function needs a block comment to describe the purpose of the function. This block comment also should describe any arguments (inputs) and any return value (outputs).

Consider the example shown previously.

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

The block comment at the start of the function has three sections to it

1. A description of the function and it’s goal, in plain language.
2. A description of each of the parameters (`@param`).
3. A description of the data that is returned (or **void** if none).

[https://youtu.be/2MZAC6HL-J8](https://youtu.be/2MZAC6HL-J8)

## Good vs Bad Comments

[Coding Rules: the Power of Correct Names, Good and Bad Comments](https://codegym.cc/groups/posts/369-coding-rules-the-power-of-correct-names-good-and-bad-comments)

For more information, please see

[5.12. Arduino Coding Style Guide — 16-223 Introduction to Physical Computing](https://courses.ideate.cmu.edu/16-223/f2016/text/resrc/coding-style-guide.html)

# Practical Exercises

1. Update all existing code to match naming conventions & function header comments.

# Review