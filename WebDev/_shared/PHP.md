# Introduction to PHP

PHP (Hypertext Preprocessor) is a popular open-source, server-side scripting language widely used for developing dynamic and interactive web applications. It integrates seamlessly with HTML and can be embedded into web pages to execute specific tasks.

**Uses of PHP**

PHP is versatile and can be used for a wide range of web development applications, including:

- **Dynamic web pages:** Creating interactive websites that respond to user input and database queries.
- **E-commerce websites:** Building online stores with shopping carts, payment gateways, and product management features.
- **Content management systems (CMS):** Developing platforms for managing website content, such as WordPress and Drupal.
- **Social networking websites:** Creating platforms for user interaction, content sharing, and networking.
- **Web services:** Exposing data and functionalities to other applications through APIs.
- **Data processing and analysis:** Using PHP to handle large data sets, perform calculations, and generate reports.

**Examples of PHP**

**1. Displaying "Hello World!"**

```php
<?php
echo "Hello World!";
?>
```

**2. Conditional Statements**

```php
<?php
$age = 18;

if ($age >= 18) {
  echo "You are an adult.";
} else {
  echo "You are a child.";
}
?>
```

**3. Looping**

```php
<?php
$numbers = array(1, 2, 3, 4, 5);

foreach ($numbers as $number) {
  echo "$number ";
}
?>
```

**4. Database Connection**

```php
<?php
$servername = "localhost";
$username = "root";
$password = "password";
$dbname = "my_database";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

echo "Connected to database.";
?>
```

**5. Form Handling**

```php
<?php
if (isset($_POST['submit'])) {
  $name = $_POST['name'];
  $email = $_POST['email'];

  // Process the form data here...
}
?>

<form action="form.php" method="post">
  <input type="text" name="name" placeholder="Name">
  <input type="email" name="email" placeholder="Email">
  <input type="submit" name="submit" value="Submit">
</form>
```

**Additional Features of PHP**

- **Object-oriented programming:** Support for classes, objects, and inheritance.
- **Error and exception handling:** Robust mechanisms for handling errors and exceptions.
- **Database connectivity:** Easy integration with various database systems, such as MySQL, PostgreSQL, and MongoDB.
- **Caching:** Support for various caching mechanisms to improve application performance.
- **Security:** Features for enhancing the security of web applications, including encryption and input validation.

# Interacting with Databases Using SQL in PHP

PHP provides a powerful set of functions for interacting with databases using SQL (Structured Query Language). Here are some common tasks and their corresponding PHP functions:

**Connecting to a Database**

```php
$servername = "localhost";
$username = "root";
$password = "password";
$dbname = "my_database";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}
```

**Executing a Query**

```php
$sql = "SELECT * FROM users";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
  // Process the result set here...
} else {
  echo "No results found.";
}
```

**Inserting Data**

```php
$sql = "INSERT INTO users (name, email) VALUES ('John Doe', 'john.doe@example.com')";

if ($conn->query($sql) === TRUE) {
  echo "New record created successfully.";
} else {
  echo "Error: " . $conn->error;
}
```

**Updating Data**

```php
$sql = "UPDATE users SET name='Jane Doe' WHERE id=1";

if ($conn->query($sql) === TRUE) {
  echo "Record updated successfully.";
} else {
  echo "Error: " . $conn->error;
}
```

**Deleting Data**

```php
$sql = "DELETE FROM users WHERE id=1";

if ($conn->query($sql) === TRUE) {
  echo "Record deleted successfully.";
} else {
  echo "Error: " . $conn->error;
}
```

**Closing the Connection**

```php
$conn->close();
```

**Additional Features**

- **Prepared Statements:** Prevent SQL injection attacks by using parameterized queries.
- **Transactions:** Group multiple SQL queries into a single unit of work, ensuring data integrity.
- **Database Abstraction Layers (DALs):** Provide a consistent interface for interacting with different database systems.

**Example**

The following PHP code demonstrates how to connect to a database, execute a query, and display the results:

```php
<?php
$servername = "localhost";
$username = "root";
$password = "password";
$dbname = "my_database";

// Connect to the database
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

// Execute a query
$sql = "SELECT * FROM users";
$result = $conn->query($sql);

// Process the result set
if ($result->num_rows > 0) {
  while ($row = $result->fetch_assoc()) {
    echo "ID: " . $row["id"] . ", Name: " . $row["name"] . ", Email: " . $row["email"] . "<br>";
  }
} else {
  echo "No results found.";
}

// Close the connection
$conn->close();
?>
```

This code connects to a database named "my_database" and executes a query to select all rows from the "users" table. It then iterates through the result set and displays the data for each row.
