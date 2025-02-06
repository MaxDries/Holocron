
# Add Products


> [!info] This functionality is very similar to the user registration process, with a few notable differences:

1. Products have a category
2. The choice for categories can be pulled from the database.
3. Images can be uploaded and stored for each product.
4. Only administrators can add products


Create a new PHP page in the main directory of the project called `productAdd.php`.  Replace the default contents of the page with the code shown.

![[addProductsNewPage.png]]

```php
<?php include "template.php";
/**  @var $conn */
?>
	<title>Add Products</title>
	<h1 class='text-primary'>Add Products</h1>
```

## Check permissions

Before allowing access to the form to create a new product, first check the user is an administrator through the `AccessLevel` session variable.

Add this code after the `Add Products` heading.

![[addProductsCheckPermission.png]]

```php
<!-- Front End -->

<?php
// Check to see if User is Administrator (level 1)
// If they are, allow functionality, otherwise redirect them back to the front page.
if ($_SESSION['AccessLevel'] == 1) {
	?>

	<?php
} else {
	header("location:index.php");
}
?>
```

## Front End

The Product Add page should have a consistent appearance to the registration page, except with the specific fields required. So for instance, the page could appear as:

![[addProductsFrontEndUpdates.png]]

To achieve this, the code would be as follows. Add this code within the permissions check code.

![[addProductsFrontEndForm.png]]

```php
<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post" enctype="multipart/form-data">
	<div class="container-fluid">
		<div class="row">
			<!--Customer Details-->

			<div class="col-md-6">
				<h2>Products Details</h2>
				<p>Product Name<input type="text" name="prodName" class="form-control" required="required"></p>
				<p>Product Category
					<input type="text" name="prodCategory" class="form-control" required="required">
				</p>
				<p>Quantity<input type="number" name="prodQuantity" class="form-control" required="required"></p>
			</div>
			<div class="col-md-6">
				<h2>More Details</h2>
				<!--Product List-->
				<p>Price<input type="number" step="0.01" name="prodPrice" class="form-control" required="required">
				</p>
				<p>Product Code<input type="text" name="prodCode" class="form-control" required="required"></p>
				<p>Product Picture <input type="file" name="prodImage" class="form-control" required="required"></p>
			</div>
		</div>
	</div>
	<input type="submit" name="formSubmit" value="Submit">
</form>
```

## Back End Functionality

After the front end code, add a new PHP block to collect the fields required from the form. This is done in the same way as other forms on the website.

Additionally, check to see if the product code already exists in the database. This is done similarly as checking if the username already exists.

![[addProductsBackEndCollectFormData.png]]

```php
<?php
// Back End
if ($_SERVER["REQUEST_METHOD"] == "POST") {
//    Customer Details
	$prodName = sanitiseData($_POST['prodName']);
	$prodCategory = sanitiseData($_POST['prodCategory']);
	$prodQuantity = sanitiseData($_POST['prodQuantity']);
	$prodPrice = sanitiseData($_POST['prodPrice']);
	$prodCode = sanitiseData($_POST['prodCode']);

//check if product exists.
	$query = $conn->query("SELECT COUNT(*) FROM Products WHERE code='$prodCode'");
	$data = $query->fetchArray();
	$numberOfProducts = (int)$data[0];

	if ($numberOfProducts > 0) {
		echo "Sorry, product already taken";
	} else {
// Product Registration commences
}
}
```

### Image Upload

Uploading images is a little more complex than simply uploading text to store in the database. **Either** the image can be uploaded and stored in the database, or the image is uploaded and stored on the server in a directory and a filename is stored in the database.

The latter option is preferred as database systems are not generally designed to store large binary data such as images.

To prepare the uploading of the image, a number of items must be checked - that the file is the correct type and that it’s not exorbitantly large. To do so, once the Product Registration Commences section of the code has been reached, extract the necessary data for the image.

![[addProductsImageUpload.png]]

```php
// Image details
$file = $_FILES['prodImage'];
$fileName = $_FILES['prodImage']['name'];
$fileTmpName = $_FILES['prodImage']['tmp_name'];
$fileSize = $_FILES['prodImage']['size'];
$fileError = $_FILES['prodImage']['error'];
$fileType = $_FILES['prodImage']['type'];

// defining what type of file is allowed
// We separate the file, and obtain the file extension.
$fileExtension = explode('.', $fileName);
$fileActualExtension = strtolower(end($fileExtension));

$allowedExtensions = array('jpg', 'jpeg', 'png', 'pdf');
```

Once all the relevant information is collected or extracted, confirm that the file extension is allowed, there are no errors, and that the size of the file is under a pre-determined size. **In other words, that the file is ok to upload**.

![[addProductsImageFileExtensions.png]]

```php
//We ensure the extension is allowable
if (in_array($fileActualExtension, $allowedExtensions)) {
	if ($fileError === 0) {
		// File is smaller than arbitrary size
		if ($fileSize < 10000000000) {
			//file name is now a unique ID based on time with IMG- preceeding it, followed by the file type.
			$fileNameNew = uniqid('IMG-', True) . "." . $fileActualExtension;
			//upload location
			$fileDestination = 'images/productImages/' . $fileNameNew;
			// Upload file
			move_uploaded_file($fileTmpName, $fileDestination);
```

### Insert Data into Database

At this stage, the form data has been collected and sanitised, and the image has been uploaded. It’s now time to write to the database.

After the image has been uploaded, add the code to write to the database.


> [!info] This code also shows the `else` blocks for if any of the image checks fail.



![[addProductsInsertData.png]]

```php
// Write details to database
$sql = "INSERT INTO Products (ProductName, Category, Quantity, Price, Image, Code) VALUES (:newProdName, :newProdCategory, :newProdQuantity, :newProdPrice, :newProdImage, :newProdCode)";
$stmt = $conn->prepare($sql);
$stmt->bindValue(':newProdName', $prodName);
$stmt->bindValue(':newProdCategory', $prodCategory);
$stmt->bindValue(':newProdQuantity', $prodQuantity);
$stmt->bindValue(':newProdPrice', $prodPrice);
$stmt->bindValue(':newProdImage', $fileNameNew);
$stmt->bindValue(':newProdCode', $prodCode);
$stmt->execute();
header("location:index.php");
```

### Create Products!

Create a number of products, with a range of different categories and images! Find images that are free to use.

### Product Categories

After a number of product categories has been created, the page can be update to limit the user to only a set number of options for the category. 

To do so, add the following SQL to the top of the page.

This code will query the products table and create a list of all the unique categories in the table.

![[addProductsProductCategories.png]]

```php
<?php
$query = $conn->query("SELECT DISTINCT category FROM Products");
?>
```

Replace the input text box for the product category with a drop-down box (using the `select` tag in HTML) which dynamically fills in the list from the SQL query.

![[addProductsProductCategoriesDropDown.png]]

```php
<select name="prodCategory">
	<?php
	while ($row = $query->fetchArray()) {
		echo '<option>' . $row[0] . '</option>';
	}
	?>
</select>
```
