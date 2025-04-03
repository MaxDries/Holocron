---
tags:
  - "2022"
  - archived
---



This section demonstrates how to create a profile page where the user can see details about their account. This page will later be updated to allow users to upload photos, see what photos they’ve uploaded and delete photos.

# Version 1 - Read Only

## User Profile Page

Create a new HTML file called `userProfile.html`. Replace the contents with the default code for the site.

```html
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

How the profile information is displayed is flexible, however all the fields in the user table can be displayed. As the site already has bootstrap included, a bootstrap grid system can be employed in `cellContent2` (or whichever ‘cell’ is appropriate for your site).

![[Screen_Shot_2022-09-02_at_1.31.31_pm.png|Screen Shot 2022-09-02 at 1.31.31 pm.png]]

```html
<div class="container text-left">

</div>
```

The contents of the user table can be displayed as a simple table, with rows for each field (email address, name etc) and two columns - the label and the data.

For instance, a row in the grid to display “User Name” and then the users email address would be entered as shown.

![[Screen_Shot_2022-09-02_at_1.41.40_pm.png|Screen Shot 2022-09-02 at 1.41.40 pm.png]]

```html
<div class="row">
	<div class="col-md-6">
		User Name
	</div>
	<div class="col-md-6">
		{{ user.name }}
	</div>
</div>
```

Create a row block for each of the fields in the user table. The end result will look similar to this.

![[Screen_Shot_2022-09-02_at_1.44.43_pm.png|Screen Shot 2022-09-02 at 1.44.43 pm.png]]

```html
{% block cellContent2 %}
	<div class="container text-left">
		<div class="row">
			<div class="col-md-6">
				User Name
			</div>
			<div class="col-md-6">
				{{ user.name }}
			</div>
		</div>
		<div class="row">
			<div class="col-md-6">
				Email Address
			</div>
			<div class="col-md-6">
				{{ user.email_address }}
			</div>
		</div>
		<div class="row">
			<div class="col-md-6">
				Password Hash
			</div>
			<div class="col-md-6">
				{{ user.password_hash }}
			</div>
		</div>
		<div class="row">
			<div class="col-md-6">
				User Level
			</div>
			<div class="col-md-6">
				{{ user.user_level }}
			</div>
		</div>
	</div>
{% endblock %}
```

## Route

Open `app.py` and create a new route for the user profile.

The route is set to GET and POST for future use, where the page could be changed to allow users to edit their details instead of just viewing.

The route is also set to require the user to be logged in, otherwise there could be a security breach, or crashing the page.

![[Screen_Shot_2022-09-02_at_1.49.21_pm.png|Screen Shot 2022-09-02 at 1.49.21 pm.png]]

```python
@app.route('/profile', methods=['GET', 'POST'])
@login_required
def profile():
	return render_template("userProfile.html", title="User Profile", user=current_user)
```

## Navbar updates

Open `templates/template.html` and add a new navbar entry in the section with Logout and Reset Password. This is the section of the code which only displays when the user is logged in.

![[Screen_Shot_2022-09-02_at_1.51.55_pm.png|Screen Shot 2022-09-02 at 1.51.55 pm.png]]

```html
<li class="nav-item">
	<a class="nav-link" href="/profile">Profile</a>
</li>
```

## Run and Test

Run the project, log in and access the User Profile link.

![[Screen_Shot_2022-09-02_at_1.52.49_pm.png|Screen Shot 2022-09-02 at 1.52.49 pm.png]]

# Version 1.5 - User Interface Updates

Showing the user their `user_level` is not useful as standard users will not know what this number represents. It would be better to output information that would be more useful to the user. 

Instead of simply outputting `user.user_level` you can first check what value it is and output a string which represents that level. For instance, level 1 then the user is an Administrator. 

In `userProfile.html` replace `user.user_level` with an if statement and appropriate output.

![[Screen_Shot_2022-09-05_at_9.56.52_am.png|Screen Shot 2022-09-05 at 9.56.52 am.png]]

```python
{% if user.user_level==1 %}
	Administrator
{% else %}
	User
{% endif %}
```

When the profile page is accessed now, it should show the more user friendly output.

![[Screen_Shot_2022-09-05_at_9.57.57_am.png|Screen Shot 2022-09-05 at 9.57.57 am.png]]

Additionally, showing the password hash is not helpful to the user at all. It would be more useful to change that entry to be a link to the reset Password page.

![[Screen_Shot_2022-09-05_at_10.02.17_am.png|Screen Shot 2022-09-05 at 10.02.17 am.png]]

```html
<a href="/reset_password">Reset Password</a>
```

When the site is run, you should see this

![[Screen_Shot_2022-09-05_at_10.02.11_am.png|Screen Shot 2022-09-05 at 10.02.11 am.png]]

# Version 2 - Editable Data

To enable the user to edit their data, you can allow the user to directly edit their profile data through this page. To do so, you’ll need to create a form to collect the data and then write the data back to the database. It’s essentially the same process as user registration, except the data will be updated instead of a new user being created.

You will need to decide what data the users can edit. For instance, you may want the users to be able to edit their name, but not their email address or password. This will depend on the requirements of the system.

## Form

Open `forms.py` and create a new `UserProfileForm` class.

Include the fields that you want the user to be able to edit.

![[Screen_Shot_2022-09-05_at_9.02.01_am.png|Screen Shot 2022-09-05 at 9.02.01 am.png]]

```python
class UserProfileForm(FlaskForm):
	email_address = StringField("Email Address (Username)", validators=[DataRequired(), Email()])
	name = StringField("Full Name", validators=[DataRequired()])
	submit = SubmitField("Update Profile")
