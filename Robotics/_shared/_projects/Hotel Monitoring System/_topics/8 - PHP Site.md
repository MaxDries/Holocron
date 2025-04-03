

# PHP Site

PHP is a **server side** scripting language. This means that PHP code runs on the server (not the client/browser) and delivers the necessary code to the client to display. 

For example, the server loads user information from the database and checks the username and password. If the password is correct, the browser is to display a message “login successful”. 

PHP co-exists with HTML and other Web Technologies. Unlike standard HTML, PHP requires a server to be running to process the PHP code.

PHP was created in 1994, so in terms of Programming Languages it is quite old, but it is very powerful and widespread.

PHP is used by itself but is also the main language in many other frameworks available, such as Wordpress and Laravel.

## Bootstrap

[Download Bootstrap](https://getbootstrap.com/docs/5.3/getting-started/download/) and unzip the folder.

Copy the CSS and JS folder into the project.

![[phpAddBootstrap.png]]

## Site Configuration

Create a new php file in the project, named `config.php`. 

![[phpAddNewFile.png]]

![[phpConfigSave.png]]

Replace the contents with the code shown.

This page will not be shown to the user at any stage, however contains all the details that are required to connect to the database. 

```php
<?php
session_start();
$servername = "10.177.200.71";
$username = "JEDI2023";
$password = "JEDI2023";
$dbname = "JEDI2023";
$errorCaught = false;

try {
	$conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
	$conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
	$errorCaught = true;
	$_SESSION['flash_message'] = "<div class='bg-danger'>The Database cannot be found: " . $servername . ". ".$e."</div>";
}
if (!$errorCaught) {
	//echo "Database connection configured correctly, and database connection good.";
}

//$conn = null;
?>
```

## Site Template

Create a new file `template.php`.

This php page will be used by all other PHP pages that are created for the website to offer a similar interface. 

![[phpTemplateView.png]]

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
		<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
				aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
			<span class="navbar-toggler-icon"></span>
		</button>
		<div class="collapse navbar-collapse" id="navbarNav">
			<ul class="navbar-nav">
				<li class="nav-item"><a class="nav-link" href="index.php">Home</a></li>
				<li class="nav-item"><a class="nav-link" href="moduleRegister.php">Register</a></li>
				<li class="nav-item"><a class="nav-link" href="moduleData.php">Module Data</a></li>
			</ul>
		</div>
		<?php
		if (isset($_SESSION["username"])) {
			echo "<div class='alert alert-success d-flex'><span>Welcome, " . $_SESSION["username"] . "<br><a href='logout.php'>Logout</a></span></div>";
		}
		?>
	</div>
</nav>
<?php
if (isset($_SESSION['flash_message'])) {
	$message = $_SESSION['flash_message'];
	unset($_SESSION['flash_message']);
	?>
	<div class="position-absolute bottom-0 end-0">
		<?= $message ?>

	</div>

	<?php
}
?>
<script src="js/bootstrap.bundle.js"></script>
<?php
function sanitise_data($data)
{
	$data = trim($data);
	$data = stripslashes($data);
	$data = htmlspecialchars($data);
	return $data;
}

?>
```

## Registering Modules

Create a new PHP file - `moduleRegister.php` - and replace the contents with the code shown.

The code will create a new entry in the database table 

```php
<?php include "template.php";
/** @var $conn */

?>

<title>Module Register</title>

<h1 class='text-primary'>Please register a module</h1>
<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post" enctype="multipart/form-data">
	<div class="container-fluid">

			<form>

				<div class="form-group">
					<label for="inputActuator">Actuator</label>
					<input type="text" class="form-control" name="inputActuator" id="inputActuator" aria-describedby="actuatorHelp" placeholder="Enter your module actuator.">
					<small id="actuatorHelp" class="form-text text-muted">This will be the output device (LED etc).</small>
				</div>
				<div class="form-group">
					<label for="inputCommand">Command</label>
					<input type="text" class="form-control" name="inputCommand" id="inputCommand" aria-describedby="commandHelp" placeholder="Enter starting command">
					<small id="commandHelp" class="form-text text-muted">This will be sent to the ESP32. Start simple, such as 0 or 1. </small>
				</div>
				<div class="form-group">
					<label for="inputPassword">Password</label>
					<input type="password" class="form-control" name="inputPassword" id="inputPassword" placeholder="Password">
				</div>

				<button type="submit" class="btn btn-primary">Submit</button>
			</form>
	</div>
</form>

<?php

if ($_SERVER["REQUEST_METHOD"] == "POST") {
	$password = sanitise_data($_POST['inputPassword']);
	$actuator = sanitise_data($_POST['inputActuator']);
	$command = sanitise_data($_POST['inputCommand']);
	$hashed_password = password_hash($password, PASSWORD_DEFAULT);

// check username in database
	$query = $conn->query("SELECT COUNT(*) FROM moduleCommands WHERE actuator='$actuator'");
	$data = $query->fetch();
	$numberOfActuatorsWithThatName = (int)$data[0];

	if ($numberOfActuatorsWithThatName > 0) {
		$_SESSION['flash_message'] = "This actuator name has already been taken.";
	} else {
		$sql = "INSERT INTO moduleCommands (actuator, command, hashedPassword) VALUES (:newActuator, :newCommand, :newPassword)";
		$stmt = $conn->prepare($sql);
		$stmt->bindValue(':newActuator', $actuator);
		$stmt->bindValue(':newCommand', $command);
		$stmt->bindValue(':newPassword', $hashed_password);
		$stmt->execute();
		$_SESSION["flash_message"] = "Module Created";
		header("Location:index.php");

	}

}
?>
```

## Testing

Load the page in the browser and it should appear similar to the one shown. Enter some data and press submit.

![[phpViewForm.png]]

Open the database table view in PHPstorm by opening the database tab and then double-clicking on `moduleCommands`. 

Ideally, if the files are all programmed correctly, the new module data should appear.

![[phpDbRecord.png]]

## JSON

JSON is a data format that is similar to a dictionary or key-value pair structure.

![[phpJsonExample.png]]

JSON, which stands for "JavaScript Object Notation," is like a special language that helps you organise and describe this data in a way that computers can easily understand.

Think of JSON as a recipe for computers. Just like a recipe lists ingredients and instructions for making a yummy dish, JSON lists pieces of information and how they relate to each other. These pieces of information are called "objects." Each object is like a container that holds different types of data, like text, numbers, or even more objects.

JSON is simple and easy to read, both for humans and computers. It's kind of like writing a story with different characters and their attributes. For example, if you're describing a person, you might use JSON like this:

```json
{
  "name": "Alice",
  "age": 16,
  "city": "New York",
  "hobbies": ["reading", "painting", "playing guitar"]
}

```

Here, we have an object that describes Alice. It includes her name, age, city, and hobbies. Her hobbies are stored as a list of activities.

This JSON "recipe" can be shared with other programs or even other people, and they can understand how the data is structured and use it for different purposes. It's like passing around the recipe for a cake so that anyone can bake the same delicious cake!

So, JSON is a way for computers to organise and talk about different pieces of information, just like you would when telling a story or following a recipe.

### Update the Arduino Code to use `ArduinoJSON` Library.

In your project, open the `platformio.ini` file and add the following to the bottom of the list of libraries - `bblanchon/ArduinoJson@^6.21.3`

This will allow the PHP site to return a JSON object to the ESP32 and have it extract the data.

Save the file. The project may have to load the library, which may take a minute or two.

![[phpArduinoJsonLibrary.png]]

Open `main.cpp`. Add the following include directive to load the library.

```cpp
#include "ArduinoJson.h"
```

![[phpArduinoJsonLibraryInclude.png]]

The `dataTransfer()` function currently uploads data and receives a response, however the code doesn’t use that response at this stage. To change this, go do the `loop()` function and add `String payload` to the start of the dataTransfer function call.

![[phpArduinoPayload.png]]

Add the code shown to the end of `loop()` to extract and output the command data. This command variable will be sent back by the PHP server in JSON format, extracted from the database table.

```cpp
Serial.print("Payload from server:");
Serial.println(payload);
DynamicJsonDocument doc(1024);
//  Serial.println(deserializeJson(doc, payload));
DeserializationError error = deserializeJson(doc, payload);
if (error)
{
  Serial.print(F("deserializeJson() failed: "));
  Serial.println(error.f_str());
  return;
}
const char *command = doc["command"];
Serial.print("Command: ");
Serial.print(command);
```

![[commonBlocks#Commit & Push]]
