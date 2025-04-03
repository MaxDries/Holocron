---
tags:
  - "2022"
  - archived
---


# Errors!

By this stage of the development process, this screen may be familiar to you.

There are a number of issues with this page.

1. There’s an error in the code that needs to be fixed.
2. The user doesn’t get any information to help them solve the problem.
3. The user loses confidence in the website as the site ‘is broken’.

It is possible to build custom ‘error’ pages to keep the user ‘within’ the website and allow them to continue using the site.

![[Screen_Shot_2022-08-09_at_10.44.41_am.png|Screen Shot 2022-08-09 at 10.44.41 am.png]]

# HTML Pages

Create a html page in the templates folder called `500.html`. Include the default code for the website:

```html
{% extends 'base.html' %}

{# Place the content for the cellContent1 in this block #}
{% block rowTwoColOneContent %}
	Sidebar
{% endblock %}

{# Place the content for the cellContent2 in this block #}
{% block rowTwoColTwoContent %}

{% endblock %}

{# Place the content for the cellContent3 in this block #}
{% block cellContent3 %}
	Footer
{% endblock %}
```

In `cellContent2`, add a message to the user to indicate that there is a problem, but in a more user friendly way than the example above.

> [!info]  Consider what the user would want to see on this page.


![[Screen_Shot_2022-08-09_at_10.51.28_am.png|Screen Shot 2022-08-09 at 10.51.28 am.png]]

You can choose how you want the messages to display. Using Bootstraps built-in Alerts can be a quick and easy solution, such as:

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 12.png|Untitled]]

```html
<div class="alert alert-danger" role="alert">
  File Not Found!
</div>
```

Repeat this process for a `404.html` error page for if the page doesn’t exist.

# Error Handlers

Open `app.py` and add the following routes.

> [!info]  These routes capture any `404` or `500` HTTP errors and instead of showing the standard error for the browser, it will show a custom built error, using based on the template. From a UX point of view, this ‘keeps’ the user within the site and can allow them to rectify the error by returning to other pages.
> - 404 error - Not Found
> - 500 error - Internal Server Error
> You can find information on HTTP errors here: [https://developer.mozilla.org/en-US/docs/Web/HTTP/Status](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)


```python
# Error Handlers
@app.errorhandler(404)
def page_not_found(e):
	return render_template("404.html", user=current_user), 404

@app.errorhandler(500)
def internal_server_error(e):
	return render_template("500.html", user=current_user), 500
```

# Test the site!

Run the project and attempt to access a page that doesn’t exist. Make sure the custom error page loads.

![[Screen_Shot_2022-08-09_at_10.55.44_am.png|Screen Shot 2022-08-09 at 10.55.44 am.png]]

To force the page to load correctly, you may need to **turn off** the FLASK_DEBUG setting. To do so, click on Edit Configurations under the Flask (app.py) dropdown.

![[Screen_Shot_2022-09-05_at_9.16.43_am.png|Screen Shot 2022-09-05 at 9.16.43 am.png]]

Uncheck the box, and click ok.

![[Screen_Shot_2022-09-05_at_9.17.34_am.png|Screen Shot 2022-09-05 at 9.17.34 am.png]]

Then, in `index.html` comment out the last line of code to force an error.

![[Screen_Shot_2022-09-05_at_9.18.34_am.png|Screen Shot 2022-09-05 at 9.18.34 am.png]]

Rerun the site and the 500 error page should appear.

# Example error pages

![[pMGfBE5gnyuzxa6BGKAi3Q-970-80.jpg.webp|pMGfBE5gnyuzxa6BGKAi3Q-970-80.jpg.webp]]

![[mYKbf3DSinvWnEzmHkEnCE-970-80.jpg.webp|mYKbf3DSinvWnEzmHkEnCE-970-80.jpg.webp]]

![[pbtqaZH7Q9QcrVDnuJomQT-970-80.jpg.webp|pbtqaZH7Q9QcrVDnuJomQT-970-80.jpg.webp]]


