---
tags: php
---

![[Theory#Data Structures]]


# PHP Implementation

## Arrays

`array()` is used in PHP to define an array.

Arrays are implemented in PHP to C-based languages, with square brackets - `[` and `]` - used to indicate the indexes being accessed.

```php
<?php
$starWarsMovies = array("A New Hope", "Empire Strikes Back", "Return Of the Jedi");
echo "I like " . $starWarsMovies[0] . ", " . $starWarsMovies[1] . " and " . $starWarsMovies[2] . ".<br>";
echo count($starWarsMovies);
?>
```

which will output

![[dataArrayOutput.png|Untitled]]

### Get the length of an Array

There is a common need to find the length of an array (how many elements there are). In PHP, you can use the `count()` function.

```php
<?php
$starWarsMovies = array("A New Hope", "Empire Strikes Back", "Return Of the Jedi");
echo "I like " . $starWarsMovies[0] . ", " . $starWarsMovies[1] . " and " . $starWarsMovies[2] . ".";
echo count($starWarsMovies);
?>
```

which will output

![[dataArrayLength.png|Untitled]]

> [!info] Remember - the length of an array is **always** one more than the index of the last element!


## Key Value Pair

> [!info] In PHP, these are referred to as **Associative Arrays**. Otherwise, they are exactly the same.


Similarly to ‘normal’ arrays, the `array()` keyword is used to define associative arrays.

```php
$subjectRooms = array("Website Development"=>"6118", "English"=>"3017", "Maths"=>"1120", "PE"=>"Gym", "Drama"=>"Theatre");
echo "Room Numbers: <br>";
echo "English: ".$subjectRooms["English"]."<br>";
echo "Maths: ".$subjectRooms["Maths"]."<br>";
echo "Website Development: ".$subjectRooms["Website Development"]."<br>";
echo "Drama: ".$subjectRooms["Drama"]."<br>";
echo "PE: ".$subjectRooms["PE"]."<br>";
```

will produce

![[dataKeyValue.png|Untitled]]

## Looping through an Array

PHP has another loop type called `foreach` where you can access each element of an array - either a ‘normal’ array or an associative array.

The syntax for this is:

```php
foreach (iterable_expression as $value)
	statement
foreach (iterable_expression as $key => $value)
	statement]
```

`iteratable_expression` is the array you are iterating over.

`value` is the element of the current iteration

`key` is used with an associative array, where the key is also assigned along with the value.

### Example

```php
foreach ($subjectRooms as $subject => $roomNumber){
	echo "Subject - ".$subject.' - is in room '.$roomNumber."<br>";
}
```


> [!info] In the first iteration of this foreach loop, `key` will be "Website Development" and `value` will be “6118”.



![[dataArrayIteration.png|Untitled]]

# Practical Exercises


> [!info] In this task, you’ll centralise the product names and prices to use associative arrays.


## Product Catalogue

Currently the Products are listed with hard coded prices, specified in `invoice.php` and the names of the products are only displayed in the HTML of the same page. Storing these in an associative array, at this stage, would be a upgrade to the system.

The new data can be saved in any page, however as the details are used on both  `invoiceList.php` and `orderForm.php` pages, this could be defined in `template.php`. 

> [!info] Eventually this data will be stored in a database, but that’s a future upgrade.


Open `template.php` and define a new associative array in the php block.


> [!info] You can choose the product names and prices.



![[dataArrayDefinition.png|Untitled]]

```php
$productNames = array("product1"=>"Darth Vader Helmet", "product2"=>"Grogu Plush", "product3"=>"ROTJ Jigsaw", "product4"=>"Aftermath", "product5"=>"Alphabet Squadron");
$productPrices= array("product1"=>299.0, "product2"=>32.95, "product3"=>219.95, "product4"=>24.95, "product5"=>24.95);
```

Save `template.php`. 

![[commonBlocks#Commit & Push]]

Open `orderForm.php` and update the Product Names that are outputted to use the `$productNames` associative array.

![[dataOrderFormOutput.png|Untitled]]

```php
<?php echo $productNames["product1"]; ?>
```

Load the page in the Preview or browser and the item names should then appear.

![[dataOrderForm.png|Untitled]]

To remove the error that appears indicating that `productNames` is unknown, you can add the following code to the top PHP block. 

This indicates that the variable `productNames` will be used later in the code.

![[dataSetVariable.png|Untitled]]

```php
/** @var $productNames */
```

![[commonBlocks#Commit & Push]]

Open `invoice.php` and repeat the same process above, but also for the prices.

The hardcoded costs can be removed as they are not required anymore. **Delete** these variables.

```php
$prod1ItemCost = 3.4;
$prod2ItemCost = 5.0;
$prod3ItemCost = 12.54;
$prod4ItemCost = 19.77;
$prod5ItemCost = 1.01;
```

Update the subTotal calculations to use the `$productPrices` associative array. 

![[dataSubTotalCalculation.png|Untitled]]

```php
$prod1SubTotal = $prod1Quantity * $productPrices["product1"];
$prod2SubTotal = $prod2Quantity * $productPrices["product2"];
$prod3SubTotal = $prod3Quantity * $productPrices["product3"];
$prod4SubTotal = $prod4Quantity * $productPrices["product4"];
$prod5SubTotal = $prod5Quantity * $productPrices["product5"];
```

Similar to `orderForm.php` update the product name outputs to use the `$productNames` associative array.

![[dataProductOutput.png|Untitled]]

```php
<?php echo $productNames["product1"]; ?>
```

The displayed individual costs of each item will need to be updated as well.


> [!info] You may have noticed the different syntax - `<?=` is equivalent to `<?php echo`.


![[dataProductPricesOutput.png|Untitled]]

Reload the page in the browser and the product names and prices should reflect the values set in the two associated arrays.

![[dataProductOrderOutput.png|Untitled]]

 

![[commonBlocks#Commit & Push]]

# Review

1. What are the main differences between an Array and a Key Value Pair.
2. What are multidimensional dimensional arrays? Describe them and show code examples for what they are and what they can do. What situations would you use a multidimensional array over other data structures?


