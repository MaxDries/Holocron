# GPS

```arduino
/*
   Gets the GPS coords and tests whether it's in "bounds"

   @params: none
   @return: void
*/
void locationBarrier() {
  while (ss.available() > 0)
	if (gps.encode(ss.read()))
	  getGPSInfo();
}

void getGPSInfo()
{
  Serial.print(F("Location: "));
  if (gps.location.isValid())
  {
	Serial.print(gps.location.lat(), 6);
	Serial.print(F(","));
	Serial.print(gps.location.lng(), 6);
  }
  else
  {
	Serial.println(F("INVALID"));
  }
}
```

# Infrared Remote

```arduino
/*
   Gets the value given by the Keyes IR remote.
   Code values are:

   Up     : 25245
   Down   : -22441
   Left   : 8925
   Right  : -15811
   Ok     : 765
   1      : 26775
   2      : -26521
   3      : -20401
   4      : 12495
   5      : 6375
   6      : 31365
   7      : 4335
   8      : 14535
   9      : 23205
   0      : 19125
   #      : 21165
   *      : 17085

   Test against each code and perform required action. See example in code.

   @params: None
   @return: void
*/
void remoteDecode() {

  if (irrecv.decode(&results)) {

	int code = results.value;
	//    Serial.println(code);
	if (code == 25245) {  // Up
	  Serial.println("Up");
	}
	irrecv.resume();
  }
}
```

# Traffic Light Module

When needed On, write HIGH to the pin (change led pin to relevant colour.

```arduino
digitalWrite(ledRed, HIGH);
```

When needed Off, write LOW to the pin (change led pin to relevant colour.

```arduino
digitalWrite(ledRed, LOW);
```

# DC Motor

```arduino
motor.forward();
delay(1000);
motor.stop();
delay(1000);
motor.backward();
delay(1000);
```

# DC Motor (DFRobot)

```arduino
int speedValue = 255; // Can be 0-255.
digitalWrite(M1,HIGH);
analogWrite(E1, speedValue);   //PWM Speed Control
```

# Servo

```arduino
// Servo position values range from 0-180
int servoPos = 100;
myservo.write(servoPos);
```

# Moisture Sensor

```arduino
int moistureValue = analogRead(moisturePin);
```

# Potentiometer

```arduino
int potValue = analogRead(pot);            // reads the value of the potentiometer (value between 0 and 1023)
```

# Piezo

```arduino
tone(piezoPin, 1000); // Send 1KHz sound signal...
delay(100);
noTone(piezoPin);
```

# Sonar

```arduino
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
// Sets the trigPin HIGH (ACTIVE) for 10 microseconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
// Reads the echoPin, returns the sound wave travel time in microseconds
long duration = pulseIn(echoPin, HIGH);
// Calculating the distance
int distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)
```

# Line Sensor

```arduino
int lineSensorValue = digitalRead(lineSensorPin);
```

# Crash Sensor (button)

```arduino
int crashSensorValue = digitalRead(crashSensor);
```

# PIR Sensor

```arduino
int pirValue = digitalRead(pirSensor);
```

# RFID

```arduino
void readRFID() {

  String uidOfCardRead = "";
  String validCardUIDWork = "198 128 61 43";  // Change this as needed
  String validCardUIDHome = "00 232 81 25";   // Change this as needed.

  if (rfid.PICC_IsNewCardPresent()) { // new tag is available
	if (rfid.PICC_ReadCardSerial()) { // NUID has been readed
	  MFRC522::PICC_Type piccType = rfid.PICC_GetType(rfid.uid.sak);

	  for (int i = 0; i < rfid.uid.size; i++) {
		uidOfCardRead += rfid.uid.uidByte[i] < 0x10 ? " 0" : " ";
		uidOfCardRead += rfid.uid.uidByte[i];
	  }
	  //Serial.println(uidOfCardRead);

	  rfid.PICC_HaltA(); // halt PICC
	  rfid.PCD_StopCrypto1(); // stop encryption on PCD
	  uidOfCardRead.trim();
	  if (uidOfCardRead == validCardUIDWork || uidOfCardRead == validCardUIDHome) {
		return true;
	  } else {
		return false;
	  }
	}
  }
}
```

