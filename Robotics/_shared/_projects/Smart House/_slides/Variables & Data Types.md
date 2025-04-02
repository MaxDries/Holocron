---
theme: simple
highlightTheme: zenburn
css: css/holocronSlides.css
---
# Variables & Data Types

## Introduction to Variables

Variables in programming are used to store data that can be used and manipulated throughout the program.

---

# Declaring Variables

In Arduino, you declare a variable by specifying its data type followed by its name.
Example:

```arduino
int myNumber;
```


---

# Data Types in Arduino


Arduino supports several data types, including:

- **int**: Integer numbers
- **float**: Floating-point numbers
- **char**: Characters
- **boolean**: True/False values

Each data type determines what kind of data can be stored in the variable.

---

# Examples of Data Types

Examples of declaring different data types in Arduino:

```arduino
int score = 10;
float temperature = 36.5;
char grade = 'A';
boolean isActive = true;
```

---



Data Types can be divided up into different categories, initially into `Primitives` and `Non-Primitives`.

![[variablesDataTypeTree.png|541]]

--
### Primitive data types

Primitives are the base types of data, including booleans, integers, characters etc. 

![[variablesDataTypesExamples.png|700]]

--
### Non-Primitive Data Types

These data types offer more complexity, but need to be understood and managed in more detail. 

Examples:  Arrays, Lists, Dictionaries, `String`.

A string is a collection of characters in a sequence. In a string, each character can be considered an individual (primitive) character. In the example here, each letter is a character.

![[variablesStringsExample.png|Data Types - Strings example.png]]

---

# Using Variables

Once declared, variables can be used to store values and perform operations. Example:

```arduino
int a = 5;
int b = 10;
int sum = a + b; // sum now holds the value 15
```

---

# Variable Scope


- **Local Variables**: Declared inside a function and can only be used within that function.
- **Global Variables**: Declared outside of all functions and can be accessed from any function in the program.

---

# Variable Scope Example 

```arduino
int globalVar = 20; // Global variable

void setup() {
  int localVar = 10; // Local variable
}

void loop() {
  // localVar cannot be used here
  globalVar = globalVar + 1; // globalVar can be used here
}
```

---

# Statically vs Dynamically Typed Languages

--

## Statically Typed Languages
- **Definition**: The type of a variable is known at compile time.
- **Example Languages**: C, C++, Java, Swift
- **Advantages**:
  - **Type Checking**: Errors are caught at compile time, reducing runtime errors.
  - **Performance**: Generally faster execution due to optimized code.
  - **Readability**: Clear type definitions can make the code easier to

--

## Dynamically Typed Languages

- **Definition**: The type of a variable is known at runtime.
- **Example Languages**: Python, JavaScript, Ruby, PHP
- **Advantages**:
	- **Flexibility**: Easier to write and read code without worrying about type declarations.
	- **Productivity**: Faster development due to less boilerplate code.
	- **Adaptability**: Easy to change and refactor code.

```python
age = 25 # In Python, the type is inferred at runtime
```

---

# Summary

- Variables store data that can be used and manipulated in a program.
- Data types define what kind of data can be stored.
- Examples include `int`, `float`, `char`, and `boolean`.
- Variables can be used to perform operations and calculations.
