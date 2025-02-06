
# Order Form


> [!info] If there is already an `orderForm.php` page in your project, you can either delete all the contents, or rename the file to `orderform.old` or similar.



## Order Form CSS

Before creating the order form page, create a new **StyleSheet** file in the CSS folder. Call the file `orderForm.css`.

Copy replace the contents with the code shown.

![[orderFormCSS.png]]

```css
.product_wrapper {
	float:left;
	padding: 30px;
	text-align: center;
}
.product_wrapper:hover {
	box-shadow: 0 0 0 2px #e5e5e5;
	cursor:pointer;
}
.product_wrapper .name {
	font-weight:bold;
}
.product_wrapper .buy {
	text-transform: uppercase;
	background: #F68B1E;
	border: 1px solid #F68B1E;
	cursor: pointer;
	color: #fff;
	padding: 8px 40px;
	margin-top: 10px;
}
.product_wrapper .buy:hover {
	background: #f17e0a;
	border-color: #f17e0a;
}
.message_box .box{
	margin: 10px 0px;
	border: 1px solid #2b772e;
	text-align: center;
	font-weight: bold;
	color: #2b772e;
}
.table td {
	border-bottom: #F0F0F0 1px solid;
	padding: 10px;
}
.cart_div {
	float:right;
	font-weight:bold;
	position:relative;
	padding-right: 30px;
}
.cart_div a {
	color:#000;
}
.cart_div span {
	font-size: 12px;
	line-height: 14px;
	background: #F68B1E;
	padding: 2px;
	border: 2px solid #fff;
	border-radius: 50%;
	position: absolute;
	top: -1px;
	left: 13px;
	color: #fff;
	width: 20px;
	height: 20px;
	text-align: center;
}
.cart .remove {
	background: none;
	border: none;
	color: #0067ab;
	cursor: pointer;
	padding: 0px;
}
.cart .remove:hover {
	text-decoration:underline;
}
```

## Order Form PHP

Create a new PHP page in the project called `orderForm.php`. Replace the contents with the code as shown.

![[orderFormNewFile.png]]

```php
<title>Order Form</title>
<?php include "template.php";
/**  @var $conn */
?>
<link rel="stylesheet" href="css/orderForm.css">

<h1 class="text-primary">Order Form</h1>
```

The logic for the order form is relatively complex due to the dynamic nature of the product information as well as the CSS used to display it.

This will display a page similar to this.

![[orderFormFinalForm.png]]

![[orderFormCode.png]]

```php
<?php
$status = "";
if (isset($_POST['Code']) && $_POST['Code'] != "") {
	$code = $_POST['Code'];
	$row = $conn->querySingle("SELECT * FROM products WHERE code='$code'", true);
	$name = $row['ProductName'];
	$price = $row['Price'];
	$image = $row['Image'];
	$id = $row['ProductID'];

	$cartArray = array(
		$code => array(
			'id' => $id,
			'productName' => $name,
			'code' => $code,
			'price' => $price,
			'quantity' => 1,
			'image' => $image)
	);

	// Debug Purposes
	// echo '<pre>'; print_r($cartArray); echo '</pre>';

	if (empty($_SESSION["ShoppingCart"])) {
		$_SESSION["ShoppingCart"] = $cartArray;
		$status = "<div class='box'>Product is added to your cart!</div>";
	} else {
		$array_keys = array_keys($_SESSION["ShoppingCart"]);
		if (in_array($code, $array_keys)) {
			$status = "<div class='box' style='color:red;'>Product is already added to your cart!</div>";
		} else {
			$_SESSION["ShoppingCart"] = array_merge(
				$_SESSION["ShoppingCart"], $cartArray
			);
			$status = "<div class='box'>Product is added to your cart!</div>";
		}
	}
}
?>

<div class="message_box" style="margin:10px 0px;">
	<?php echo $status; ?>
</div>

<?php

if (!empty($_SESSION["ShoppingCart"])) {
	$cart_count = count(array_keys($_SESSION["ShoppingCart"]));
	?>
	<div class="cart_div">
		<a href="cart.php"><img src="images/cart-icon.png"/> Cart<span>
<?php echo $cart_count; ?></span></a>
	</div>
	<?php
}

$result = $conn->query("SELECT * FROM Products");
while ($row = $result->fetchArray()) {
	echo "<div class='product_wrapper'>
	<form method ='post' action =''>
	<input type='hidden' name='Code' value=" . $row['Code'] . " />
	<div class='image'><img src='images/productImages/" . $row['Image'] . "' width='100' height='100'/></div>
	<div class='name'>" . $row['ProductName'] . "</div>
	<div class='price'>$" . $row['Price'] . "</div>
	<button type='submit' class='buy'>Add to Cart</button>
	</form>
	</div>";
}

?>
```

## Update the Navbar

Open `template.php` and update the navbar to link to the Order Form. However, make sure the user is logged in before showing the link. If they aren’t logged in, display the registration link.

![[orderFormNavUpdate.png]]

```php
if (isset($_SESSION["FirstName"])) {
	echo '<li class="nav-item" ><a class="nav-link" href = "orderForm.php"> Order Form </a ></li >';
	echo '<li class="nav-item" ><a class="nav-link" href = "invoiceList.php"> Invoice list</a ></li >';
} else {
	echo '<li class="nav-item"><a class="nav-link" href="register.php">Register</a></li>';
}
```
