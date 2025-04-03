
## Background Information

### PHP

PHP stands for **P**HP **H**ypertext **P**reProcessor and is an open-source, server-side scripting language. It is used to develop dynamic web applications, as well as create interactive and responsive websites. Due to its versatility, it has become one of the most widely-used programming languages on the web. It is well-known for its easy syntax, allowing both novice and experienced coders to write web applications quickly and effectively. In essence, PHP is an invaluable tool for web developers, providing them with the power to create robust, dynamic and interactive web applications.

In this project, you will be developing PHP page/s to interface between the Adafruit Feather and the MySQL database.

### MySQL

MySQL is a powerful and widely used **database management system**, designed to store and manage large amounts of data in an efficient and secure manner. It provides a range of features and tools that allows users to create, manage and maintain databases with ease, making it an ideal choice for businesses and organisations of all sizes. As it is open-source, it is highly customisable, allowing users to tailor the system to their specific needs and requirements. With its robust security and scalability, MySQL is an excellent choice for managing large-scale databases. 

You will be given a login to the database on the server. 

This login is shared by all users of the system, so be careful if you change anything!!

### CRUD

![[WebDev/_shared/_projects/Ngunnawal/_images/CRUD.jpeg|[https://www.atatus.com/glossary/crud/]]

[https://www.atatus.com/glossary/crud/](https://www.atatus.com/glossary/crud/)

**Create**, **Read**, **Update** & **Delete** (CRUD) are the standard operations for use with databases, especially with SQL statements using a relational data system such as SQLite or MySQL. 

You can read more on CRUD by accessing [this page](https://www.humio.com/glossary/crud/).

## System Overview

[https://docs.google.com/drawings/d/e/2PACX-1vQghkYq2rMikuPfnL4HrByHcnjMhT4yYoMRNa9HB2P7C-2QKNloAkr7s8ITbW-b6ZSIfvbpKvVkrOaT/pub?w=1440&amp;h=1080](https://docs.google.com/drawings/d/e/2PACX-1vQghkYq2rMikuPfnL4HrByHcnjMhT4yYoMRNa9HB2P7C-2QKNloAkr7s8ITbW-b6ZSIfvbpKvVkrOaT/pub?w=1440&amp;h=1080)

## Connect PHPStorm to the database

Open the project in PHPStorm and open the database tab.



Click the + icon to add a new database source. Select MariaDB from the dropdown list.



Enter the IP Address of the database server in the Host text box.

Enter the Username and password.

If the message appears to Download or upgrade drivers, Click that.

Click Test Connection.

Click Ok.



Once the connection has been established, click on the `1 of 3` status next to the database connection and then select **All Schemas**. 

![[week14ContactDemo.png|Untitled]]

You should then be able to view the databases associated with your logon.

## `config.php`

In order to manage the connection to the MySQL database, it makes sense to keep the details in one single location rather than in various files across the system.

Create a new PHP file called `config.php` in the root of the project.

![[phpNewConfig.png]]

Copy this code into config.php.

You will need to change the values of the following variables:

`servername`

`username`

`password`

`dbname`

You will be given these details in class.

![[phpConfigCode.png]]

```php
<?php
session_start();

$servername = "10.76.43.63";
$username = "RC";
$password = "RC";
$dbname = "RC";
$errorCaught = false;

try {
	$conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
	$conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
	$errorCaught = true;
	echo "Error: " . $e->getMessage();
}

if (!$errorCaught) {
	echo "Database connection configured correctly, and database connection good.";
}

$conn = null;
?>
```

## `template.php`

`template.php` is the file that contains the HTML, PHP and any other information that is required for all pages throughout the site. 

This page will contain the code for the loading of `config.php`, the navigation bar, bootstrap etc.

Create a new PHP file called `template.php`. Replace the contents with this code.

The first line of this file loads the config.php code created earlier to establish the connection to the database.

```php
<?php require_once 'config.php'; ?>
<html>
<head>
	<!-- Required meta tags -->
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

	<!-- Bootstrap CSS -->
	<link rel="stylesheet" href="css/bootstrap.min.css">
</head>
<body>
<!-- Navigation Bar -->
<nav class="navbar navbar-expand-sm navbar-light bg-light">
	<div class="container-fluid">
		<a class="navbar-brand" href="index.php">
			<img src="images/logo.png" alt="" width="80" height="80">
		</a>
		<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
				aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
			<span class="navbar-toggler-icon"></span>
		</button>
		<div class="collapse navbar-collapse" id="navbarNav">
			<ul class="navbar-nav">
				<li class="nav-item">
					<a class="nav-link" href="index.php">Home</a>
				</li>
			</ul>
			<?php if (isset($_SESSION["name"])) {
				echo "<div class='alert alert-success d-flex'><span>Welcome, " . $_SESSION["name"] . "<br><a href='logout.php'>Logout</a></span></div>";
			} else {
				echo "<div class='alert alert-info d-flex'><a href='index.php'>Sign In</a>";
			}
			?>
		</div>
	</div>
</nav>

<script src="js/bootstrap.bundle.js"></script>

<?php
function sanitise_data($data)
{
	$data = trim($data);
	$data = stripslashes($data);
	$data = htmlspecialchars($data);
	return $data;
}

function outputFooter()
{
	date_default_timezone_set('Australia/Canberra');
	echo "<footer>This page was last modified: " . date("F d Y H:i:s.", filemtime("index.php")) . "</footer>";
}

?>
```

If you launch the page in the browser, you’ll see something like this.

![[phpOutput.png]]

### Explanation

The template page is complex because it covers a number of base tasks.

| 1 | Loads and executes code in `config.php` |
| --- | --- |
| 2 | Loads and enables access to the bootstrap CSS library (more later) |
| 3 | Bootstrap Navbar code. Currently only showing the link to the homepage. |
| 4 | Dynamic content in the navbar - if the user is not logged in the navbar shows a link to log in. If already logged in, the navbar welcomes that user. |
| 5 | Loads and enables access to the bootstrap javascript library. Primarily used for the automatic dropdown link for small screens. |
| 6 | Site-wide function to sanitise data. More later. |
| 7 | Site-wide function to output a standardised footer across pages. |

![[phpCodeExplain.png]]

## `index.php`

The Index page will be the homepage that the users see when they first access your section of the site.

Create a new PHP file called `index.php`. Replace the contents with this.

```php
<?php include "template.php"; ?>
<title>Cyber City</title>

<h1 class='text-primary'>Welcome to our The Cyber City</h1>
```

Launch the page and you should see something similar to this:

![[phpHeader.png]]

## `registration.php`

The form code

```php
<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post" enctype="multipart/form-data">
	<div class="container-fluid">
		<div class="row">
			<!--Customer Details-->

			<div class="col-md-12">
				<h2>Account Details</h2>
				<p>Please enter wanted username and password:</p>
				<p>User Name<input type="text" name="username" class="form-control" required="required"></p>
				<p>Password<input type="password" name="password" class="form-control" required="required"></p>

			</div>
		</div>
	</div>
	<input type="submit" name="formSubmit" value="Submit">
</form>
```

```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
	$username = sanitise_data($_POST['username']);
	$password = sanitise_data($_POST['password']);
	$hashed_password = password_hash($password, PASSWORD_DEFAULT);
	//echo $username;
	//echo $hashed_password;

$sql = "INSERT INTO user (username, hashed_password, access_level) VALUES (:newUsername, :newPassword, 1)";
	$stmt = $conn->prepare($sql);
	$stmt->bindValue(':newUsername', $username);
	$stmt->bindValue(':newPassword', $hashed_password);
	$stmt->execute();
}
?>
```

## `login.php`

The login page will visually be very similar to the registration page, at this stage. The first part of the page can just be duplicated from `registration.php`. 

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 12.png|Untitled]]

```php
<?php include "template.php"; ?>
<title>Cyber City - Login</title>

<h1 class='text-primary'>Login</h1>

<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post" enctype="multipart/form-data">
	<div class="container-fluid">
		<div class="row">
			<!--Customer Details-->

			<div class="col-md-12">
				<h2>Account Details</h2>
				<p>Please enter wanted username and password:</p>
				<p>User Name<input type="text" name="username" class="form-control" required="required"></p>
				<p>Password<input type="password" name="password" class="form-control" required="required"></p>

			</div>
		</div>
	</div>
	<input type="submit" name="formSubmit" value="Submit">
</form>
```

And again, the first part of the PHP code from registration will be the same, namely collecting and sanitising the form data and searching for how many users of that user name are found in the database.

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 13.png|Untitled]]

```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
	$username = sanitise_data($_POST['username']);
	$password = sanitise_data($_POST['password']);

	$query = $conn->query("SELECT COUNT(*) as count FROM `user` WHERE `username`='$username'");
	$row = $query->fetch();
	$count = $row[0];

		if ($count > 0) {

		}

}
```

If a user has be found (i.e. if `$count > 0`) then the code will want to load all the details of that users record.


> [!important] A **record** in a database is a collection of data which is organised into fields and is stored within a table. Each record contains values which correspond to the fields in the table. These records are used to store and retrieve information, such as a user's name, address, and other personal data. Records are usually identified by a unique key or identifier, such as an ID number.
> 
> ![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 14.png|Untitled]]



```php
$query = $conn->query("SELECT * FROM `user` WHERE `username`='$username'");
$row = $query->fetch();
```

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 15.png|Untitled]]

