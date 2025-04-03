---
theme: simple
highlightTheme: zenburn
css: css/holocronSlides.css
---
# Modularisation in Software Development

- Functions
- Modules
- Procedures
- Methods
- Routines
- Subroutines

---

# What is Modularisation?

- **Modularisation** is the process of dividing a software system into smaller, manageable parts, known as **modules**. 
- Each module represents a specific functionality or feature of the system.

---

# Why Modularise?

- **Manageability**: Smaller pieces of code are easier to manage and maintain.
- **Reusability**: Modules can be reused in different parts of the program or in different projects.
- **Team Collaboration**: Different team members can work on separate modules simultaneously.
- **Testing**: Easier to test individual modules.

---

# Example of a Module

Here’s an example of a simple module for a calculator:

```java
// Calculator Module

// Function to add two numbers
function add(a, b) {
    return a + b;
}

// Function to subtract two numbers
function subtract(a, b) {
    return a - b;
}

// Function to multiply two numbers
function multiply(a, b) {
    return a * b;
}

// Function to divide two numbers
function divide(a, b) {
    if (b != 0) {
        return a / b;
    } else {
        return "Error: Division by zero";
    }
}
```


---

# Parameters

- **Parameters** are variables used in the definition of a function.
- They act as placeholders for the values that will be passed to the function.

Example:
```javascript
function add(a, b) {
	return a + b;
}
```
- Here, `a` and `b` are parameters.
--
# Purpose of Parameters

- Parameters allow functions to be more flexible and reusable.
- They enable functions to perform operations on different data.

---

# Arguments

- **Arguments** are the actual values that are passed to a function when it is called.
- They correspond to the function's parameters.
- Example:
```javascript
add(5, 3);
```
  - Here, `5` and `3` are arguments.
--
# Purpose of Arguments

- Arguments provide the specific data that the function will operate on.
- They enable the function to process different inputs.

---

# Return Data

- **Return data** is the value that a function produces and sends back to the caller after execution.
- The return value is often the result of the function's operations.

Example: 
```javascript
function add(a, b) {
  return a + b;
}

let sum = add(5, 3);
```
  - Here, `add(5, 3)` returns `8`, which is stored in `sum`.

--
# Purpose of Return Data

- Return data allows the function to provide results to the calling code.
- It enables further processing based on the function’s output.

---

# Summary

1. **Modularisation**: Dividing a software system into smaller, manageable parts.
2. **Why Modularise**:
   - **Manageability**: Easier to manage and maintain code.
   - **Reusability**: Modules can be reused across projects.
1. **Parameters & Arguments**:

note:
- **Parameters**: Placeholders in function definitions.
- **Arguments**: Actual values passed to functions.
- **Return Data**: Values produced and sent back by functions.