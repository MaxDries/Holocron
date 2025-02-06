---
tags:
  - "2022"
  - archived
---


> [!bug]  Prerequisite: the [[18 User Registration]] process being complete. Ensure users can register accounts before progressing.


The User Registration development process does much of the work for setting up the database and model, meaning this process is shorter.

# Update User Model

In order to perform the complex process of user login, we’re going to use the `flask_login` library.

One of the requirements of the flask_login system is a helper function to load the user information, specifically the ID of the currently logged in user.

Open `models.py`, and update the import statement to include login from app.

![[Screen_Shot_2022-08-24_at_12.23.28_pm.png|Screen Shot 2022-08-24 at 12.23.28 pm.png]]

Add the following code to the bottom of the `models.py` file.

![[Screen_Shot_2022-08-03_at_9.28.13_pm.png|Screen Shot 2022-08-03 at 9.28.13 pm.png]]

```python
@login.user_loader
def load_user(id):
	return User.query.get(int(id))
```

# Login Form

> [!info] These instructions are made assuming that there is only a need for a `email address` and a `password` in order to log in.


Open [forms.py](http://forms.py) and create a new class - `LoginForm`.

![[Screen_Shot_2022-08-03_at_3.52.53_pm.png|Screen Shot 2022-08-03 at 3.52.53 pm.png]]

Define the variables and fields that are required for the form. In this case, there should be a `email address` field and a `password` field. The password field is similar to a normal `StringField` except that it won’t display the letters entered, but hides the password when typing it in.

The submit entry will show as a button, similar to the Register button in the Registration process.

![[Screen_Shot_2022-08-03_at_3.57.11_pm.png|Screen Shot 2022-08-03 at 3.57.11 pm.png]]

```python
class LoginForm(FlaskForm):
	email_address = StringField('Email Address', validators=[DataRequired()])
	password = PasswordField('Password', validators=[DataRequired()])
	submit = SubmitField('Sign In')
```

# Login Page

Create a new HTML page in the templates folder called `login.html`.  This login page will structured similarly to the registration page, but won’t need to display as many fields. As discussed earlier, only two fields are needed.

Copy the contents of `registration.html` and remove the code to display the fields that aren’t required - `name` and `password_confirm`. For example, remove this block of code: 

```python
<p>
	{{ form.password_confirm.label }}<br>
	{{ form.password_confirm(size=32) }}<br>
	{% for error in form.password_confirm.errors %}
		<span style="color: red;">[{{ error }}]</span>
	{% endfor %}
</p>
```

The code to the login page will be similar to this:

![[Screen_Shot_2022-08-03_at_4.04.22_pm.png|Screen Shot 2022-08-03 at 4.04.22 pm.png]]

You can modify the login page to display however you would like.

# Route

Open [`app.py`](http://app.py).

Import the new `LoginForm` at the top of the file, extending the code which already imports the `RegistrationForm` (amongst others).

![[Screen_Shot_2022-08-03_at_4.11.21_pm.png|Screen Shot 2022-08-03 at 4.11.21 pm.png]]

Import the necessary libraries from `flask_login`. 

![[Screen_Shot_2022-08-22_at_9.23.57_am.png|Screen Shot 2022-08-22 at 9.23.57 am.png]]

```python
from flask_login import current_user, login_user, LoginManager, logout_user, login_required
```

Configure the login manager to use the new login system that is being designed.

![[Screen_Shot_2022-08-22_at_9.21.46_am.png|Screen Shot 2022-08-22 at 9.21.46 am.png]]

```python
login = LoginManager(app)
login.login_view = 'login'
```

Create a new route for `/login`. 

![[Screen_Shot_2022-08-03_at_4.08.31_pm.png|Screen Shot 2022-08-03 at 4.08.31 pm.png]]

Load the `LoginForm` and store it in a variable named `form`.

You’ll notice that the structure of these routes so far is fairly similar, especially when loading a form. This will assist you when creating your own additional functionality

![[Screen_Shot_2022-08-03_at_4.32.30_pm.png|Screen Shot 2022-08-03 at 4.32.30 pm.png]]

Check if the form has been submitted.

![[Screen_Shot_2022-08-24_at_12.26.14_pm.png|Screen Shot 2022-08-24 at 12.26.14 pm.png]]

If the form has been submitted (i.e. the user has entered an email address and password) the process should then attempt to load the user record from the database that has that email address.

This code uses the User model, and loads the first entry in the database where the email address stored in the table matches the email address entered in the form.

![[Screen_Shot_2022-08-24_at_12.27.18_pm.png|Screen Shot 2022-08-24 at 12.27.18 pm.png]]

```python
user = User.query.filter_by(email_address=form.email_address.data).first()
```

> [!info] There are three possible cases that need to be addressed:
> 1. The User is **not** found in the database.
> 2. The User is found in the database and the password **does not** match.
> 3. The User is found in the database and the password matches.
> 
> The code needs to cater for all three cases.


The code can easily check if the user is not found in the database (i.e. no match on the email address) by checking if the `user` variable is empty.

![[Screen_Shot_2022-08-03_at_4.21.51_pm.png|Screen Shot 2022-08-03 at 4.21.51 pm.png]]

At this stage, you, the developer, need to determine what is to occur to users who enter incorrect details. There are a number of possibilities, however at this stage, the code can simply reload the login page to prompt them again.

> [!info] This caters for Case #1 listed above.
> [!info] 

![[Screen_Shot_2022-08-03_at_4.23.39_pm.png|Screen Shot 2022-08-03 at 4.23.39 pm.png]]

Turning to Case #2, if the user enters an email address that is found in the database, but an incorrect password, the same outcome is wanted as Case #1 - the login page is reloaded. The `if user is None:` statement can be extended to check if the password is incorrect. This extra check will run the `check_password` function written in the User model. 

> [!info]  Notice that at no stage is the password stored in the code, in the clear. The password is taken from the form and then passed through the check_password function which runs the hash function to check against the database.


> [!info]  This caters for Case #2 listed above.


![[Screen_Shot_2022-08-03_at_4.26.55_pm.png|Screen Shot 2022-08-03 at 4.26.55 pm.png]]

Finally, the last case (Case #3) is if the email address and password are correct. If this is the case, you can utilise the pre-written function to log the user in. If the user successfully logs in, they will be redirected to the homepage *at this stage*.

![[Screen_Shot_2022-08-03_at_9.22.13_pm.png|Screen Shot 2022-08-03 at 9.22.13 pm.png]]

# Template Updates

The login page has been created, but is not accessible through the website. Update the navbar in templates/template.html to include the link to the newly created login page.

![[Screen_Shot_2022-08-03_at_9.32.32_pm.png|Screen Shot 2022-08-03 at 9.32.32 pm.png]]

One of the additional features that can be easily included is the ability to dynamically load information regarding the user who is currently logged in, such as their name.

As an example, open `templates/index.html` and in the main block, include the following code.

![[Screen_Shot_2022-08-03_at_9.48.32_pm.png|Screen Shot 2022-08-03 at 9.48.32 pm.png]]

```python
Welcome {{ user.name }}!
```

Save the file.

Open `app.py` and update the `return` statement to include the `current_user` variable to indicate the user who is currently logged in (if any).

![[Screen_Shot_2022-08-03_at_10.21.25_pm.png|Screen Shot 2022-08-03 at 10.21.25 pm.png]]

Log into the site and when the home page is loaded, the users name should appear.

![[Screen_Shot_2022-08-03_at_10.34.23_pm.png|Screen Shot 2022-08-03 at 10.34.23 pm.png]]

Now that this is proven to work, this `{{ user.name }}` can be added to the template, so it appears alongside the navbar. This means that the users name can be shown on any page they access.

![[Screen_Shot_2022-08-03_at_10.39.38_pm.png|Screen Shot 2022-08-03 at 10.39.38 pm.png]]

```html
<div style="color:white">{{ user.name }}</div>
```

As it is part of the template for all pages, all the other routes in `app.py` will need to also be updated to include the `user=current_user`, similar to the homepage.

![[Screen_Shot_2022-08-03_at_10.43.12_pm.png|Screen Shot 2022-08-03 at 10.43.12 pm.png]]

![[Screen_Shot_2022-08-03_at_10.42.43_pm.png|Screen Shot 2022-08-03 at 10.42.43 pm.png]]

- User Logout


Providing the functionality for logging out is relatively simple as the bulk of the hard work has already been done in the flask_login library.

# Logout User

In `app.py`, update the flask-login import line to include `logout_user`.

![[Screen_Shot_2022-08-03_at_10.45.54_pm.png|Screen Shot 2022-08-03 at 10.45.54 pm.png]]

Add a new Route for `/logout`. This route only needs to run the logout_user() function and then redirect the user to the homepage.

![[Screen_Shot_2022-08-03_at_10.46.15_pm.png|Screen Shot 2022-08-03 at 10.46.15 pm.png]]

```python
@app.route('/logout')
def logout():
	logout_user()
	return redirect(url_for('homepage'))
```

# Template Updates

Update the navbar in `templates/template.html` to include a link to the new `/logout` route

![[Screen_Shot_2022-08-03_at_10.50.36_pm.png|Screen Shot 2022-08-03 at 10.50.36 pm.png]]

Test it out!

![[2022-08-03_22-53-17.2022-08-03_22_53_59.gif|2022-08-03 22-53-17.2022-08-03 22_53_59.gif]]

# Dynamic Navbar Updates

One of the issues at this stage is the navbar shows both Login and Logout regardless of whether the user is logged in or not. It is possible, with some coding, to only display the option that is valid for the user status. For instance :

- If the user is not logged in (anonymous), then only show the **login** link.
- If the user is logged in, only show the **logout** link.

Open `templates/template.html`. 

Add an `if` check in the navbar and check to see if the user is anonymous or not.

![[Screen_Shot_2022-08-03_at_11.01.51_pm.png|Screen Shot 2022-08-03 at 11.01.51 pm.png]]

![[2022-08-03_22-58-58.2022-08-03_23_00_24.gif|2022-08-03 22-58-58.2022-08-03 23_00_24.gif]]
