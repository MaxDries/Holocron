---
tags: php
---
![[Theory#Variables and Data Types]]


# PHP Implementation

In PHP, all user defined variables **must** start with `$`. 

For example:

`$username = **"Obiwan Kenobi"**;`

In this case `username` is the variable name.

> Rules for PHP variables:
A variable starts with the `$` sign, followed by the name of the variable
A variable name must start with a letter or the underscore character
A variable name cannot start with a number
A variable name can only contain alpha-numeric characters and underscores (A-z, 0-9, and _ )
Variable names are case-sensitive (`$age` and `$AGE` are two different variables)
[https://www.w3schools.com/php/php_variables.asp](https://www.w3schools.com/php/php_variables.asp)
> 

## Declaring Variables

PHP is a Loosely Typed Language (alternatively called Weakly Typed) meaning the rules for what data can be stored in variables is lax. For example, you do not need to declare the data type of variables when you first use them, unlike other some other languages (most notably C-based languages (Arduino, Java etc). 

```php
$radius = 5;
$area = pi() * $radius * $radius;
echo "The area is ", $area;
```

More complex example:

```php
$numberInput = $_GET["age"];
```

## Using Variables

After the variable has been declared and data stored within it, the data can be used at a later stage of the code. When the code executes, the value stored within the variable name is used instead:

This example will calculate the area of a circle with a radius of 5.

The output will be :

![[variablesVariableOutput.png|Screenshot 2022-11-21 at 2.52.24 pm.png]]

```php
$radius = 5;
$area = pi() * $radius * $radius;
echo "The area is ", $area;

```

```php
$numberInput = $_GET["age"];
echo "The square of ".$numberInput." is ".squareNumber($numberInput)."<br>";
echo "The square root of ".$numberInput." is ".squareRootNumber($numberInput)."<br>";
echo "<br>";
```

## PHP Data Types

PHP Supports these data types.

| Data type | Description | Possible Values |
| --- | --- | --- |
| Integer | Whole number value. | Range between -2,147,483,648 and 2,147,483,647 |
| Boolean | Value either true or false (1 or 0). | `true` or `false`.  |
| Float | Numbers with decimal point values | 5.3
0.1 |
| String | Complex (non-primitive) data type. Comprised of a series of characters in an array. Use double quotes. An an array, Strings allow for more complex functionality, such as converting to integers, searching for substrings, converting to upper case etc. | "Lake Tuggeranong College"
"32" |

## Constants

Constants are variables where the channel cannot be changed once it’s been defined.

```php
define("school", "Lake Tuggeranong College");
```

# Practical Exercises

> [!info]  **Goal**
> 
> In this task, you’ll learn:
> 
> - How to create a simple HTML form,
> - The PHP code to collect the data the user entered in the form, and
> - How to output variables onto a HTML page.


Open your `Software-Development Fundamentals-PHP` project. In this stage of the project, you will be creating a simple contact form, which will collect data from the user and then display it back to them to confirm. 

## Contact Page

Right click on the Project name in the Project Explorer and choose New→PHP file. 

![[variablesNewPHPFile.png|Screenshot 2022-11-21 at 2.11.30 pm.png]]

Enter `contact` for the filename. This will create the contact.php file.

![[variablesNewPHPFileName.png|Screenshot 2022-11-21 at 2.13.41 pm.png]]

PHPStorm may ask you if you wish to add this to the Git Repository. Click `Don’t Ask Again`, and then click Add.

![[variablesAddFileToGit.png|Screenshot 2022-11-21 at 2.13.50 pm.png]]

Replace the contents of the the file with the standard ‘template’ for HTML files for this project.

[https://gist.github.com/RyanCather/7af6a1df2bc62ec750d137ea9a77d336](https://gist.github.com/RyanCather/7af6a1df2bc62ec750d137ea9a77d336)

After the heading copy the code for a simple HTML form.

This may look complex, however much of the code is Bootstrap code to display the form in a certain way. 

At this stage, the important aspects to note is the `name` attributes of each of the `input` fields - `inputEmail` and `inputMessage`. These will be referred to later in PHP code.

![[variablesContactForm.png|Screenshot 2022-11-22 at 9.49.12 am.png]]

```html
<div class="container-fluid">
	<form action="contact.php" method="post">
		<div class="mb-3">
			<label for="inputEmail" class="form-label">Email address</label>
			<input type="email" class="form-control" id="inputEmail" name="inputEmail" aria-describedby="emailHelp">
			<div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
		</div>
		<div class="mb-3">
			<label for="inputMessage" class="form-label">Message</label>
			<input type="text" class="form-control" id="inputMessage" name="inputMessage">
		</div>
		<button type="submit" class="btn btn-primary">Submit</button>
	</form>
</div>
```

At this stage, the page simple shows the form but doesn’t do anything with the data when the user presses submit. That’s where PHP is introduced.

![[variablesContactFormDisplay.png|Screenshot 2022-11-22 at 9.50.51 am.png]]

After the form add PHP code to detect if the form has been submitted. This is done through the `$_SERVER["REQUEST_METHOD"] == "POST"` check. 

`POST` is a standard webserver protocol, and is linked with the `<form action="contact.php" method="post">` code created earlier. When the user presses the submit button on the form, the form is `post`ed back to the server. The PHP code added detects that `post` and can respond accordingly.

![[variablesIfPost.png|Screenshot 2022-11-22 at 9.52.34 am.png]]

```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {

}
?>
```

If the PHP code detects the post (meaning the user presses the submit button), the code will then access and store the data the user entered into the form field.

`$_POST['inputEmail'];` refers to the field with the name `inputEmail` created earlier. Likewise for `inputMessage`.

> [!info] `$emailAddress` and `$messageSubmitted` are two variables to store the data entered into the form. What data type would they be?


![[variablesGetFormData.png|Screenshot 2022-11-22 at 9.55.38 am.png]]

```php
$emailAddress = $_POST['inputEmail'];
$messageSubmitted = $_POST['inputMessage'];
```

The last step is then to output the values stored in the two variables. This can be done simply by using the `echo` command to output the values onto the webpage itself.

![[variablesDisplayFormData.png|Screenshot 2022-11-22 at 9.57.10 am.png]]

```php
echo $emailAddress;
echo "<p>";
echo $messageSubmitted;
```

Test the form by entering data and pressing submit. The data entered should be displayed on screen.

![[variableSubmitDemo.gif|2022-11-22 09-59-41.2022-11-22 10_00_25.gif]]

Update the Heading on the page to the Contact page.

![[variableContactUsHeading.png|Untitled]]

![[variableContactUsForm.png|Untitled]]

In this Contact form, a standard programming process has been developed - Input → Processing → Output. The data gets inputted into the system, it is processed and then outputted.

![[commonBlocks#Commit & Push]]

# Review

1. How is data stored in memory?
2. What’s the difference between primitive and non-primitive data types?
3. What is type inference?
	1. Find three programming languages that use type inference.
4. What is the opposite of type inference?
	1. Find three programming languages that don’t use type inference.
5. What does strongly typed and weakly typed languages mean?


