---
tags:
  - "2022"
  - archived
---

Go to [https://getbootstrap.com/](https://getbootstrap.com/) and click on the Read Installation docs link.

![[Screen_Shot_2022-07-27_at_2.32.15_pm.png|Screen Shot 2022-07-27 at 2.32.15 pm.png]]

Download the Compiled CSS & JS.

![[Screen_Shot_2022-07-27_at_2.33.25_pm.png|Screen Shot 2022-07-27 at 2.33.25 pm.png]]

Unzip (Extract) the zip file and copy the `CSS` and `JS` folders into the `static` folder in your project.

![[Screen_Shot_2022-07-27_at_2.34.59_pm.png|Screen Shot 2022-07-27 at 2.34.59 pm.png]]

## Add Bootstrap into Template

Open templates/template.html and add the following code to import the new Bootstrap CSS and JS libraries.

```python
<link rel="stylesheet" href="/static/css/bootstrap.css">
<script src="/static/js/bootstrap.bundle.js"></script>
```

![[Screen_Shot_2022-07-27_at_2.37.53_pm.png|Screen Shot 2022-07-27 at 2.37.53 pm.png]]

If you run the project and load the site you may notice some changes.

> [!info] These will need to be fixed by updating the site to utilise the Bootstrap libraries more effectively.


![[Screen_Shot_2022-07-27_at_2.39.06_pm.png|Screen Shot 2022-07-27 at 2.39.06 pm.png]]

## Navbar Updates

Bootstrap has a number of different, configurable, navbars that you can use in your sites. The Bootstrap site (linked) has a number of the different styles that you can choose from or take parts that you want or need to include.

[https://getbootstrap.com/docs/5.2/components/navbar/](https://getbootstrap.com/docs/5.2/components/navbar/)

Some examples are shown here.

![[Screen_Shot_2022-07-27_at_2.42.16_pm.png|Screen Shot 2022-07-27 at 2.42.16 pm.png]]

![[Screen_Shot_2022-07-27_at_2.42.51_pm.png|Screen Shot 2022-07-27 at 2.42.51 pm.png]]

> [!info]  It’s up to you to decide how exactly your navbar appears to the user. You can configure it as much or as little as you want.


Start by Using the simplest version of the navbar code. Using the link above, this can be found under “Nav” down the page. 

![[Screen_Shot_2022-07-27_at_2.45.53_pm.png|Screen Shot 2022-07-27 at 2.45.53 pm.png]]

Copy the code shown there. 


> [!info] The code shown here *may* be the same as shown on the website, however the website code may have been updated. Check the site for the latest version.


```python
<nav class="navbar navbar-expand-lg bg-light">
  <div class="container-fluid">
	<a class="navbar-brand" href="#">Navbar</a>
	<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
	  <span class="navbar-toggler-icon"></span>
	</button>
	<div class="collapse navbar-collapse" id="navbarNav">
	  <ul class="navbar-nav">
		<li class="nav-item">
		  <a class="nav-link active" aria-current="page" href="#">Home</a>
		</li>
		<li class="nav-item">
		  <a class="nav-link" href="#">Features</a>
		</li>
		<li class="nav-item">
		  <a class="nav-link" href="#">Pricing</a>
		</li>
		<li class="nav-item">
		  <a class="nav-link disabled">Disabled</a>
		</li>
	  </ul>
	</div>
  </div>
</nav>
```

Look for the first link and modify it to instead of the href being `#`, it instead directs the user to `/`.

You’ll also need to remove any code that indicates it’s the current page, as this will not work through Flask and the template.

**Before**

```python
<a class="nav-link active" aria-current="page" href="#">Home</a>
```

**After**

```python
<a class="nav-link" href="/">Home</a>
```


> [!info] You can check the website changes by refreshing the page at each step to make sure it works.

Change the other links in the nav bar to link to the pages that will be included later (from the previous project).

**Before**

![[Screen_Shot_2022-07-27_at_2.53.19_pm.png|Screen Shot 2022-07-27 at 2.53.19 pm.png]]

**After**

![[Screen_Shot_2022-07-27_at_2.54.19_pm.png|Screen Shot 2022-07-27 at 2.54.19 pm.png]]

Included in the default code is the first link `Navbar` ****which is a placeholder link to return the user back to the homepage. As our previous project had a logo, change this text to be the logo used previously. 


> [!info]  The image should be in the static/images folder. If it is not, copy it in.


![[Screen_Shot_2022-07-27_at_2.59.27_pm.png|Screen Shot 2022-07-27 at 2.59.27 pm.png]]

Move the banner image from the old navbar code to just above the new navbar code block.

Remove the `class="center"` attribute as well.

![[Screen_Shot_2022-07-27_at_3.01.12_pm.png|Screen Shot 2022-07-27 at 3.01.12 pm.png]]

Remove the old navbar code as it is no longer required.

![[Screen_Shot_2022-07-27_at_2.55.07_pm.png|Screen Shot 2022-07-27 at 2.55.07 pm.png]]

The site now appears similar to this. The issue is that the links in the navbar are not visible.

This can be changed using some of the other colour styles for Navbars.

![[Screen_Shot_2022-07-27_at_3.02.05_pm.png|Screen Shot 2022-07-27 at 3.02.05 pm.png]]

Again, on this site - [https://getbootstrap.com/docs/5.2/components/navbar](https://getbootstrap.com/docs/5.2/components/navbar) Click on the Color Schemes section to look at the different options and copy the code relevant to what you want.

![[Screen_Shot_2022-07-27_at_3.12.34_pm.png|Screen Shot 2022-07-27 at 3.12.34 pm.png]]

You will need to change the `<nav ...>` line of code to match what the code is on the website.

**Before**

![[Screen_Shot_2022-07-27_at_3.14.00_pm.png|Screen Shot 2022-07-27 at 3.14.00 pm.png]]

**After**

![[Screen_Shot_2022-07-27_at_3.15.24_pm.png|Screen Shot 2022-07-27 at 3.15.24 pm.png]]

After that small change, you should be able to see a significant difference in the navbar.

![[Screen_Shot_2022-07-27_at_3.15.52_pm.png|Screen Shot 2022-07-27 at 3.15.52 pm.png]]


> [!info]  The site will still need some more changes required, however this already has added some additional functionality that you may have already noticed. If not, try resizing the width of the window!


## Bootstrap Layout

Bootstrap allows for much more flexible layout organisations, and due to its responsive website feature, allows you to build grids with up to 12 columns.

Bootstraps Grid system is described in detail on this page. 

[Grid system](https://getbootstrap.com/docs/5.2/layout/grid/)


> [!info]  Each row can be split into 12 columns, and each row can be separated in individuals ways. This allows for a lot of flexibility.
> ![[bootstrap_grid_system_sample_diagram.png|bootstrap_grid_system_sample_diagram.png]]
> [https://www.tutlane.com/tutorial/bootstrap/bootstrap-grid-system](https://www.tutlane.com/tutorial/bootstrap/bootstrap-grid-system)


Now, the layout of the site, previously CSS’s `Grid` layout can be updated to use Bootstrap’s `Grid` layout.

> [!info]  As with much of the site, the design choices are yours to make - this is only an example.


Change the class of the main layout to `container-fluid`.

![[Screen_Shot_2022-07-27_at_10.42.30_pm.png|Screen Shot 2022-07-27 at 10.42.30 pm.png]]

In this example, the first row will contain just the heading. The second row will contain `cellContent1` and `cellContent2`, and the third row will contain `cellContent3` as the footer.

Here is the wireframe of what the following code will attempt.

![[Image.png|Image.png]]

Add a new section of the layout to organise the site into rows.

The three rows have been added - `<div class="row">` matching the wireframe above.

![[Screen_Shot_2022-07-27_at_10.54.00_pm.png|Screen Shot 2022-07-27 at 10.54.00 pm.png]]

> [!info]  You can use CTRL+ALT+L to automatically format the code.


For each Row, you need to add the class `col` to indicate it’s a new column, even if there is only one column for the row.

You can see in the screenshot that `col` can be used in conjunction with other custom-written CSS classes such as text-highlighting if needed.

![[Screen_Shot_2022-07-27_at_11.00.31_pm.png|Screen Shot 2022-07-27 at 11.00.31 pm.png]]

Lastly, the width of each column can be set. In this case, the sidebar in the second row is roughly a third of the width of the page, and the main content is roughly two thirds. 

Bootstrap defines a row has 12 columns, so that would mean the Sidebar column has a width of 4, and the main content has a width of 8.

Change the different sizes of the columns to suit your layout design.

![[Screen_Shot_2022-07-27_at_11.05.17_pm.png|Screen Shot 2022-07-27 at 11.05.17 pm.png]]

After this process, the site should look similar to this (depending on your layout design choices)

![[Screen_Shot_2022-07-27_at_11.06.26_pm.png|Screen Shot 2022-07-27 at 11.06.26 pm.png]]

## Further Development

There are many other updates that can be made throughout the site. Some of these will be addressed later in the process, however you may look at Bootstraps Docs for some ideas.

[Get started with Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/introduction/)