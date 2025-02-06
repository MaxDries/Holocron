---
tags:
  - "2022"
  - archived
---


# App.py

Open [`app.py`](http://app.py). This file contains all the python code to manage the server-side functionality. 

Each route has an associated function.

In this example, “`/`” is the route and `hello_world()` is the associated function.

![[Screen_Shot_2022-06-21_at_10.28.21_am.png|Screen Shot 2022-06-21 at 10.28.21 am.png]]

## How does this work?

When the browser access that route, it will run the associated function. This function performs any instructions that are programmed and sends the result back to the browser to display, through the `return` statement.

## Example

[2022S2 - How to run Flask.webm](https://drive.google.com/file/d/1z39dUnvYnL5cckQEu1scP4_6hp54Y6C3/view?usp=drivesdk)

- Site Template


# Template.html updates

In the templates folder open the HTML file called `template.html`.

![[Screen_Shot_2022-06-16_at_4.22.55_pm.png|Screen Shot 2022-06-16 at 4.22.55 pm.png]]

This file will be used by all the other pages of our website to contain any common code, such as the 

- design of the site
- navbar links
- CSS
- Javascript
- Logo and Banner
- etc.

For instance, in the previous versions of the site, “cells” were created to hold different content. In this template, you can define the layout of the pages, including the cells and define them as different places to include content on specific pages.

You’ll use this template later on for the Ngunnawal site.
