---
tag: Robotics
---
# Theory
![[Theory#Algorithm Design - Loops]]

# Arduino Implementation

The Arduino language has the three loop structures identified on the General Learning Resource - `while`, `do..while` and `for`. As Arduino is a C-based language, the syntax for these are the same as with C, C++ etc.

Here you can see the syntax and an example of each.

## While Loop

```arduino
while (condition) {
  // statement(s)
}
```

### Example

```arduino
var = 0;
while (var < 200) {
  // do something repetitive 200 times
  var++;
}
```

## Do... While Loop

```arduino
do {
  // statement block
} while (condition);
```

### Example

```arduino
int x = 0;
do {
  delay(50);          // slight delay
  x = x + 1;          // add 1 to x.
} while (x < 100);
```

## For Loop

```arduino
for (initialization; condition; increment) {
  // statement(s);
}
```

### Example

```arduino
for (int i = 0; i <= 255; i++) {
	analogWrite(PWMpin, i);
	delay(10);
  }
```

> [!info] Remember the *condition* has to equate to `true` or `false`.


# Review

1. What is the difference between the available types of loops? Why choose one of the other/s?
2. How many times can loops iterate?
3. Write the steps in plain language, **using loops**, to 
	1. Read in individual lines of a text file (use the one shown)
	2. Calculate the GST
	3. Output the GST component to the user.
