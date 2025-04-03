---
tags:
  - "2022"
  - archived
---

Copy the contents of the `index.html` from the previous iteration of the site and replace the contents of `template.html`.

As can be seen in the screenshot, the CSS file is not loading correctly.

![[Screen_Shot_2022-06-16_at_4.23.47_pm.png|Screen Shot 2022-06-16 at 4.23.47 pm.png]]

Change the link from `ngunnawal.css` to `/static/ngunnawal.css` and save the changes. The preview should update with the CSS loaded.

![[2022-06-16_16-26-55.2022-06-16_16_27_37.gif|2022-06-16 16-26-55.2022-06-16 16_27_37.gif]]

In the HTML, remove the content from the cells and leave them blank.

Include code in each cell such as:

`{% **block cellContent1** %} {% **endblock** %}`

`{% **block cellContent2** %} {% **endblock** %}`

`{% **block cellContent3** %} {% **endblock** %}`

> [!info] The names within the {% and %} are Jinja2 blocks. These will be referred to later from the child pages.


![[Screen_Shot_2022-06-21_at_10.19.49_am.png|Screen Shot 2022-06-21 at 10.19.49 am.png]]

```python
<div class="grid-container"> <!-- Overall Layout -->
	<div class="full-width text-highlighting">
		<h1 style="text-align: center">{{ title }}</h1>
	</div>
	<div class="double-height">
		{% block cellContent1 %} {% endblock %}
	</div>
	<div>
		{% block cellContent2 %} {% endblock %}
	</div>
	<div class="text-highlighting">
		{% block cellContent3 %} {% endblock %}
	</div>
</div>
```

## Parent? Child? Pages?

In Flask, the pages can be created as a hierarchy, with one or some being the parent. In this project, so far, the template.html page will be the parent.

The child pages will inherit everything from template.html and modify only the sections of the page that are ‘allowed’. 

![[Flask_Parent_Child_Pages_(1).png]]

## Update Images

Update the links for the Logo and Banner by including `/static/` at the start of the path.

![[Screen_Shot_2022-06-16_at_9.37.59_pm.png|Screen Shot 2022-06-16 at 9.37.59 pm.png]]

## Page Title

Modify the “heading” cell and the `<title>` tag to be replaced with `{{ title }}`.

This will allow us to define the title in the python code, specific for each individual page.

It has the added advantage that the Heading on the page will always be exactly the same as the page title that appears in the tab.

![[Screen_Shot_2022-06-16_at_4.57.54_pm.png|Screen Shot 2022-06-16 at 4.57.54 pm.png]]

## Internal CSS Block

At this stage, there is no need for internal CSS on the template file, as it’s all imported through the ngunnawal.css external file. However some of the individual pages *may* require internal CSS. 

To make it easier in the future, create an empty block called `pageStyle` in the head section to be used if needed.

![[Screen_Shot_2022-07-01_at_10.40.41_am.png|Screen Shot 2022-07-01 at 10.40.41 am.png]]

```python
{% block pageStyle %}
{% endblock %}
```

## Template Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>{{ title }}</title>
	<link rel="stylesheet" href="/static/ngunnawal.css">
	<link rel="stylesheet" href="/static/css/bootstrap.css">
	<script src="/static/js/bootstrap.bundle.js"></script>
	{% block pageStyle %}
	{% endblock %}
	<style>

	</style>
</head>
<body>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
	<div class="container-fluid">
		<a class="navbar-brand" href="#">
			<img src="/static/images/NgunnawalCountryLogo.png">
		</a>
		<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
				aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
			<span class="navbar-toggler-icon"></span>
		</button>
		<div class="collapse navbar-collapse" id="navbarNav">
			<ul class="navbar-nav">
				<li class="nav-item">
					<a class="nav-link" href="/">Home</a>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="history.html">History</a>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="gallery.html">Gallery</a>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="contact.html">Contact Us</a>
				</li>

				<li class="nav-item">
					<a class="nav-link" href="/todo">Web App - Todo</a>
				</li>
				{% if user.is_anonymous %}
					<li class="nav-item">
						<a class="nav-link" href="/register">Registration</a>
					</li>
					<li class="nav-item">
						<a class="nav-link" href="/login">Login</a>
					</li>
				{% else %}
					<li class="nav-item">
						<a class="nav-link" href="/logout">Logout</a>
					</li>
					<li class="nav-item">
						<a class="nav-link" href="/reset_password">Reset Password</a>
					</li>
				{% endif %}
			</ul>
		</div>
		<div style="color:white">{{ user.name }}</div>
		<img class="img-fluid" src="/static/images/NgunnawalCountryBanner.png">

	</div>
</nav>

<div class="container-fluid"> <!-- Overall Layout -->
	<div class="row">
		<div class="col">
			<h1 style="text-align: center">{{ title }}</h1>
		</div>
	</div>
	<div class="row">
		<div class="col-4">
			{% block cellContent1 %}

			{% endblock %}
		</div>
		<div class="col-8">
			{% block cellContent2 %} {% endblock %}
		</div>
	</div>
	<div class="row">
		<div class="col text-highlighting">
			{% block cellContent3 %} {% endblock %}
		</div>
	</div>
</div>

</body>
</html>
```