```

## Update Model

In order to be able to edit the details in the database, you will need to update the User Model to allow for those changes.

Add a update_details() function to the User model to update only the fields that you wish to allow the users to update.

> [!info]  This function may need different variables depending on which fields are allowed to be changed.


![[Screen_Shot_2022-09-05_at_10.20.51_am.png|Screen Shot 2022-09-05 at 10.20.51 am.png]]

```python
def update_details(self, email_address, name):
	self.email_address = email_address
	self.name = name
```

## Update userProfile.html

Currently `userProfile.html` only displays the information, rather than the form, so this page will need to be updated.

For instance, the Users name is displayed using the following code.

![[Screen_Shot_2022-09-05_at_9.04.32_am.png|Screen Shot 2022-09-05 at 9.04.32 am.png]]

Whereas the form code, taken from registration.html looks like this.

![[Screen_Shot_2022-09-05_at_9.05.41_am.png|Screen Shot 2022-09-05 at 9.05.41 am.png]]

Therefore in `userProfile.html` each field that is determined to be updatable, the simple **display** will need to be replaced  with the **form** code.

For instance:

![[Screen_Shot_2022-09-05_at_9.08.10_am.png|Screen Shot 2022-09-05 at 9.08.10 am.png]]

# Update route

Open `app.py` and import `UserProfileForm` from forms.

![[Screen_Shot_2022-09-05_at_9.48.00_am.png|Screen Shot 2022-09-05 at 9.48.00 am.png]]

Find the `profile()` route. Create the `form` variable, loading the `UserProfileForm`, and then send the form to the template.

![[Screen_Shot_2022-09-05_at_9.52.45_am.png|Screen Shot 2022-09-05 at 9.52.45 am.png]]

When the site is run now, the form should appear.

![[Screen_Shot_2022-09-05_at_9.59.21_am.png|Screen Shot 2022-09-05 at 9.59.21 am.png]]

## Update Route

Open `app.py` and add the following code to the route to update the database record.

 > [!info] You’ll need to change the user.update_details() command to match the variables needed by the model.


![[Screen_Shot_2022-09-05_at_10.29.09_am.png|Screen Shot 2022-09-05 at 10.29.09 am.png]]

```python
if form.validate_on_submit():
	user = User.query.filter_by(email_address=current_user.email_address).first()
	user.update_details(email_address=form.email_address.data, name=form.name.data)
	db.session.commit()
	flash("Your details have been changed")
	return redirect(url_for("homepage"))
```

# Version 2.5 - User Interface Updates

One of the issues with the current system is that the user can’t see their current details in the form.

To do so, the code first needs to load the current data from the database, and then pre-populate the data into the form. The first step is to move the line of code which loads the current user to outside the `form.validate_on_submit()` block.

![[Screen_Shot_2022-09-05_at_11.08.53_am.png|Screen Shot 2022-09-05 at 11.08.53 am.png]]

After loading the data, the code should then check if the page being loaded (not submitted). If it is, then fill in the data into the form.

![[Screen_Shot_2022-09-05_at_11.11.00_am.png|Screen Shot 2022-09-05 at 11.11.00 am.png]]

```python
if request.method == 'GET':
	form.name.data = user.name
	form.email_address.data = user.email_address
```
