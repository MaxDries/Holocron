

# Administrator Access

In order to add Administrator functionality, a simple modification that can be implemented is by adding an `AccessLevel` field in the Customer table. As developers you have the choice of how this is managed, i.e. this field could be TEXT with the words “Administrator” or “Regular User” stored. Alternatively, using numbers to denote access level could be used. For instance:

| AccessLevel field | User Level |
| --- | --- |
| 1 | Administrator |
| 2 | Regular User |

Add a new field in the Customer table called `AccessLevel`, with `integer` as the type.

Once created update the fields so that there is at least one administrator and one regular user in the system. Make sure you Submit the changes back to the database by pressing the Submit button.

![[userLevelNewField.png]]

## Modify `login.php`

When the user successfully authenticates, set the `AccessLevel` session variable to the value stored in the database for that user.

![[userLevelSessionVariable.png]]

```php
$_SESSION['AccessLevel'] = $row['AccessLevel'];
```



## Navbar changes

In preparation of the coming changes, the navbar needs to be updated to link to the new pages created.

Open `template.php` and add a new section for the Product Management section. 

This can be done in a number of ways, however as this section all relate to the Products, it may make sense to collect all the links in a single dropdown.

![[userLevelNavigationDropDown.png]]

Look for the section of the code where the navbar loads different links depending on whether the user is logged in or not (`if (isset($_SESSION["FirstName"])) {`)

Add the code to show a dropdown list of the links required, however only where the user is an administrator. 


> [!info] The AccessLevel session variable will be implemented in another section of the tutorial.



![[userLevelNavigation.png]]

```php
if (isset($_SESSION["AccessLevel"])) {
	if ($_SESSION["AccessLevel"] == 1) {
		?>
		<li class="nav-item dropdown">
			<a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown"
			   aria-expanded="false">
				Product Management
			</a>
			<ul class="dropdown-menu">
				<li><a class="dropdown-item" href="productAdd.php">Add Products</a></li>
				<li><a class="dropdown-item" href="productList.php">Product List</a></li>
			</ul>
		</li>
		<?php
	}
}

?>
```
