
# Project Overview

Open or create `design.md` in your project. Write a short description of the website, what the products are you’re going to sell etc. And a list of the functionality.

This is a markdown file, and as such you can indicate headings with #’s.

```markdown
# Heading 1
## Heading 2
### Heading 3
```

## Example

The markdown shown here will be rendered as indicated.

```markdown
# Project Overview

This PHP website will be an ecommerce site to sell handmade star wars memorabilia.

## User Management
Users will be able to login, log out, reset their passwords, and edit their details.

Users will need to store:
- Name
- DOB
- Hashed password
- Access Level (user vs Administrator)
- Status (active or disabled)
```

![[WebDev/_shared/_projects/Shopfront/_images/projectOverview.png]]


# Shopping Cart Overview

The shopping cart functionality, or subsystem, is broken up into two pages - `orderForm.php` and `cart.php`. The Order form is responsible for displaying the products, allowing the user to select items to purchase. The Cart page displays the selected item/s and calculates the sub totals and total, and allows the user to ‘make’ a purchase.

[Example](https://drive.google.com/file/d/1pcbU9X2vxiTqaxBkXhadTkw6CL3NHcSS/view?usp=drive_web)
