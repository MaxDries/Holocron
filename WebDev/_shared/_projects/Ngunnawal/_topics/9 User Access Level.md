---
tags:
  - "2022"
  - archived
---


So far in the development process, there is very little access control. 

> [!info]  Access control is the term to cover which users can access certain pages and other resources.


A Quick Summary of what is implemented so far: 

- In the User table in the database, there is a `user_level` field, which is defined as an integer.
- A 1 in this field means standard user. A 2 means the user is an administrator.
- The User model has a function - `is_admin()` - which will return `True` if the user_level is set to 2.
- Some routes have `@login_required` defined which requires the user to be logged in or will redirect the user to the login page.

![[week14UserAccessLevel.png|Screen Shot 2022-09-05 at 9.40.44 pm.png]]

This is not *great* security and can use a lot of work. 

Ideally, only Administrators will be able to access certain pages of the website, and standard users will be redirected if they attempt to access those resources. 

For instance, only administrators should be able to view all the messages on the [Contact Messages](https://www.notion.so/Contact-Messages-727997aeeb73406a91fb930af1d5a6cd?pvs=21) Page. Currently, **any** logged-in user can view that page (if they know the link)

## Update Contact Messages Route

Open `app.py` and find the `contact_messsages` route.

![[week14ContactRouteUpdateStart.png|Screen Shot 2022-09-05 at 10.40.48 pm.png]]

To implement access control, before the Contact table is loaded and the template is rendered, first implement an `if` statement to check to see if the current user is an admin.

If the user is an administrator (level 2) then the page will load as it did previously.

![[week14ContactRouteCheckAdmin.png|Screen Shot 2022-09-05 at 10.43.08 pm.png]]

```python
if current_user.is_admin():
```

Include an `else` clause to redirect the user to the home page if they are not an administrator. 

![[week14ContactRouteAdminElse.png|Screen Shot 2022-09-05 at 10.45.11 pm.png]]

```python
else:
	return redirect(url_for("homepage"))
```

## Update the Navbar

The `is_admin()` function can also be used in the base template to control which links administrators see vs regular users.

Currently there is a check to see if the current_user is logged in or not and displays the relevant links.

Add in a check to see if the user is an admin, and if so, show the link to the contact messages route.

![[Untitled.png]]

## Test the implementation

Try to view the site as a regular user and then and administrator.

To change the access level, you’ll need to view the User table, change the value in the `user_level` field and write the changes back to the database.

Attempt to access the URL without logging in, and it should redirect you to the login page.

Attempt to access the URL logged in as a user with an access level set to 1.

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 1.png|Untitled]]

Attempt to access the URL logged in as a user with an access level set to 2.

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 2.png|Untitled]]

## Continue Implementing Access Control

What other pages should be limited to administrators only? Any that need to be, just add the same structure as was implemented for the contact messages route:

```python
if current_user.is_admin():
	# Normal Route code. For administrators only.
else:
	return redirect(url_for("homepage"))
```
