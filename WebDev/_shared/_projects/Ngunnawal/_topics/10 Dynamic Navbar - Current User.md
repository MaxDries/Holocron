---
tags:
  - "2022"
  - archived
---


In this instance, ‘dynamic navbar’ means that the navbar only shows links that are relevant or accessible that the current user can access.

For instance, if the user is not logged in, they will need the login link, however a user already logged in, will need a logout link instead.

---

`current_user` is built into `Flask-Login` allowing you to check to see if the user of the website is logged in or not, and if they are, then access the `User` model you defined in `models.py`. Hence using `user=current_user` on every route.

As the current user details is being sent to the template to be rendered, you can run some checks on that user.

Open `templates/base.html` and find the registration and login links.

![[Screen_Shot_2022-09-19_at_9.17.10_am.png|Screen Shot 2022-09-19 at 9.17.10 am.png]]

Surrounding these two links, add an `if` statement to check to see if the user is anonymous. This means that the user has **not** logged in yet.

![[Screen_Shot_2022-09-19_at_9.18.45_am.png|Screen Shot 2022-09-19 at 9.18.45 am.png]]

```python
{% if user.is_anonymous %}
...
{% endif %}
```

Test this code and you’ll notice that the Registration and Login links only show for users who are not logged in!

The next step is to extend this by adding in an {% else %} section to only show the links that are accessible for users who are logged in.

In the example shown, you can see that the **Registration** and **Login** links are shown if the user is not logged in (`is_anonymous`) and the **Profile**, **Photos**, **Logout** and **Reset Password** Links are shown if the user is logged in (`is_anonymous` is false)

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 3.png|Untitled]]
