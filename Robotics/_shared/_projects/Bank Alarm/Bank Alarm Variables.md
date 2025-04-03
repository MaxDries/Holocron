


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
