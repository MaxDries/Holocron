---
tags: php
---

![[Theory#Algorithm Design - Loops]]




# PHP Implementation

There are four main loops in PHP:

## `while`

Iterates over a block of code **while the condition is `true`.** The condition is checked *before* the loop begins for the first time.

```php
<?php
$loopControlVariable = 1;
while($loopControlVariable <= 3) {
  echo "The number is: $loopControlVariable <br>";
  $loopControlVariable++;
}
?>
```

## `do..while`

 Iterates over a block of code **while the condition is `true`.** The condition is checked *after* the first time running the code. 

```php
 <?php
$loopControlVariable = 1;

do {
  echo "The number is: $loopControlVariable <br>";
  $loopControlVariable++;
} while ($loopControlVariable <= 10);
?>
```

## `for`
Iterates over a block of code for a set number of times. 

```php
<?php
for ($loopControlVariable = 0; $loopControlVariable <= 100; $loopControlVariable++) {
  echo "The number is: $loopControlVariable <br>";
}
?>
```

## `foreach`
Iterates over a block of code for each element in an array. 

```php
`<?php
$characters = array("Luke", "Leia", "Rey", "Finn");

foreach ($characters as $name) {
	echo "$name <br>";
}
?>
```

# Practical Exercises

> [!info] **Goal**
> 
> In this task, you’ll implement an invoice page which reads the data from `orders.csv`, looping over each line. To do this, you’ll first implement an order form which will collect the details from the user and save the data to a CSV file.


## Order Form

Create a new PHP page in the project called `orderForm.php`. 

![[loopsNewPHPOrderForm.png|Untitled]]

Replace the contents of the file with the following HTML form.

> [!info] In this form, currently, the user can only order from 5 different products. This is to make the programming of the page manageable at this stage.


This form is similar in structure to the contact form on `contact.php` however with more fields for the users details, and for the 5 products the user wishes to order.

![[loopsFormExample.png|Untitled]]

```php
<?php include "template.php" ?>
<title>Order Form</title>
<body>

<div class="container-fluid">
	<h1 class="text-primary">Order Form</h1>
	<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post">
		<div class="mb-3">
			<div class="row">
				<!--Customer Details-->

				<div class="col-md-6">
					<h2>Customer Details</h2>
					<p>Please enter your details:</p>
					<label for="customerNameFirst" class="form-label">First Name</label>
					<input class="form-control" id="customerNameFirst" name="customerNameFirst"
						   placeholder="...">
					<label for="customerNameSecond" class="form-label">Second Name</label>
					<input class="form-control" id="customerNameSecond" name="customerNameSecond"
						   placeholder="...">
					<label for="customerAddress" class="form-label">Address</label>
					<input class="form-control" id="customerAddress" name="customerAddress"
						   placeholder="...">
					<label for="customerPhone" class="form-label">Phone Number</label>
					<input class="form-control" id="customerPhone" name="customerPhone"
						   placeholder="...">
					<label for="customerEmail" class="form-label">Email Address</label>
					<input type="email" class="form-control" id="customerEmail" name="customerEmail"
						   placeholder="name@email.com">

				</div>
				<div class="col-md-6">
					<h2>Products</h2>
					<!--Product List-->
					<p>Please enter the quantities of each product:</p>
					<label for="orderProduct1" class="form-label">Product 1</label>
					<input type="number" class="form-control" id="orderProduct1" name="orderProduct1"
						   value="0">
					<label for="orderProduct2" class="form-label">Product 3</label>
					<input type="number" class="form-control" id="orderProduct2" name="orderProduct2"
						   value="0">
					<label for="orderProduct3" class="form-label">Product 3</label>
					<input type="number" class="form-control" id="orderProduct3" name="orderProduct3"
						   value="0">
					<label for="orderProduct4" class="form-label">Product 4</label>
					<input type="number" class="form-control" id="orderProduct4" name="orderProduct4"
						   value="0">
					<label for="orderProduct5" class="form-label">Product 5</label>
					<input type="number" class="form-control" id="orderProduct5" name="orderProduct5"
						   value="0">

				</div>
			</div>
		</div>
		<input type="submit" name="formSubmit" value="Submit">
	</form>
</div>

<?php echo footer() ?>
</body>
<script src="js/bootstrap.bundle.min.js"></script>
</html>
```

After the HTML form code, add in a PHP block which then checks if the user has pressed the submit button.

![[loopsIfPost.png|Untitled]]

```php
<?php

if ($_SERVER["REQUEST_METHOD"] == "POST") {

}
?>
```

 

Using `sanitiseData()`, collect and store the field data into various variables.

![[loopsFormCollectData.png|Untitled]]

```php
// Customer Details
$cusNameFirst = sanitiseData($_POST['customerNameFirst']);
$cusNameSecond = sanitiseData($_POST['customerNameSecond']);
$cusAddress = sanitiseData($_POST['customerAddress']);
$cusEmail = sanitiseData($_POST['customerEmail']);
$cusPhone = sanitiseData($_POST['customerPhone']);

// Product Quantities
$prodQuantity1 = sanitiseData($_POST['orderProduct1']);
$prodQuantity2 = sanitiseData($_POST['orderProduct2']);
$prodQuantity3 = sanitiseData($_POST['orderProduct3']);
$prodQuantity4 = sanitiseData($_POST['orderProduct4']);
$prodQuantity5 = sanitiseData($_POST['orderProduct5']);
```

Finally, save the collected data to `orders.csv`. 

![[loopFormSaveData.png|Untitled]]

```php
$csvFile = fopen("orders.csv", "a");
// Write the string to the end of the file.
fwrite($csvFile, $cusNameFirst . "," . $cusNameSecond . "," . $cusAddress . "," . $cusEmail . "," . $cusPhone . "," . $prodQuantity1 . "," . $prodQuantity2 . "," . $prodQuantity3 . "," . $prodQuantity4 . "," . $prodQuantity5 . "," . "\n");
// Close the connection to the file.
fclose($csvFile);
```

![[commonBlocks#Commit & Push]]

Run the page in the browser, and fill out the details. Press submit and `orders.csv` should be created, if not already there.

> [!info]  Don’t forget to add `orders.csv` to the git repository!


![[loopsCSVOutput.png|Untitled]]

## Invoice

> [!info] Initially, the invoice page will **only** display the last entry in `orders.csv`. It is possible to display all invoices, however that is for a later time.


Create a new page in the project called `invoice.php`. 

![[loopNewfileInvoice.png|Untitled]]

Create the code to display all the data that is going to be read and calculated.

This code displays the customer details and the products they ordered, the quantities and calculated totals.

```php
<?php include "template.php" ?>
<title>Invoice</title>
<body>

<?php

?>

<!--Customer Details-->
<h1 class="text-primary">Invoice</h1>
<div class="container-fluid">

	<div class="row">
		<div class="col-md-12">
			<h2 class="text-secondary">Customer Details</h2>
		</div>
	</div>
	<div class="row">
		<div class="col-md-6 text-primary">
			Customer Name
		</div>
		<div class="col-md-6 text-bg-light">
			<?= $cusNameFirst . " " . $cusNameSecond ?>
		</div>
		<div class="col-md-6 text-primary">
			Address
		</div>
		<div class="col-md-6 text-bg-light">
			<?= $cusAddress ?>
		</div>

		<div class="col-md-6 text-primary">
			Email
		</div>
		<div class="col-md-6 text-bg-light">
			<?= $cusEmail ?>
		</div>

		<div class="col-md-6 text-primary">
			Phone
		</div>
		<div class="col-md-6 text-bg-light">
			<?= $cusPhone ?>
		</div>
	</div>
</div>
<!--Products ordered -->
<div class="container-fluid">
	<div class="row">
		<div class="col-lg-12">
			<h2 class="text-secondary">Products Ordered</h2>
		</div>
	</div>

	<div class="row">
		<div class="col-lg-3">Product 1</div>
		<div class="col-lg-3">$<?= $prod1ItemCost ?></div>
		<div class="col-lg-3"><?= $prod1Quantity ?></div>
		<div class="col-lg-3">$<?= $prod1SubTotal ?></div>
	</div>
	<div class="row">
		<div class="col-lg-3">Product 2</div>
		<div class="col-lg-3">$<?= $prod2ItemCost ?></div>
		<div class="col-lg-3"><?= $prod2Quantity ?></div>
		<div class="col-lg-3">$<?= $prod2SubTotal ?></div>
	</div>
	<div class="row">
		<div class="col-lg-3">Product 3</div>
		<div class="col-lg-3">$<?= $prod3ItemCost ?></div>
		<div class="col-lg-3"><?= $prod3Quantity ?></div>
		<div class="col-lg-3">$<?= $prod3SubTotal ?></div>
	</div>
	<div class="row">
		<div class="col-lg-3">Product 4</div>
		<div class="col-lg-3">$<?= $prod4ItemCost ?></div>
		<div class="col-lg-3"><?= $prod4Quantity ?></div>
		<div class="col-lg-3">$<?= $prod4SubTotal ?></div>
	</div>
	<div class="row">
		<div class="col-lg-3">Product 5</div>
		<div class="col-lg-3">$<?= $prod5ItemCost ?></div>
		<div class="col-lg-3"><?= $prod5Quantity ?></div>
		<div class="col-lg-3">$<?= $prod5SubTotal ?></div>
	</div>
	<div class="row">
		<div class="col-lg-12">
			<h2 class="text-secondary text-sm-end">$<?= $invoiceTotal ?></h2>
		</div>
	</div>
</div>

<?php echo footer() ?>
</body>
<script src="js/bootstrap.bundle.min.js"></script>
</html>
```

In the empty PHP block near the top of the page, create an `if` statement which will attempt to open `orders.csv` and will only continue if there are no errors.

![[loopOpenCSV.png|Untitled]]

```php
// Read the contents of the file
$currentRow = 1;
if (($handle = fopen("orders.csv", "r")) !== FALSE) {
```

Using a `while` loop, read the contents of each line of the `orders.csv` 

![[loopWhileLoop.png|Untitled]]

```php
while (($data = fgetcsv($handle, 1000, ",")) !== FALSE) {

}
```

The while loop will iterate as long as there are lines of text in `orders.csv`. 

Each time the loop iterates it will read the current line of the file and store the information in `$data`. 

`$data` is an array. This type of data structure may not have been covered in the course as yet.

 `$data` in this case is divided into 10 different variables, each with an identifier (starting with 0). Each variable can be accessed by using the identifier, for example `$data[2]` is `123 Fake St`, as can be seen in the image below.

![[loopCSVStructure.png|Untitled]]

![[loopCSVReadData.png|Untitled]]

```php
$numberOfRowsOfData = count($data);
$currentRow++; //Add one to the current row

// Customer Details
$cusNameFirst = $data[0];
$cusNameSecond = $data[1];
$cusAddress = $data[2];
$cusEmail = $data[3];
$cusPhone = $data[4];

// Product Quantities
$prod1Quantity = $data[5];
$prod2Quantity = $data[6];
$prod3Quantity = $data[7];
$prod4Quantity = $data[8];
$prod5Quantity = $data[9];
```

Calculate all the totals, but first define the cost of each item.


> [!info] Defining the costs of the items in the code limits the scalability of the code as the site will need to be modified if prices change etc. However in this example it can be quickly set up, and later this will be modified to load prices from another source.


```php
$prod1ItemCost = 3.4;
$prod2ItemCost = 5.0;
$prod3ItemCost = 12.54;
$prod4ItemCost = 19.77;
$prod5ItemCost = 1.01;

$prod1SubTotal = $prod1Quantity * $prod1ItemCost;
$prod2SubTotal = $prod2Quantity * $prod2ItemCost;
$prod3SubTotal = $prod3Quantity * $prod3ItemCost;
$prod4SubTotal = $prod4Quantity * $prod4ItemCost;
$prod5SubTotal = $prod5Quantity * $prod5ItemCost;
$invoiceTotal = $prod1SubTotal + $prod2SubTotal + $prod3SubTotal + $prod4SubTotal + $prod5SubTotal;
```

![[commonBlocks#Commit & Push]]

## Navbar Updates

Add links to the Order form and invoice pages in the navbar in `template.php`. 

![[loopNavbarUpdates.png|Untitled]]

```html
<li class="nav-item">
	<a class="nav-link" href="orderForm.php">Order Form</a>
</li>
 <li class="nav-item">
	<a class="nav-link" href="invoice.php">Invoice</a>
</li>
```

![[commonBlocks#Commit & Push]]

## Test the pages

Load the pages in the browser, and they should appear similar to these.

![[loopFormTest.png|loopFormTest.png]]

![[loopInvoiceTest.png|loopInvoiceTest.png]]

# Review
