---
tags:
  - "2022"
  - archived
---

The Password Reset form is relatively easy compared to other parts of the website. Registered Users can reset their passwords once they’re logged in. 

Apart from the usual steps to create the form class, the HTML page etc, there is a small addition to this process, which is the requirement that users **must** be logged in before they can access the page/route. If the users are not logged in, it will automatically redirect them.

> [!info] This is not a “forgot password” function. That would require access to an email server, which is difficult in a school environment.


# Password Reset Form

The form to reset the password is relatively simple, as it only requires a single input and a submit button.

Open `forms.py` and add a new class:

![[Screen_Shot_2022-08-08_at_11.05.49_pm.png|Screen Shot 2022-08-08 at 11.05.49 pm.png]]

```python
class ResetPasswordForm(FlaskForm):
	new_password = StringField('New Password', validators=[DataRequired()])
	submit = SubmitField('Submit')
```

That’s all there is to it at this stage!

# Password Reset HTML

As this is a simple form, the HTML file is relatively simple as well.

Create a new html file in the templates folder and name it `passwordreset.html`. Enter the contents.

You’ll notice that the code is extremely similar to the login and registration forms in structure. 

![[Screen_Shot_2022-08-08_at_11.16.48_pm.png|Screen Shot 2022-08-08 at 11.16.48 pm.png]]

```python
{% extends 'template.html' %}

{% block header %}
  <h1>{% block title %} {{ title }}{% endblock %}</h1>
{% endblock %}

{% block cellContent2 %}
	<form action="" method="post" novalidate>
		{{ form.hidden_tag() }}
	<p>
		Resetting Password for user: <div class = "focus">{{ user.email_address }}</div>
	</p>
		<p>
			{{ form.new_password.label }}<br>
			{{ form.new_password(size=32) }}<br>
			{% for error in form.new_password.errors %}
			<span style="color: red;">[{{ error }}]</span>
			{% endfor %}
		</p>
		<p>{{ form.submit() }}</p>
	</form>
{% endblock %}
```

# Password Reset Route

Open `app.py` and as done previously before creating the route, import the new password reset form near the top of the file.

Look for the `from forms...` line of code and add `ResetPasswordForm` to the end.

![[Screen_Shot_2022-08-08_at_11.21.24_pm.png|Screen Shot 2022-08-08 at 11.21.24 pm.png]]

Create the new route to load the form and return the HTML file just created.

![[Screen_Shot_2022-08-08_at_11.22.50_pm.png|Screen Shot 2022-08-08 at 11.22.50 pm.png]]

```python
@app.route('/reset_password', methods=['GET', 'POST'])
def reset_password():
	form = ResetPasswordForm()
	return render_template("passwordreset.html", title='Reset Password', form=form, user=current_user)
```

At this point, you can test the form to see if it appears by running the project and accessing the url: [http://127.0.0.1:5000/reset_password](http://127.0.0.1:5000/reset_password)

![[Screen_Shot_2022-08-08_at_11.24.03_pm.png|Screen Shot 2022-08-08 at 11.24.03 pm.png]]

The remaining code to update the password is very similar to other code, such as register or any time data has been submitted to the database.

This code finds the user in the database (filtering by the username and comparing it to the logged in user). The code then runs the `set_password` function, passing it the password entered. 

Finally it commits the changes to the database and sends the user back to the home page.

![[Screen_Shot_2022-08-24_at_12.34.13_pm.png|Screen Shot 2022-08-24 at 12.34.13 pm.png]]

```python
if form.validate_on_submit():
	  user = User.query.filter_by(email_address=current_user.email_address).first()
	user.set_password(form.new_password.data)
	db.session.commit()
	return redirect(url_for('homepage'))
```

Test out the process. Log into the site, access the page and reset the users password. Log out and log back in again.

# Requiring Users to be logged in

Obviously, this page serves no purpose for users who are not logged into the site. Luckily Flask has a helper function for routes that require users to be logged in before the code runs. This is also useful for security.

Open `app.py` and add `login_required` to the imports for `flask_login`.

![[Screen_Shot_2022-08-08_at_11.37.53_pm.png|Screen Shot 2022-08-08 at 11.37.53 pm.png]]

At the `reset_password` route, add the `@login_required` decorator after the `@app.route` decorator and before the function definition.

![[Screen_Shot_2022-08-08_at_11.40.20_pm.png|Screen Shot 2022-08-08 at 11.40.20 pm.png]]

You can now test the update by attempting to access the page after logging out the current user.

You’ll notice that the site automatically redirects the user to the login page with the url [http://127.0.0.1:5000/login?next=%2Freset_password](http://127.0.0.1:5000/login?next=%2Freset_password). What does this do?

# Update Navbar

The route works and is configured correctly. The URL can now be added to the navbar.

Open `templates/template.html` and search for the navbar code. 

Add the link to the section of the navbar that only displays if the users are logged in. I.e. in the same block as the logout link.

![[Screen_Shot_2022-08-08_at_11.45.41_pm.png|Screen Shot 2022-08-08 at 11.45.41 pm.png]]

![[Screen_Shot_2022-08-08_at_11.46.04_pm.png|Screen Shot 2022-08-08 at 11.46.04 pm.png]]

# Extension: Confirm password

Extend this functionality but requiring the user to enter the password in twice into the form before the password is changed. 

HINT: Look to the user registration process for information on how to do this.