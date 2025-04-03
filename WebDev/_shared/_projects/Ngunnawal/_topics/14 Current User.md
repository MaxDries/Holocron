---
tags:
  - "2022"
  - archived
---


Flask has a built-in variable called `current_user`. This variable stores the instance of the user that is currently logged in, for instance, their username, password_hash etc. But also has access to any custom functions written for the User model, such as `is_admin().`

This is incredibly useful for the Flask application as this user can be sent to the template to change behaviour. In this case, the nav bar is or will be changing the links based on whether the user is logged in or not. For this to occur, each template needs access to the `current_user` which in turn **requires each route to be updated to send the variable**.

Open `app.py` and **update EVERY route** **rendering a template** to include the additional argument `user=current_user**.**`

For instance, the homepage route needs to have the `render_template` line of code updated as shown.

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 13.png|Untitled]]
