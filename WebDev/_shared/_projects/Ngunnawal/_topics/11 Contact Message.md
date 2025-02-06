---
tags:
  - "2022"
  - archived
---


So far, users can send messages through the contact form, however the only way to view those messages is to access the database directly, which is not user-friendly and is a security issue. The next logical step is to build a page to dynamically load the table and display the messages to the user through the webpage.

## Route

Open `app.py` and create a new route.

![[week14ContactRoute.png|Screen Shot 2022-09-05 at 10.25.29 pm.png]]

```python
@app.route('/contact_messages')
@login_required
def view_contact_messages():
```

Load all the entries in the contact table and store them in a variable.

> [!info]  The code `Contact.query.all()` reads the entire Contact table from the database and stores it in the `contact_messages` variable.


![[week14ContactQueryAll.png|Screen Shot 2022-09-05 at 10.27.27 pm.png]]

```python
contact_messages = Contact.query.all()
```

Using this `contact_messages`, send the messages to a template page called `contactMessages.html`.

![[week14ContactTemplate.png|Screen Shot 2022-09-05 at 10.08.56 pm.png]]

```python
return render_template("contactMessages.html", title="Contact Messages", user=current_user, messages=contact_messages)
```

 

## Contact Messages Template

Duplicate `index.html` and name the new file `contactMessages.html`. 

![[WebDev/_shared/_projects/Ngunnawal/_images/Untitled 6.png|Untitled]]

To display the contact messages from the database, output the user's name, email address, message and the date it was submitted. This can be done using the bootstrap grid system as previously used.

Remove the contents of the `rowTwoColTwoContent` block and add the new `div`.

![[week14ContactTemplateContainer.png|Untitled]]

```python
<div class="container-fluid">
		
</div>
```

To create a structure that will create as many rows as needed to display the whole table, the temples can dynamically create rows using a loop.

This **for** loop will iterate over the entries in `messages` (which is the `contact` table). Each time the loop iterates, it’s accessing a different record in the table.

> [!info]  The first time, it will be accessing the first entry in the table, the second time it will access the second entry etc.


![[week14ContactTemplateLoop.png|Untitled]]

```python
<div class="container-fluid">
	{% for message in messages %}

	{% endfor %}
</div>
```

Each entry will be a row, so a new `div` tagged with `row` is needed inside the for loop.

![[week14ContactTemplateContainerRow.png|Untitled]]

```html
<div class="row">

</div>
```

Output each of the fields in the record (except for the ID) in different columns.

![[week14ContactTemplateContainerRowFields.png|Untitled]]

```html
<div class="col-3"> {{ message.name }}</div>
<div class="col-3"> {{ message.email }}</div>
<div class="col-3"> {{ message.message }}</div>
<div class="col-3"> {{ message.dateSubmitted }}</div>
```

## Test the route

Run the site and login. Modify the URL by adding `/contact_messages` to the end.

You should see a page similar to this (with different messages).

![[week14ContactDemo.png|Untitled]]

## Extension

The Date Submitted output isn’t very user-friendly. How could this be improved?

- Answer
	
	Update `contactMessages.html` to change how the date is outputed by placing `.strftime('%H:%M - %d/%m/%Y')` after the `datetime` output.
	
	![[week14ContactTemplateTimeFix.png|Screen Shot 2022-09-07 at 12.45.06 pm.png]]
	

How could the administrator control which messages they’ve seen or responded to? What would need to be added to the database and webpage? 
