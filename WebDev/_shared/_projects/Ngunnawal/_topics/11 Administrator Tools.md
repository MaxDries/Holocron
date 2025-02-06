---
tags:
  - "2022"
  - archived
---



This section of the project focuses on Administrator functionality. 

It is *generally* a bad idea to allow anyone, apart from a core group of people, direct access to the database, which means that most functions need to be configured to be allowed through a web interface. 

Administrator functionality that would require a web interface could be:

- Resetting other users passwords
- Removing or disabling user accounts
- Removing or disabling photos uploaded
- etc.

## Navbar updates

Update the navbar component in `base.html` to add links to the administrator functionality.

Look for the `{% if user.is_admin() %}` code and replace the existing links to be a dropdown menu.

![[week14TemplateNavbarAdmin.png|Untitled]]

```html
<li><a class="nav-link" href="/contact_messages">Contact Messages</a></li>
<li><a class="nav-link" href="/admin/list_all_users">List all Users</a></li>
```

## Reseting User Passwords

## Listing All Users

To be able to access individual users to reset their passwords, a new route can be added to list all the users current in the system and providing a link to the direct url, such as : [http://127.0.0.1:5000/reset_password/6](http://127.0.0.1:5000/reset_password/6)

Before creating the route code, create the structure for the HTML template to load.

Create a new HTML file in the templates folder called `listAllUsers.html` and include the following code.

```python
{% extends 'base.html' %}

{# Place the content for the cellContent1 in this block #}
{% block rowOneContent %}

{% endblock %}

{% block rowTwoColOneContent %}
  
{% endblock %}

{# Place the content for the cellContent2 in this block #}
{% block rowTwoColTwoContent %}
  
{% endblock %}

{% block rowThreeContent %}
  <div class="footerText">Copyright 2024.</div>
{% endblock %}
```

In the `rowTwoColTwoContent` block add the code to loop over the number of users given, and automatically generating URLs for the reset password functionality defined below.

Save the file.

![[week14ListUsers.png|Screen Shot 2022-10-21 at 10.32.50 pm.png]]

```html
<div class="container-fluid">
	{% for user in users %}
		<div class="row">
			<div class="col-md-4">{{ user.email_address }}</div>
			<div class="col-md-4"><a href="/reset_password/{{ user.id }}">Reset Password</a></div>
		</div>
	{% endfor %}
</div>
```

Open `app.py` and Start by defining the route.

![[week14UserListRoute.png|Screen Shot 2022-10-21 at 10.20.10 pm.png]]

```python
@app.route('/admin/list_all_users')
@login_required
def list_all_users():
```

The code to load all the user details is simple enough, querying the entire user table, however this route needs to be limited so that only administrators can access it. If a user accesses it who is **not** an administrator, it will redirect them back to the front page.

![[week14UserListAdminCheck.png|Screen Shot 2022-10-21 at 10.35.21 pm.png]]

```python
if current_user.is_admin():
	all_users = User.query.all()
	return render_template("listAllUsers.html", title="All Active Users", user=current_user, users=all_users)
else:
	flash("You must be an administrator to access this functionality.")
	return redirect(url_for("homepage"))
```

When the site is run and the url accessed, the administrator will see a page similar to this:

![[week14UserListAdminDemo.png|Screen Shot 2022-10-21 at 10.36.13 pm.png]]

## Reset User Password Route

The site already allows for the user to reset their own password. The process for administrators to reset other peoples passwords is very similar.

> [!info] Consider the reset password function in `app.py` that already has been coded.
![[passwordresetRoute.png|Untitled]]


To allow administrators to reset other peoples passwords, you can reuse `ResetPasswordForm` and the majority of the code shown with slight tweaks. The main one of these tweaks is the addition of the `userid` as a variable in the route and the function.

Enter the function in `app.py`. 

```python
@app.route('/reset_password/<userid>', methods=['GET', 'POST'])
@login_required
def reset_user_password(userid):
	form = ResetPasswordForm()
	user = User.query.filter_by(id=userid).first()
	if form.validate_on_submit():
		user.set_password(form.new_password.data)
		db.session.commit()
		flash('Password has been reset for user {}'.format(user.name))
		return redirect(url_for('homepage'))
	return render_template("passwordreset.html", title='Reset Password', form=form, user=user)
```

The differences between the existing reset password route and this one can be seen here.

![[week14AdminResetPasswordRoute.png|Screen Shot 2022-10-21 at 10.09.34 pm.png]]

Notice the `/reset_password/<userid>` in the route and `reset_user_password(userid)` in the function header. This allows for the browser to access a URL such as [http://127.0.0.1:5000/reset_password/6](http://127.0.0.1:5000/reset_password/6). Where the `6` indicates the user id to be accessed as can be seen below.

![[week14ResetPasswordAdminDB.png|Screen Shot 2022-10-21 at 10.05.04 pm.png]]

Additionally, the User.query code has been moved to be run before the if statement. This is so that the user details can be loaded to display on screen (as indicated in the render_template code with `user=user`). This allows the intended users email address to be shown instead of the `current_user` details.

Test the functionality to ensure it works. 

> [!info] It may be useful to register a test account to practice the resetting of passwords on.


## Disabling User Accounts

User accounts can be disabled or deleted for any number of reasons - an account could have been made in error, legal issues, or the user just wishes to get rid of the account for some reason. Whatever the reason, it may be better to *disable* the account rather than delete it. 

To enable the disabling of accounts will require a few changes throughout the system:

1. The Login process needs to only allow users to login with active accounts.
2. User Registration needs to default active to true.
3. Administrators need to be able to enable and disable accounts as needed.

There may be other changes required, however these are the main ones.

> [!info]  The Database and model already has support for enabling and disabling of user accounts through the `active` field.


### Login

The login process does not need to be changed significantly - the only addition is a check to ensure the user account is active (the field is `1`) before the login process proceeds.

This is only a minor change to the existing code.

> [!info]  It would be useful to add a flash message to the user if the login process fails, instead of just redirecting them back to the user page.


![[week14UserNotActive.png|Screen Shot 2022-10-23 at 10.31.27 pm.png]]

### Administrator Control

To allow administrators to enable and disable user accounts, there are a number of approaches to take.

**One** easy method is to update the `listAllUsers.html` file to show if the account is enabled or disabled, and to generate a link to flip the account status (enabled→disabled or disabled→enabled).

Save the file.

![[week14TemplateCheckUserActive.png|Screen Shot 2022-10-23 at 11.29.01 pm.png]]

```html
<div class="col-md-3">
	{% if user.active %}
		Enabled
	{% else %}
		Disabled
	{% endif %}
</div>
<div class="col-md-3"><a href="/admin/user_enable/{{ user.id }}">Enable/Disable Account</a></div>
```

Open [`app.py`](http://app.py) and create a new route for `/admin/user_enable`

This route loads the user identified by their user id, then flips the status. The changes are written back to the database.

Notice that this route doesn’t display a template to the user, just redirects them back to the `list_add_users` route. The user performing this function will see the changes reflect in the user list.

![[week14AdminDisableUser.png|Screen Shot 2022-10-23 at 11.27.05 pm.png]]

```python
@app.route('/admin/user_enable/<userid>')
@login_required
def user_enable(userid):
	user = User.query.filter_by(id=userid).first()
	user.active = not user.active
	db.session.commit()
	return redirect(url_for("list_all_users"))
```

### Extension

Update the `user_enable` route to limit access to administrators only. 

Hint: Look at the `list_all_users` route for suggestions.

Open `models.py` and add a new variable to the `User` class to reflect the new addition to the database.

> [!info] Create an account to test the process - check the database to ensure that the new account has been created correctly.


Similarly to login, the registration process only needs a small addition to make the account active by default.
