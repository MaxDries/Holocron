
# Remove Products

## Product List Update

Open`productList.php` page and add a new link to the page to remove the products.

![[removeProductLink.png]]

```php
<div class="col-md-2">
	<!-- remove button-->
	<a href="productRemove.php?prodCode=<?php echo $productData[2]; ?>">Remove</a>
</div>
```

## Product remove page

Create a new page `productRemove.php` in the root directory of the project. Replace the default contents with the project default code.

![[removeProductNewPage.png]]

```php
<?php include "template.php";
/**  @var $conn */
?>
<title>Remove Product</title>
```

Add the code block to only allow the functionality if the user is an administrator.

![[removeProductCheckPermissions.png]]

```php
<?php
// Check to see if User is Administrator (level 1)
// If they are, allow functionality, otherwise redirect them back to the front page.
if ($_SESSION['AccessLevel'] == 1) {
   

   
} else {
	header("location:index.php");
}
?>
```

Assuming the user is an administrator, the next step is to find the product code in the URL as a parameter.

![[removeProductAdminGetCode.png]]

```php
if (isset($_GET["prodCode"])) {
  // delete product from database
  $productToDelete = $_GET["prodCode"];
} else {
  echo "No product code found.";
}
```

Now assuming that the code is valid, then run a simple SQL query to remove the record from the table.


> [!info] This approach means that the product details will be completely removed from the database without being able to retrieve the data. Consider how this could be done differently to allow for a product to be ‘reinstated’ in the project.



![[removeProductAdminDeleteProduct.png]]

```php
$query = "DELETE FROM products WHERE code='$productToDelete'";
$sqlstmt = $conn->prepare($query);
$sqlstmt->execute();
echo "<p>Product " . $productToDelete . " has been deleted from the database";
```

## Test the functionality

Try adding a ‘dummy’ product into the system and then attempt to remove it through the newly created page.

