---
tag: Robotics
---
# Theory
![[Theory#Data Structures]]


# Arduino Implementation

## Arrays

Arrays in Arduino are treated as standard variables, made up of other types. It’s important to note that in Arduino, the arrays are declared with a single data type, meaning that you cannot mix data types within the elements.


>[!info] In Arduino, Array indexes start at 0. If your array has 5 elements, the maximum index is 4.


### Declaring Arrays

Declaring arrays can be declared in a number of approaches in Arduino, much like other C-based languages. 

Here are the options you have:

```arduino
int myInts[6];                          // 6 Element array, with null as each element. 5 is maximum index. 
int myPins[] = {2, 4, 8, 3, 6};         // 5 Element array, initialised with indicated values
int mySensVals[5] = {2, 4, -8, 3, 2};   // Mixture of the two previous options.
char message[6] = "hello";              // An array of chars, similar to a string.
```

If you know how many elements you will need, you can declare an empty array of *n* elements. When an array is declared without the initial values, each element is `null`.

### Accessing Array Elements

After declaring arrays, you can access individual elements by indicating the array by using its name, and then indicating the index within square brackets - `[]`. For example: `myInts[0] = 5;`

### Array Example

```arduino
int arrayExample[10];

void setup() {
  Serial.begin(9600);
  delay(100);
  for (int index = 0; index < 10; index++) {
	arrayExample[index] = index * 10;           // populates the array with 0, 10, 20, 30 etc.
  }
  delay(100);

  for (int index = 0; index < 10; index++) {
	Serial.println(arrayExample[index]);
  }
}

void loop() {
  // Nothing to see here...
  return;
}
```

![[dataSerialMonitorLoop.png]]

## Key Value Pair

Unlike C++, Arduino does not have the native ability to implement dictionaries. C++ refers to these structures as *maps*, although they are the same thing. This is due to the processing power and memory requirements for this structure, and the Arduino board does not have enough resources.

# Practical Exercises
# Review

1. What are the main differences between an Array and a Key Value Pair.
2. Research other data structures available in Arduino and other languages, such as linked lists. For each:
	1. Describe how they operate
	2. What situations you may wish to use them
	3. Code samples
3. Practical - Write code to generate 1000 random numbers (between 0 and 10000) and store them in an array. Sort the array (research required) so that the numbers are in ascending order. 
4. What are multidimensional dimensional arrays? Describe them and show code examples for what they are and what they can do. What situations would you use a multidimensional array over other data structures?