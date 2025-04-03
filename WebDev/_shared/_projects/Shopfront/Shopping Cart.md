

# Shopping Cart


> [!info] Prerequisite: The Order Form has been completed.



Create a new file in the root directory of the project called `cart.php`. 

This page will be developed to load the contents of the `ShoppingCart` session variable and display the relevant details.

The page will also dynamically generate the sub totals for each product and the invoice total. When the user completes the order, the details are stored in the Order table.

![[cartNewFile.png]]

Replace the contents of the file with the code shown.

```php
<?php include "template.php";
/**
 * Shopping Cart.
 * Displays (and allows edits) of the items that the user has entered into their cart.
 * On submit, writes it to the orderDetails table.
 * Additionally, updates messaging table to send message to admin to indicates order has been made.
 *
 * "Defines" the conn variable, removing the undefined variable errors.
 * @var $conn
 */
?>
<title>Cart</title>

<link rel="stylesheet" href="css/orderForm.css">

<?php
// Debug Purposes
 //echo '<pre>'; print_r($_SESSION["ShoppingCart"]); echo '</pre>';

if (isset($_SESSION["FirstName"])) {
date_default_timezone_set("Australia/Sydney");
$status = "";
if (isset($_POST['action']) && $_POST['action'] == "remove") {
	if (!empty($_SESSION["ShoppingCart"])) {
		foreach ($_SESSION["ShoppingCart"] as $key => $value) {
			if ($_POST["code"] == $key) {
				unset($_SESSION["ShoppingCart"][$key]);
				$status = "<div class='box' style='color:red;'>Product is removed from your cart!</div>";
			}
			if (empty($_SESSION["ShoppingCart"]))
				unset($_SESSION["ShoppingCart"]);
		}
	}
}

//This code runs when the quantity changes for a row
if (isset($_POST['action']) && $_POST['action'] == "change") {
	foreach ($_SESSION["ShoppingCart"] as &$value) {
		if ($value['code'] === $_POST["code"]) {
			$value['quantity'] = $_POST["quantity"];
			break; // Stop the loop after we've found the product
		}
	}
}
?>

<div class="message_box" style="margin:10px 0px;">
	<?php echo $status; ?>
</div>

<div class="cart">
	<?php
	if (isset($_SESSION["ShoppingCart"])) {
	$total_price = 0;
	?>
	<div class="container-fluid">
		<div class="row">
			<div class="col-2"></div>
			<div class="col-2">ITEM NAME</div>
			<div class="col-2">QUANTITY</div>
			<div class="col-2">UNIT PRICE</div>
			<div class="col-2">ITEMS TOTAL</div>
		</div>
		<?php
		foreach ($_SESSION["ShoppingCart"] as $product) {
			?>
			<div class="row">
				<div class="col-2">
					<img src='images/productImages/<?php echo $product["image"]; ?>' width="50" height="40"/>
				</div>
				<div class="col-2"><?php echo $product["productName"]; ?> <br>
					<form method='post' action=''>
					<input type='hidden' name='code' value="<?php echo $product["code"]; ?>"/>
					<input type='hidden' name='action' value="remove"/>
					<button type='submit' class='remove'>Remove Item</button>
					</form>
				</div>
				<div class="col-2">
					<form method='post' action=''>
						<input type='hidden' name='code' value="<?php echo $product["code"]; ?>"/>
						<input type='hidden' name='action' value="change"/>
						<select name='quantity' class='quantity' onChange="this.form.submit()">
							<option <?php if ($product["quantity"] == 1) echo "selected"; ?>
									value="1">1
							</option>
							<option <?php if ($product["quantity"] == 2) echo "selected"; ?>
									value="2">2
							</option>
							<option <?php if ($product["quantity"] == 3) echo "selected"; ?>
									value="3">3
							</option>
							<option <?php if ($product["quantity"] == 4) echo "selected"; ?>
									value="4">4
							</option>
							<option <?php if ($product["quantity"] == 5) echo "selected"; ?>
									value="5">5
							</option>
						</select>
					</form>
				</div>
				<div class="col-2">
					<!--Individual product price-->
					<?php echo "$" . $product["price"]; ?>
				</div>
				<div class="col-2">
					<!-- Subtotal for product-->
					<?php echo "$" . $product["price"] * $product["quantity"]; ?>
				</div>
			</div>
			<?php
			$total_price += ($product["price"] * $product["quantity"]);
		}
		?>
		<div class="row">
		<div class="col-12" align="right">
		<div class="badge bg-primary text-wrap fs-5">TOTAL: <?php echo "$". $total_price; ?></div>
</div>
</div>
	</div>
<form method="post">
			<input type="submit" name="orderProducts" value="Order Now"/>
		</form>
	<?php
}

	if(isset($_POST['orderProducts'])) {
		// Writing the order to the database
		$orderNumber = "ORDER" . substr(md5(uniqid(mt_rand(), true)), 0, 8);
		foreach ($_SESSION["ShoppingCart"] as $row) {
				$customerID = $_SESSION["CustomerID"];
				$productID = $row['id'];
				$quantity = $row['quantity'];
				$orderDate = date("Y-m-d h:i:sa");

				// Write to the Db.
				$conn->exec("INSERT INTO Orders (OrderNumber,CustomerID, ProductID, OrderDate, quantity) VALUES('$orderNumber','$customerID','$productID','$orderDate', '$quantity')");

			}
		$_SESSION["ShoppingCart"] = [];

	}
} else {
	header("Location:index.php");
}

?>
```

