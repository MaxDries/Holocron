---
tags:
  - "2022"
  - archived
---


# Update the Template

Before delving into the user registration code, an easy place to start is to update the template.html file to link to the registration route that will be created later in the process.

Open `templates/template.html` . Update the navbar code to include a new link which directs the user to `/register`.

![[Screen_Shot_2022-07-05_at_10.45.46_pm.png|Screen Shot 2022-07-05 at 10.45.46 pm.png]]

```python
<a href="register">User Registration</a>
```

When the site is next run, the User Registration link should appear on all pages based on this template.

![[Screen_Shot_2022-07-05_at_10.46.33_pm.png|Screen Shot 2022-07-05 at 10.46.33 pm.png]]

# Create User Table

The first step in the process of developing the functionality for users to register is to decide what data needs to be stored in the database.

For the purposes of this module, the data that will be stored will be:

| Data | Data Type | Description |
| --- | --- | --- |
| ID | Integer | Auto-incrementing, Primary Key. Unique identifier for each user. |
| Email Address | Text | Unique. Users email address that will double as their “username” |
| Name | Text | Users Full Name |
| Password_hash | Text | The password stored securely as a “hash”. This is explained later. |
| UserLevel | Integer | 1=Normal User
2=Administrator |

## Create the Table

Right-click on the Database in Pycharm and choose New Table. Call the table `user`.

![[Screen_Shot_2022-07-05_at_8.46.18_pm.png|Screen Shot 2022-07-05 at 8.46.18 pm.png]]

Create the fields as needed.

> [!info]  Which ever field is going to be the users “username”, set that to be Unique in the database creation process.
> ![[Screen_Shot_2022-07-05_at_8.48.41_pm.png|Screen Shot 2022-07-05 at 8.48.41 pm.png]]


![[Screen_Shot_2022-08-03_at_9.16.36_pm.png|This is only an example. Create the fields as you need for the specific implementation requirements.]]

This is only an example. Create the fields as you need for the specific implementation requirements.

The table should be created similar to this example.

![[Screen_Shot_2022-08-03_at_9.17.05_pm.png|Screen Shot 2022-08-03 at 9.17.05 pm.png]]

# User Model

Open `models.py` to create the User Class. Unlike other models, this model needs to be implemented slightly differently because Users have much more default functionality that needs to be supported - such as login and log off. Flask has some helper functions that assists with these types of processes.

At the top of `models.py` add the following import statements.

![[Screen_Shot_2022-07-05_at_9.33.20_pm.png|Screen Shot 2022-07-05 at 9.33.20 pm.png]]

```python
from flask_login import UserMixin
from werkzeug.security import generate_password_hash, check_password_hash
```

> [!info] Remember the class has to match the specific implementation of the database, not necessarily a direct copy from here.


The code for the user class is shown here.

![[Screen_Shot_2022-08-03_at_9.17.52_pm.png|Screen Shot 2022-08-03 at 9.17.52 pm.png]]

```python
class User(UserMixin, db.Model):
	id = db.Column(db.Integer, primary_key=True)
	email_address = db.Column(db.String(255), unique=True)
	name = db.Column(db.String(255))
	password_hash = db.Column(db.String(255))
	user_level = db.Column(db.Integer)
```

## Password Storage

User passwords is a tricky topic. So far, all the data stored in the database is being stored ‘in the clear’, meaning that if a human was to read the table data, they can read the unencrypted data.

Passwords should be encrypted before being stored in the database and if an unauthorised person gained access to the database, they would not be able to read the passwords and therefore still not be able to log in.

This process is called **password hashing**.

To read more about Password Hashing, see the following page.

> Password Hashing
> 

## Password Functions

The website application will need to perform two main password functions:

1. When a user registers, the application will need to encrypt the password using the hash function and store the hash in the database.
2. When a user logs into the site, the application will need to retrieve the password hash from the database, and compare it to the hash of what the user typed into the website in the password box.

In brief, the application will need to perform a hash many times. To assist with that, some specialised functions should be implemented.

The first sets the user password. This function takes the unencrypted password and sets the password_hash variable as the encrypted (hashed) version of the password.

The second compares the stored hash with an unencrypted password. This will be used when the user logs in.

Add the following functions to your User class.

