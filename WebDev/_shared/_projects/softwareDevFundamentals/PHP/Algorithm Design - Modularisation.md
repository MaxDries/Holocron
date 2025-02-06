---
tags: php
---
![[Theory#Algorithm Design - Modularisation]]


# PHP Implementation

In PHP, functions are identified with the `function` keyword.

![[moduleFunctionExamples.png]]

PHP functions can be called from other PHP blocks.

![[moduleFunctionCall.png]]

## PHP Arguments

Functions can accept data through Arguments. Arguments are declared in the function declaration as with many other programming languages.

![[moduleFunctionArguments.png]]

## PHP Return Data

PHP does not **require** the declaration of return data when declaring functions. Arguably it is recommended to do so if known, however not strictly required.

When declaring functions, identify the return data type after the `()` with `: <data type>`.

![[moduleFunctionReturnType.png]]

# Practical Exercises

> [!info] In this task, you’ll be creating code functions that will be used multiple times throughout the site.


The site being developed requires a number of forms on various pages, and on those pages will have a number of fields that need to be completed.

## Template Page

Open your `Software-Development Fundamentals-PHP` project. Open Create a new PHP file called `template.php`. Add the file to Git if required.

![[moduleTemplateStart.png]]

![[commonBlocks#Commit & Push]]

## Footer Code

A simple example of implementing functions at this stage is to create a function to have a standardised footer on all pages across the site.

Open `template.php`.

Create a new function called `footer()`. This function doesn’t need any parameters as it will simply dynamically generate the HTML code for the footer and return it to where it was called from.

![[moduleFooterStart.png]]

Add the code to generate a string which includes the last date and time that the file was saved.

![[moduleFooterCode.png]]

```php
$filename = basename($_SERVER["SCRIPT_FILENAME"]);
$footer = "This page was last modified: " . date("F d Y H:i:s.", filemtime($filename));
return $footer;
```

Create `index.php` if there is not already one created. Replace the contents of the file with the default code.

```php
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>PHP Template</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<h1>Heading</h1>

</body>
<script src="js/bootstrap.bundle.min.js" ></script>
</html>
```

Add the PHP code to load another file into the current file for the server to process.

> [!info] Including other files can save you a lot of development time, as you can write the code once and access it many times. Having a central location for this shared code can also make it quicker to update code, as there is only one place to update code, not spread out through other files.


![[moduleIncludeTemplate.png]]

```php
<?php include "template.php"?>
```

Execute and output the result from `footer()` near the bottom of the page - just before `</body>`.

![[moduleCallFooter.png]]

```php
<?php echo footer() ?>
```

Open the page in the browser and you should see the footer output.

![[moduleFooterOutput.png]]

One of the issues with the output is that the time is not correct, specifically, it is in GMT. Edit `template.php` to first set the timezone correctly before generating the time the file was modified.

Save the file.

![[moduleFooterUpdate.png]]

```php
date_default_timezone_set('Australia/Canberra');
```

Open `index.php` in the browser again and the time should be accurate.

![[moduleFooterCorrectTime.png]]

![[commonBlocks#Commit & Push]]

## Sanitising Data

Open `template.php`.

One of the biggest issues, outside the actual website development, that need to be considered throughout the process, is the security of site itself. Whenever websites accept data through forms, it is possible that malicious users could attempt to cause issues by including code in the form.

This is known as Cross Site Scripting (XSS).

![https://www.youtube.com/watch?v=EhOcAZJp81s](https://www.youtube.com/watch?v=EhOcAZJp81s)

![https://www.youtube.com/watch?v=M_nIIcKTxGk](https://www.youtube.com/watch?v=M_nIIcKTxGk)

As there is many fields for data to be submitted, instead of individually sanitising each field, a function can be written to sanitise the data and when the forms are submitted, the function is called for each field.

Declare a new function called `sanitiseData` which accepts a variable which will be the unsanitised data.

![[moduleFunctionSanitiseDataStart.png]]

```php
function sanitiseData($unsanitisedData)
{

}
```

This function, at this stage, will manipulate `unsanitisedData` in three ways.

First, the string is `trim`ed which removes white space (spaces/tabs) from the beginning of the string.

E.g. `data` becomes `data`.

Then `stripslashes` is run on the trimmed string which removes any `\\` from the string which can be used to indicate a string with quotes within it.

E.g. `stripslashes("Who\\'re you?")` would return `Who're you?`

Finally, `htmlspecialchars` is run which convert certain characters (&, “,. ‘, < and >) into the HTML code version. This removes the possibility that the string will have HTML code included.

E.g. `echo("This is some <b>bold</b> text.");` will be converted to `This is some &lt;b&gt;bold&lt;/b&gt; text.` . The browser then renders (displays) this as normal, however it couldn’t run it as code.

![[moduleFunctionSanitiseDataFinish.png]]

```php
$unsanitisedData = trim($unsanitisedData);
$unsanitisedData = stripslashes($unsanitisedData);
$sanitisedData = htmlspecialchars($unsanitisedData);
return $sanitisedData;
```

This function will be used in the future whenever data is uploaded through a HTML form.

![[commonBlocks#Commit & Push]]

![[moduleFunctionSanitiseCommit.png]]

## Contact Page

Create a new file `contact.php` and copy the contents of index into it.

Add code to create the HTML form below the H1 tag and about the footer() output.

```php
<div class="container-fluid">
    <h1 class="text-primary">Please Send us a Message</h1>
    <form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post">
        <div class="mb-3">
            <label for="contactEmail" class="form-label">Email address</label>
            <input type="email" class="form-control" id="contactEmail" name="contactEmail"
                   placeholder="name@example.com">
        </div>
        <div class="mb-3">
            <label for="contactMessage" class="form-label">Message</label>
            <textarea class="form-control" id="contactMessage" name="contactMessage" rows="3"></textarea>
        </div>
        <button type="submit" name="formSubmit" class="btn btn-primary">Submit</button>
    </form>
</div>
```

