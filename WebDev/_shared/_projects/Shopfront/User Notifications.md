
# User Notifications

An important aspect to the user experience (UX) is giving appropriate feedback to users. Notifications need to inform the user of any errors that have occurred but also inform the user if something has occurred successfully if it may not be obvious otherwise (such as logging in).

[A Comprehensive Guide to Notification Design | Toptal®](https://www.toptal.com/designers/ux/notification-design)

Currently in the project there is minimal feedback to the user, although some has been implemented. One would be the use of the welcome message once the user has logged in. Another is when a product is added to the shopping cart etc.

## Notification Centre

To create a notification system, start by creating part of the website to display the messages. The best place would be to create this in the template.

Open `template.php` and find a position to include the notifications. In this example, it has been placed simply after the navbar is closed off.

This section of code will display any text that has been saved in the ‘`flash_message`’ session variable. It’s stored (line 62) in the `$message` variable, then the session variable is cleared (line 63).

The contents of `$message` is then displayed on the webpage (line 66).

![[flashMessagesCode.png]]

```php
<?php
if (isset($_SESSION['flash_message'])) {
	$message = $_SESSION['flash_message'];
	unset($_SESSION['flash_message']);
//    echo $message;
	?>
	<div class="position-absolute bottom-0 end-0">
		<?= $message ?>

	</div>

	<?php
}
?>
```

## Notifications

To notify the users of any errors or updates, you can simply assign the message to the flash message session variable. For example, with the login process, you may wish to notify the user that they’ve logged in successfully, and also notify them if the login with unsuccessful.

![[flashMessagesSessionVariableExample.png]]

```php
$_SESSION["flash_message"] = "<div class='bg-success'>Login Successful</div>";

$_SESSION["flash_message"] = "<div class='bg-danger'>Invalid Username or Password</div>";
```

![[flashMessagesExampleOutput.png]]

## Notification Colours

Notice the different classes in the example for the two messages - one uses `bg-success` and the other uses `bg-danger`. These are bootstrap classes to define the colour of the backgrounds of the div blocks.

The different classes available can be found here:

[Background](https://getbootstrap.com/docs/5.2/utilities/background/)

![[bootstrapColours.png]]

Update the other pages to notify the users of any updates to the system and/or errors. Some examples may be:

- The user has attempted to access a page they’re not authorised
- The user has successfully ordered products
- Orders have been opened or closed.
- Contact Us message has been successfully logged
- etc.