---
tags:
  - "2022"
  - archived
---


Messaging is important to any User Interface - users need feedback to confirm if operations have been successful or not. 

**Flash** is a addon module for Flask which allows for messages to be sent back to the user. The message is only valid for the *next* page displayed that contains a flash block.

Luckily the project being built allows for a easy implementation of a flash message system. The template.html can be updated to display any flash messages, and existing routes can add in confirmation messages with a single line of code.

# Import flash

Open `app.py` and add `flash` to the `from flask import….` line of code.

![[Screen_Shot_2022-08-09_at_9.50.41_am.png|Screen Shot 2022-08-09 at 9.50.41 am.png]]

# Update Routes

Update routes where feedback is important to the user. For instance, the password reset route. It would be useful for the user to get confirmation that the password has been changed.

To add a flash message, the command is: `flash("Message")`. 

> [!info]  As a rule of thumb, add the flash statements near the end of the process but before any return statement, as shown here.


![[Screen_Shot_2022-08-09_at_9.52.45_am.png|Screen Shot 2022-08-09 at 9.52.45 am.png]]

Add appropriate messages to other routes, such as `loggout`, `contact`, and `User Registration`. 

# Update Template

Open `templates/base.html` and decide where you want the messages to appear. They could appear just under the navbar, or in another specific place on the website. It is up to you where they will appear.

Wherever the messages are to appear in your code, add the following code block.

```
{% with messages = get_flashed_messages() %}
	{% if messages %}
		<ul class="flashes">
			{% for message in messages %}
				<li><div class="focus">{{ message }}</div></li>
			{% endfor %}
		</ul>
	{% endif %}
{% endwith %}
```

> [!info]  Make sure you don’t put your message output into a block that will be overridden.


This example shows that messages will appears just below the navbar.

![[Screen_Shot_2022-08-09_at_10.18.13_am.png|Screen Shot 2022-08-09 at 10.18.13 am.png]]

# Flash CSS

Currently, the messages appear very simply, as a dot list. Ideally the messages need to draw the users attention, so should stand out. 

Open `static/site.css` and add the following CSS

```css
.flashes {
	color: red;
	list-style: none
}
```

Test out a flash message and you should see it appear similar to the one shown.

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 14.png|Untitled]]

You can expand on this CSS to make the messages stand out even more by updating the CSS, as shown here.

Modify the CSS to make the messages appear the way you wish to. 

> [!info]  Find CSS code on the web, or use generative AI, to assist you.



```css
.flashes {
	padding: 12px;
	border-radius: 3px;
	font-size: 1.2rem;
	margin-bottom: 16px;
	border: 2px solid orange;
	background-color: yellow;
	color: black;
}
```

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 15.png|Untitled]]

# Demonstration

![[2022-08-09_10-19-24.2022-08-09_10_22_39.gif|2022-08-09 10-19-24.2022-08-09 10_22_39.gif]]