![[Screen_Shot_2022-08-03_at_9.18.36_pm.png|Screen Shot 2022-08-03 at 9.18.36 pm.png]]

```python
def set_password (self, password):
		self.password_hash = generate_password_hash(password)

def check_password (self, password):
	return check_password_hash(self.password_hash, password)
```

## Is the user an administrator?

One of the fields shown in the example is `user_level`. This indicates whether the user is a normal user (1) or an administrator (2). 

> [!info]  This could have been achieved with a boolean (True/False), however this limits the application from implementing another user type, such as content manager with more access than a normal user, but not be able to edit user details.


A simple helper function can be added to the User class to return a true or a false indicating if the user is an administrator or not. This will help the system when implementing administrator functions later.

At this stage, the function can return true is the `user_level` value is a 1 and false for everything else.

> [!info] Implementing the function, rather than coding the remainder of the site to check `user_level` means that if this needs to be changed at a later stage (say, and administrator changes to 10), then only the `is_admin()` function needs to be changed, not all throughout the code.


![[Screen_Shot_2022-08-03_at_9.19.09_pm.png|Screen Shot 2022-08-03 at 9.19.09 pm.png]]

```python
def is_admin(self):
  if self.user_level == 2:
	  return True
  else:
	  return False
```

# Registration Form

The register form needs to collect the information needed to create a User entry in the database.  The form doesn’t need to collect the `id` as this is automatically generated.

Depending on the system requirements, the fields may be required or not. Modify the form specifications as needed.

Open `forms.py`. 

Add a new `PasswordField` to the `wtforms` imports to enable the password to be hidden.

![[Screen_Shot_2022-07-05_at_10.48.58_pm.png|Screen Shot 2022-07-05 at 10.48.58 pm.png]]

Add a new validator in the import statements, this one `EqualTo`.

![[Screen_Shot_2022-07-05_at_9.53.46_pm.png|Screen Shot 2022-07-05 at 9.53.46 pm.png]]

The RegistrationForm class will be similar to this.

> [!info]  Remember to match the form to the requirements of the database!


Collecting the password is done a little differently, as per standard practice, the user should enter the password twice to confirm the passwords are identical.

![[Screen_Shot_2022-07-05_at_10.50.25_pm.png|Screen Shot 2022-07-05 at 10.50.25 pm.png]]

```python
class RegistrationForm(FlaskForm):
	email_address = StringField("Email Address (Username)", validators=[DataRequired(), Email()])
	name = StringField("Full Name", validators=[DataRequired()])
	password = PasswordField("Password", validators=[DataRequired()])
	password_confirm = PasswordField("Confirm Password", validators=[DataRequired(), EqualTo("password")])
	submit = SubmitField("Register")
```

## Unique Users

One issue with users self-registering is that the system should not accept duplicated email addresses, or user names, as this is used to identify the user. Another helper function is required to check if the same email address exists **before** registration can continue.

Import the User model into the `forms.py` file.

![[Screen_Shot_2022-07-05_at_9.59.52_pm.png|Screen Shot 2022-07-05 at 9.59.52 pm.png]]

```python
from models import User
```

Also add a new Validator - `ValidationError`

![[Screen_Shot_2022-07-05_at_10.02.30_pm.png|Screen Shot 2022-07-05 at 10.02.30 pm.png]]

Add the verification function to the RegistrationForm class.

![[Screen_Shot_2022-07-05_at_10.03.53_pm.png|Screen Shot 2022-07-05 at 10.03.53 pm.png]]

```python
def validate_email_address(self, email_address_to_register):
  user = User.query.filter_by(email_address=email_address_to_register.data).first()
  if user is not None:
	  raise ValidationError("Please Use a Different Email Address)")
```

# Registration Template

Create a new HTML page in the `templates` folder, called `registration.html` and copy in the standard starting code.

The HTML template will again be based on the main site `template.html`. 

```python
{% extends 'template.html' %}

{# Place the content for the cellContent1 in this block #}
{% block cellContent1 %}
	Sidebar
{% endblock %}

{# Place the content for the cellContent2 in this block #}
{% block cellContent2 %}
	Main Content
{% endblock %}

{# Place the content for the cellContent3 in this block #}
{% block cellContent3 %}
	Footer
{% endblock %}
```

## Form Structure