---

When this code is executed, `$row` will contain the record as an array and each field (or column) will be stored in the different elements in order. 

So, in this specific case, the fields will equate to these *indexes*.

![[decisionsSonarData.png|Untitled]]

---

At this stage, the username is correct and valid, but the system still needs to confirm the password, which is hashed in the database. Luckily PHP has a helper function to compare a password in plain text with the hashed password. This function - `password_verify()` - will return `true` if the passwords match and `false` if not. 


>[!info] Note that the clear text password is being compared to `row[2]` which is the hashed_password field from the database.

![[decisionsDetermineLEDBrightness.png|Untitled]]

```php
if (password_verify($password, $row[2])) {
}
```

If `password_verify()` returns true then the user has successfully logged on. Before proceeding with any other part of the process, you need to set some **session** variables.


> [!info] Session variables in PHP are variables that store user information for a specific session. They are stored in the server's memory for a certain amount of time and are used to track user activities during that session. Session variables can be used to store user-specific data, such as their name, preferences, or cart contents. They allow a website to remember user-specific data across multiple pages and even after a user has left the website and returned.


For the purposes of this project, the only sessions variables needed (at this stage) are the `user_id`, `username` and `access_level`.

![[dataArrayOutput.png|Untitled]]

```php
$_SESSION["user_id"] = $row[0];
$_SESSION["username"] = $row[1];
$_SESSION['access_level'] = $row[3];
```

Finally, add a catch for if the logon was unsuccessful.

![[dataArrayLength.png|Untitled]]

```php
else {
	// unsuccessful log on.
	echo "<div class='alert alert-danger'>Invalid username or password</div>";
}
```

Try it out!
![[commonBlocks#Commit & Push]]

## `logout.php`

Create a new page named `logout.php`. All this page needs to do at this stage is to clear all session variables from memory so that the user is now no longer logged in.

![[dataKeyValue.png|Untitled]]

```php
<?php
session_start();
session_destroy();
?>
```

The final line of code for the log out process should be to redirect the browser to another page.

![[dataArrayIteration.png|Untitled]]

```php
header("Location:index.php");
```

## Template Update

Open `template.php` as a few changes need to be made due to a change in the project. 

In the nav bar, there are references to the session variable name, however this has not been set in the project so far. Change both instances of `name` to `username`. 

![[dataArrayDefinition.png|Untitled]]

Save and reload the login page in the browser and the navbar should now display the successfully logged in users username.

![[dataOrderFormOutput.png|Untitled]]

Update the link for Sign in `template.php` to link to the correct page for login. 

![[dataOrderForm.png|Untitled]]
