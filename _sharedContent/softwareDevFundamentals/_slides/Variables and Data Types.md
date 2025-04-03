---
theme: simple
highlightTheme: zenburn
css: css/holocronSlides.css
---
# Variables and Data Types


**What are Variables?**

- Variables are named storage locations in memory.
- They hold values that can be used and manipulated in a program.
**What are Data Types?**

- Data types specify the kind of data a variable can hold.


---
# Variables

- Store data in memory.
- Use the value later.
- Variables have a name

![[variablesBoxes3.jpeg|variablesBoxes3.jpeg]]

note:
Some programming languages require the variables to be *declared* before they can use them. 



---
# Data Types

Define the type (structure) of data that can be stored using a variable.

--

# Data Type Categories


Data Types can be divided up into different categories, initially into `Primitives` and `Non-Primitives`.


note:
Primitives are the most simplest types of data, which can be used to create more complex, non-primitive data structures.


--

![[variablesDataTypeTree.png]]

--

# Primitives


| Data Type | Details            | Example |
| --------- | ------------------ | ------- |
| Boolean   | True or False      | True    |
| Integer   | Whole Number       | 32      |
| Float     | Decimal Value      | 24.5    |
| Character | A single character | 'c'     |

--
# Non-Primitives

Some examples of non-primitive data types are : `Arrays`, `Lists`, Dictionaries, but the most common one you'll deal with when starting out is a `String`.

A `string` is a collection of characters in a sequence.. In the example here, each letter is a character.

![[variablesStringsExample.png|Data Types - Strings example.png]]

---
# Assignment Statements

Assigns a value to a variable.


```
variable = expression
```

- The right-hand side of the `=` is calculated first.
- the **answer** of the expression is stored in the variable to the left.

--

# Assignment Example

| Example                     | Explanation                                        |
| --------------------------- | -------------------------------------------------- |
| `Age = 42`                  | Sets the `Age` variable to the integer 42          |
| `Name = “Anakin Skywalker”` | Assigns “Anakin Skywalker” to the variable `Name`. |
