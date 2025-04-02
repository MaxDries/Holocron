
This project focuses on designing and simulating a smart house system using robotics principles within the Tinkercad environment. We will explore how various sensors, actuators, and microcontroller programming can be integrated to create an automated and intelligent living space


# TinkerCAD Project

Create a new project by opening the "Smart House" project in the class on TinkerCAD:

![[smartHouseProjectActivity.png]]

In this empty activity, add the Arduino Uno and a breadboard. Wire the power rails appropriately.

![[smartHouseArduinoBreadboard.png]]


Before adding any sensors or actuators, or any additional components, and focus on the code design. The goal of this is to create code that is:
- clean
- organised
- self-documenting (to a point)
- simplified at the high-level (i.e. the `loop()` function only contains function calls to other functions).

# Required Theory

In order to continue with the project, there are some theory topics that you will need to know:

- [[Robotics/_shared/_projects/Smart House/_topics/Variables and Data Types|Variables and Data Types]]
- [[Robotics/_shared/_projects/Smart House/_topics/Style Guide|Style Guide]]
- [[Robotics/_shared/_projects/Smart House/_topics/Algorithm Design - Sequence|Sequences]]
- [[Robotics/_shared/_projects/Smart House/_topics/Algorithm Design - Decisions|Decisions]]
- [[Robotics/_shared/_projects/Smart House/_topics/Algorithm Design - Loops|Loops]]
- [[Robotics/_shared/_projects/Smart House/_topics/Data Structures|Data Structures]]

# Code Design

Let's assume you've got 5 robotic behaviours that you wish to implement. For instance:
1) When the motion sensor detects movement in a room, the light bulb turns on. If no movement is detected for a specified period, the light bulb turns off to save energy.
2) When the temperature sensor detects that the room temperature has fallen below a set threshold, the smart thermostat activates the heater. Conversely, it activates the air conditioner if the temperature exceeds the threshold.
3) When the motion sensors detect movement and door/window sensors register unauthorized entry, the alarm system is triggered. Simultaneously, smart lights turn on in specific areas to deter intruders, and security cameras start recording. Door locks automatically engage to secure all entry points.
4) When the light sensor detects excessive sunlight, the motorised blinds lower automatically to reduce glare and heat. Conversely, they raise to allow more natural light when the sunlight is insufficient.
5) When the soil moisture sensor detects that the soil in the garden is too dry, the smart sprinkler system activates, watering the garden. It turns off automatically once the desired moisture level is achieved.

The code design should follow the features discussed in [[Robotics/_shared/_projects/Smart House/_topics/Algorithm Design - Modularisation|Modularisation]]. Therefore the high-level view of the code would appear to be something similar to this:

![[smartHouseHighLevelCode.png]]

> [!note] In this flowchart you should be able to see that `loop()` has only got minimal amount of code in it, just calls to the other functions. 
> This allows the loop function to focus on the high-level logic (what order the functionality is in). 
> The low-level code is implemented in the specific functions. 
> This benefits the code to make it easier for the programmer to know where the code is located for each part of the project.



# Code Implementation

The first step is to define the code structure according to the code design. For instance, if implementing the above design, the code structure would be:

```arduino
void setup()
{
 
}

void loop()
{
  
  motionDetectionLights();
  autoAirconditioner();
  autoAlarmSystem();
  autoBlinds();
  autoWateringSystem();
  
  delay(100);
}


void motionDetectionLights() {
  
}

void autoAirconditioner() {
  
}

void autoAlarmSystem() {
  
}

void autoBlinds() {
  
}

void autoWateringSystem() {
  
}


```


# Approach coding

With the initial setup complete, it's time to add addition components, both input and output.

Follow these steps to implement code (if you're unsure):

1. Components
	1. Pins, find out what each of then pins are on the components (VCC, GND, signal etc)
	2. Wire the pins to the Arduino. Find out where each pin needs to be connected to.
2. Code
	1. Configuration, define the pins and variables.
	2. `setup()` - add the required code to the setup function.
	3. Custom Function - use one of the functions created previously, or create a new one.
		1. Input - test the input for the input device
		2. Output - the the code for the output device
		3. Processing - write the logic for how the input device impacts the output device.

All these steps require research. You will need to google each component and specifically the options available in TinkerCad. 

> [!important] Every component is different and requires different code.

When googling, use keywords such as : `tinkercad <component> wiring`

