# User Registration

To begin the process of User Management, the user will need to be able to register an account.

Create a new file - `register.php` and replace the contents with the starting code.

```php
<?php include "template.php";
/**  @var $conn */
?>
	<title>User Registration</title>
	<h1 class='text-primary'>User Registration</h1>

	<!-- Front End -->
	<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post" enctype="multipart/form-data">
		<div class="container-fluid">
			<div class="row">
				<!--Customer Details-->

				<div class="col-md-6">
					<h2>Account Details</h2>
					<p>Please enter wanted username and password:</p>
					<p>Email Address<input type="text" name="username" class="form-control" required="required"></p>
					<p>Password<input type="password" name="password" class="form-control" required="required"></p>

				</div>
				<div class="col-md-6">
					<h2>More Details</h2>
					<!--Product List-->
					<p>Please enter More Personal Details:</p>
					<p>First Name<input type="text" name="firstName" class="form-control" required="required"></p>
					<p>Second Name<input type="text" name="secondName" class="form-control" required="required"></p>
					<p>Address<input type="text" name="address" class="form-control" required="required"></p>
					<p>Phone Number<input type="text" name="phoneNumber" class="form-control" required="required"></p>
				</div>
			</div>
		</div>
		<input type="submit" name="formSubmit" value="Submit">
	</form>
```

Add the link to the navbar.

![[userRegistrationNavigation.png]]

```php
<li class="nav-item">
	<a class="nav-link" href="register.php">Register</a>
</li>
```

Back on `register.php` add a PHP block to the end of the file and collect (and sanitise) the data the user entered.

![[userRegistationSanitiseInput.png]]

```php
<?php
// Back End
if ($_SERVER["REQUEST_METHOD"] == "POST") {
		$username = sanitiseData($_POST['username']);
	$password = sanitiseData($_POST['password']);
	$firstName = sanitiseData($_POST['firstName']);
	$secondName = sanitiseData($_POST['secondName']);
	$address = sanitiseData($_POST['address']);
	$phoneNumber = sanitiseData($_POST['phoneNumber']);
}
?>
```

Search the database to discover if the username entered by the user already exists.


> [!important] Make sure the table name matches your database (shown in blue) and the field matches as well (shown in red)
> 
![[userRegistrationSQLMatches.png]]

```php
$query = $conn->query("SELECT COUNT(*) FROM Customers WHERE EmailAddress='$username'");
```

Retrieve the count of usernames that match (if any).

Check if the `numberOfUsers` is greater than 0. This means that the username already exists.

```php
$data = $query->fetchArray();
$numberOfUsers = (int)$data[0];

if ($numberOfUsers > 0) {  // username already exists.
	echo "Sorry, that username already exists";
}
```

If `numberOfUsers` is 0 that means the username doesn’t already exist. Add an `else` clause and then hash the password.

![[userRegistrationCountUsers.png]]

```php
$hashedPassword = password_hash($password, PASSWORD_DEFAULT);
```

Create the SQL to insert the data into the database.

![[userRegistrationInsertUserData.png]]

```php
$sqlStmt = $conn->prepare("INSERT INTO Customers (EmailAddress, HashedPassword, FirstName, SecondName, Address, PhoneNumber) VALUES (:EmailAddress, :HashedPassword, :FirstName, :SecondName, :Address, :PhoneNumber)");
$sqlStmt->bindParam(':EmailAddress', $username);
$sqlStmt->bindParam(':HashedPassword', $hashedPassword);
$sqlStmt->bindParam(':FirstName', $firstName);
$sqlStmt->bindParam(':SecondName', $secondName);
$sqlStmt->bindParam(':Address', $address);
$sqlStmt->bindParam(':PhoneNumber', $phoneNumber);
$sqlStmt->execute();
```

Run the page in the browser and register an account! You can view the database to see if it works.

![[userRegistrationSuccess.png]]

