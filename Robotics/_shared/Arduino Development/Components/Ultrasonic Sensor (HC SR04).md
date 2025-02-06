---
tag: Robotics
---
# Ultrasonic Sensor (HC SR04)

```arduino

/*
  HC-SR04 Ping distance sensor]
  More info at: http://goo.gl/kJ8Gl
  Original code improvements to the Ping sketch sourced from Trollmaker.com
  Some code and wiring inspired by http://en.wikiversity.org/wiki/User:Dstaub/robotcar
  http://www.instructables.com/id/Simple-Arduino-and-HC-SR04-Example/?ALLSTEPS
*/

#define trigPin 5 // Change this pin to match your robot.
#define echoPin 6 // Change this pin to match your robot.

void setup() {
  Serial.begin (115200);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  //while(!Serial.available()){}
  Serial.println("HC-SR04 tester");

}

void loop() {
  long duration, distance;
  digitalWrite(trigPin, LOW); 
  delayMicroseconds(2); 
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration / 2) / 29.1;
  Serial.print(distance);
  Serial.println(" cm");
  if (distance >= 200 || distance <= 0) {
    Serial.println("Out of range");
	}
  delay(50);
}
```