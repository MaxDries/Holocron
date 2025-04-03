---
tag: Robotics
---
# Theory
![[Theory#Algorithm Design - Decisions]]


# Arduino Implementation
## IF..THEN..

In Arduino, as with many other languages, the main decision block is the `IF..THEN..`. The syntax for this in Arduino is:

![[decisionsIfBlock.png|Screen Shot 2021-12-16 at 2.07.37 pm.png]]

> [!info] Note the condition is enclosed in parenthesis.


For the code block to execute, the condition needs to equate to **true**. If the condition equates to **false**, the code block is skipped.

### IF..THEN.. Example

```arduino
if (x > 120) {
  digitalWrite(LEDpin1, HIGH);
  digitalWrite(LEDpin2, HIGH);
}
```

In this example, `x > 120` is the condition and `digitalWrite(LEDpin1, HIGH);
  digitalWrite(LEDpin2, HIGH);` is the code block that will run only if the condition is true at the time it is executed.

## IF..THEN.. ELSE..

![[decisionsIfElseBlock.png|Screen Shot 2021-12-16 at 2.28.59 pm.png]]

The standard IF..THEN structure can be extended with an ELSE to allow a code block to run if the condition is false.

### IF..THEN..ELSE.. Example

```arduino
if (x > 120) {
  digitalWrite(LEDpin1, HIGH);
  digitalWrite(LEDpin2, HIGH);
} else {
	digitalWrite(LEDpin1, LOW);
  digitalWrite(LEDpin2, LOW);
}
```

In this case, if x is greater than 120, then the LEDs will be turned on. If x is NOT greater than 120, the LEDs will turn off.

## Switch ... Case

Switch cases are a different type of control structure to replace multiple, nested if..then statements. 

Based on a given variable, Arduino code can have multiple cases or a catch-all state - default. They can be used to simplify code, but still provide multiple cases for variables.

### Syntax

```arduino
switch (var) {
  case label1:
	// statements
	break;
  case label2:
	// statements
	break;
  default:
	// statements
	break;
}
```

### Example

```arduino
x = random(5);
switch (x) {
	  case 0: y = y + 1;
	  case 1: y = y + 5;
	  case 2: y = y + 2;
	  case 3: y = y + 7;
	  case 4: y = y + 3;
	  default: y = 0;
	}

// Compared to the equivalent IF statements:
if (x == 0) y = y + 1;
	else if (x == 1) y = y + 5;
	else if (x == 2) y = y + 2;
	else if (x == 3) y = y + 7;
	else if (x == 4) y = y + 3;
	else y = 3;
```

## **Comparison Operators**

The standard *comparison* operators in Arduino are shown here and can be used in `if` conditions.

| Operator | Description |
| --- | --- |
| `!` | Not |
| `x == y` | x is equal to y |
| `x != y` | x is not equal to y |
| `x < y` | x is less than y |
| `x > y` | x is greater than y |
| `x <= y` | x is less than or equal to y |
| `x >= y` | x is greater than or equal to y |

## Logical Operators

| Operator | Description | Example |
| --- | --- | --- |
| && | AND  |  |
| || | OR |  |
| ! | NOT |  |


# Review

1. Describe a decision in programming using plain language.
	1. Update the sentence with more technical language. 
2. What’s a nested if statement?
3. What’s the difference between a nested if block and a case switch?
4. What logic gates or circuits can be found in the “StateChangeDetection Arduino” code?