Replace the Main Content text with the structure for the form.

![[Screen_Shot_2022-07-05_at_10.16.23_pm.png|Screen Shot 2022-07-05 at 10.16.23 pm.png]]

```python
<form action="" method="post">
  {{ form.hidden_tag() }}
	
	
</form>
```

## Form Fields

Similar to previous forms, you will need to create a copy of this block of code for each field that is being displayed - matching the RegistrationForm class defined earlier. 

The only parts that need to change for each field are highlighted red.

![[Screen_Shot_2022-07-01_at_1.37.36_pm.png|Screen Shot 2022-07-01 at 1.37.36 pm.png]]

```python
<p>
	{{ form.name.label }}<br>
	{{ form.name(size=32) }}<br>
	{% for error in form.name.errors %}
	<span style="color: red;">[{{ error }}]</span>
	{% endfor %}
</p>
```

The final version will appear similar to this.

> [!info]  Remember: The form field names have to match **exactly** the variable names in the RegistrationForm class. For Example:
form.`email_address`.label


![[Screen_Shot_2022-07-05_at_10.19.28_pm.png|Screen Shot 2022-07-05 at 10.19.28 pm.png]]

```python
<form action="" method="post">
	{{ form.hidden_tag() }}

	<p>
		{{ form.email_address.label }}<br>
		{{ form.email_address(size=64) }}<br>
		{% for error in form.email_address.errors %}
			<span style="color: red;">[{{ error }}]</span>
		{% endfor %}
	</p>
	<p>
		{{ form.name.label }}<br>
		{{ form.name(size=64) }}<br>
		{% for error in form.name.errors %}
			<span style="color: red;">[{{ error }}]</span>
		{% endfor %}
	</p>
	<p>
		{{ form.password.label }}<br>
		{{ form.password(size=32) }}<br>
		{% for error in form.password.errors %}
			<span style="color: red;">[{{ error }}]</span>
		{% endfor %}
	</p>
	<p>
		{{ form.password_confirm.label }}<br>
		{{ form.password_confirm(size=32) }}<br>
		{% for error in form.password_confirm.errors %}
			<span style="color: red;">[{{ error }}]</span>
		{% endfor %}
	</p>
	<p>{{ form.submit() }}</p>
</form>
```

# Registration Route

The final step is to link all the parts together in the registration route.

Open `app.py` and update the forms import statement to include the the User model and RegistrationForm created.

![[Screen_Shot_2022-07-05_at_10.27.57_pm.png|Screen Shot 2022-07-05 at 10.27.57 pm.png]]

Update the flask import statement to also import `url_for`.

![[Screen_Shot_2022-07-05_at_10.35.28_pm.png|Screen Shot 2022-07-05 at 10.35.28 pm.png]]

Create the new Route.

This route will create the RegistrationForm in memory and pass it to the `registration.html` template.

![[Screen_Shot_2022-07-05_at_10.30.19_pm.png|Screen Shot 2022-07-05 at 10.30.19 pm.png]]

```python
@app.route("/register", methods=["GET", "POST"])
def register():
	form = RegistrationForm()

	return render_template("registration.html", title="User Registration", form=form)
```

Add the user to the database.

When the user presses the submit button, the code block in `form.validate_on_submit()` is run. This creates the new user in memory first, then sets the password (hashing it in the process by using the `set_password` helper function) and the writes it to the database.

![[Screen_Shot_2022-07-05_at_10.37.42_pm.png|Screen Shot 2022-07-05 at 10.37.42 pm.png]]

```python
if form.validate_on_submit():
	new_user = User(email_address=form.email_address.data, name=form.name.data, user_level=1) # defaults to regular user
	new_user.set_password(form.password.data)
	db.session.add(new_user)
	db.session.commit()
	return redirect(url_for("homepage"))
```

## Test Registration.

Run the project and visit [http://127.0.0.1:5000/register](http://127.0.0.1:5000/register).

![[Screen_Shot_2022-07-05_at_10.51.52_pm.png|Screen Shot 2022-07-05 at 10.51.52 pm.png]]

Register a user and then view the database (you may need to refresh the view). The new user should appear similar to this.

![[Screen_Shot_2022-07-05_at_10.33.58_pm.png|Screen Shot 2022-07-05 at 10.33.58 pm.png]]