![[commonBlocks#Commit & Push]]


# User Login

To handle the login process, a login script will be created, however this won’t be used to display any HTML. This will allow the login process to occur on any page that you wish in the future.


> [!info] This script needs to process 2 cases:
1. The user entered a correct username and password combination
2. The user did **not** enter a correct username and password combination.



## Login Script

Create a new file in the root of your project called `login.php`. 

![[userLoginNewFile.png]]

**Replace** the default contents of the file with the following

| Line  | Description                                                                                                                      |
| ----- | -------------------------------------------------------------------------------------------------------------------------------- |
| 2     | Identifies `$conn` as a variable declared in another script.                                                                     |
| 4     | Checks to see if `login` is set. This in effect checks to see if the user has pressed the **login** button (to be created later) |
| 5 & 6 | Collects and sanitises the data entered in the `username` and `password` text boxes.                                             |


![[userLoginSanitiseInput.png]]

```php
<?php
/**  @var $conn */

if (isset($_POST['login'])) {
	$username = sanitiseData($_POST['username']);
	$password = sanitiseData($_POST['password']);

?>
```

Run a SQL query on the database which will return a count of all the users with the username entered by the user - `COUNT(*) as count`. Including the `as count` means that you can later use ‘count’ later in the SQL (such as on line 11).


> [!info] Remember that the table and fields needs to match **your** database.



![[userLoginSQL.png]]

```php
$query = $conn->query("SELECT COUNT(*) as count, * FROM Customers WHERE `EmailAddress`='$username'");
$row = $query->fetchArray();
$count = $row['count'];
```

If `$count` is above 0, this means that the username exists in the database. This is an important first step to determine if the user can login. If the username isn’t found, you do not want the user to login!

![[userLoginRowCount.png]]

```php
if ($count > 0) {

}
```

After confirming the username exists, the next step is to compare the password entered by the user to the hashed version of the password in the database. PHP provides a helper function for this - `password_verify(cleartext password, hashed password)` . If this returns `true` then the passwords match.


> [!info] Ensure that the field name matches the database you’re using.



![[userLoginPasswordVerify.png]]

```php
if (password_verify($password, $row['HashedPassword'])) {

}
```

At this stage, the username is found and the password matches the one stored in the database. The user can now log in! 

To do this in PHP, set the **session variables** that are required for your project. The session variables can be anything that is needed/wanted to meet the projects goals. 

In this case, two session variables are being set - `FirstName` and `EmailAddress` - and they are being assigned the values taken from the SQL query above.

`header("location:index.php");` is the PHP code required to redirect the browser to a new page - i.e. after the user logs in, load the `index.php` page.


> [!info] So far, this script handles Case 1 from above.



![[userLoginAssignSessionVariables.png]]

```php
$_SESSION["FirstName"] = $row['FirstName'];
$_SESSION['EmailAddress'] = $row['EmailAddress'];
header("location:index.php");
```

Regardless of whether the username or password is incorrect, the output should be the same - invalid username or password. 

For **both** if statements shown, include an `else` block to output the error to the user.

![[userLoginFailedConditions.png]]

```php
else {
	echo "<div class='alert alert-danger'>Invalid username or password</div>";
	}
} else {
	echo "<div class='alert alert-danger'>Invalid username or password</div>";
}
```

![[commonBlocks#Commit & Push]]

# Index

As the site’s front page is underused at this stage of development, this page will be used to link with the login script. Eventually the front page will appear similar to this.

![[indexEndResult.png]]

Open `index.php.`

Alongside including `template.php`, also include `login.php` to utilise the script created in that file.

![[indexInclude.png]]

```php
<?php include 'login.php'; ?>
```

Change the heading to indicate the shop name or products to be sold.

Change the text between the `<h1>` and `</h1>` tags.

![[indexHeading.png]]

```html
<h1>Star Wars Shopfront</h1>
```

Using bootstraps container layout options, create a container that will resize based on the width of the window (`container-fluid`) with 1 row of equal sized columns.

In bootstrap, each container is divided into 12 ‘columns’. In the example shown, the two columns set up uses the class `col-6` meaning that the column will take up half the width of that container.

![[indexLayout.png]]

```html
<div class="container-fluid">
	<div class="row">
		<div class="col-6">
			Column 1
		</div>
		<div class="col-6">
			Column 2
		</div>
	</div>
</div>
```

At this stage, the page renders as such:

![[indexLayoutColumns.png]]

Replace **Column 1** with details of your user accounts. This will make it easier to remember the username and password in the future.


> [!info] Change the username and passwords to the account that you created.



```php
<p>username: ryan.cather@ed.act.edu.au</p>
<p>Password: password</p>
<p>username: admin@admin.com</p>
<p>Password: admin</p>
```

**Column 2** will hold the form for the login process. 

Initially, replace Column 2 with a PHP if statement to check to see if the user is already logged in. This is done by using `isset($_SESSION["EmailAddress"])` which returns `true` if the variable has been set. However, the code wants to check if that variable has **not** been set. The way to do that is to ‘flip’ the response from the function so that it changes the response to the opposite. 

i.e. 

`true` → `false`, and

`false` → `true`.

In effect, this if statements run the code in the brackets if the session variable hasn’t been set.

![[indexCheckIfLoggedIn.png]]

```php
<?php if (!isset($_SESSION["EmailAddress"])) : ?>

<?php endif; ?>
```

If the user is not logged in, display the login form.

You’ll notice that the button name is `login` and this must match the code in `login.php` where it checks:

```php
if (isset($_POST['login']))
```

So the `login` must match on both pages!

![[indexLoginButton.png]]

```php
<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="POST">
	<div class="form-group">
		<label>Username</label>
		<input type="text" name="username" class="form-control" required="required"/>
	</div>
	<div class="form-group">
		<label>Password</label>
		<input type="password" name="password" class="form-control" required="required"/>
	</div>

	<div class="text-center">
		<button name="login" class="btn btn-primary">Login</button>
	</div>
</form>
```

The home page of the site should now appear similar to this:

![[indexFinal.png]]


![[commonBlocks#Commit & Push]]

# Context Sensitive Navbar

At this stage of development, the navbar is standard across all pages (that include `template.php`), showing all the same links.

![[navStatic.png]]

With users now being able to login and out, it’s possible to utilise the session variables to change the options shown to the user. For instance, if the user is logged in, there’s no need for the **Register** link to be visible.

At this stage, depending on the users state (logged in or unregistered), the following links should be visible. 

| Anonymous | Logged in |
| --- | --- |
| Home (index) | Order Form |
| Contact Us | Invoice List |
| Register | Log Out |


> [!info] Later in development, an ‘administrator’ level user will be implemented which will have access to certain pages as well.



Open `template.php`.

First, to utilise the session variables in `template.php` add the following code block to the top of the file.

![[navStartSession.png]]

```php
<?php session_start(); ?>
```

Modify the list of nav bar items to check if the FirstName session variable is  set. If it is, then display the Order Form and Invoice List navbar items.

If the session varaible is not set, then show the Register navbar item.

![[navContextSensitiveLinks.png]]

```php
<?php
if (isset($_SESSION["FirstName"])) {
	echo '<li class="nav-item" ><a class="nav-link" href = "orderForm.php"> Order Form </a ></li >';
	echo '<li class="nav-item" ><a class="nav-link" href = "invoiceList.php"> Invoice list</a ></li >';
} else {
	echo '<li class="nav-item"><a class="nav-link" href="register.php">Register</a></li>';
}
?>
```

Finally, after the navbar items, on the left hand side of the navbar, add another check if the FirstName session variable is set, and if so, display a Welcome message, and the log out link.

![[navWelcome.png]]

```php
<?php
if (isset($_SESSION["FirstName"])) {
	echo '<div class="bg-light">Welcome, ' . $_SESSION["FirstName"] . '!<a class="nav-link" href="logout.php">Logout</a></div>';
}
?>
```

Once complete, the functionality should appear as shown.

![[navContextSensitive.gif]]

# User Logout

Create a new page - `logout.php`.

Luckily, this page doesn’t need much - all that is required is to clear all session variables defined by other pages (such as `login.php` etc). 

Replace the contents of `logout.php` with the code shown.

![[userLogoutNewPage.png]]

```php
<?php
session_start();
session_destroy();
header("Location:index.php");
?>

</body>
</html>
```

Try it out - load the site login, and then log out. Check the navbar to confirm that user is now logged out.

![[commonBlocks#Commit & Push]]
