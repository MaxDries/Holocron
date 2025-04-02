---
tag: Robotics
---
# Theory
![[Theory#Algorithm Design - Sequence]]

# Arduino Implementation

Arduino is a subset of the C++ language. C++, invented in 1985, is an extension to C, which was invented in 1972. So, the basis for the language is quite old, in computer science terms. Luckily the Arduino language simplifies some aspects of C++ to suit the Arduino environment.

Consider the code sample for `Blink`. 

## Brackets

Notice the use of brackets - () and {} - these are very important in the Arduino language (as they are in C and C++). 

> [!info] Each open bracket, whichever type, must have a corresponding close bracket.


The standard brackets - or parentheses - indicate **conditions** and **parameters** and **arguments**. 

The Curly Brackets - { and } - indicate the start and the end of a code block. This code block can be a **function** (as can be seen in the image with `setup()` and `loop()`) and also with **decisions** and **loops**. 

![[sequenceBlink.png|Screen Shot 2021-12-16 at 1.23.37 pm.png]]

## Semicolon

The details of these topics will be discussed later in the course - just be aware of the different brackets and their uses at this stage.

You may also notice that *almost* each line of code shown ends with a semi-colon. In C-based languages (Java, javascript, Arduino etc), the semi-colon identifies the end of a statement.

## Whitespace

In programming, whitespace refers to the use of indentation (tabs), space characters new newline characters.

In Arduino, the compiler generally ignores whitespace, meaning you can have as many blank lines in your code as you wish - the code will still compile.

Indentation is also not critical, however it makes it easier to read code blocks if indented correctly.

> [!info]  CTRL+T is the shortcut to auto-format your code and fix up indentation.


# Review

1. Write the sequence of steps to make and eat toast.
2. What the sequence of steps to 
	1. Read in individual lines of a text file (example shown)
	2. Calculate the GST
	3. Output the GST component to the user.


