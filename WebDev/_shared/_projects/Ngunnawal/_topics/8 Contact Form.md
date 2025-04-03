---
tags:
  - "2022"
  - archived
---

This task implements the back-end functIonality for the “contact us” page. To do this, there are a number of steps, and areas of the project to modify, namely the database, model, form and html. 

This process will be similar to the process for adding future functionality in the project. 

![[Flask_-_MVC.png|Flask - MVC.png]]

# Contact Page Configuration

Create a new HTML file in the templates folder called `contact.html` and replace the contents of the file with the standard code:

![[Screen_Shot_2022-07-01_at_10.16.31_am.png|Screen Shot 2022-07-01 at 10.16.31 am.png]]

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

Replace the Main Content text with form tags.

`form.hidden_tag()` is required for security purposes. It generates a CSRF token. You can read about Cross Site Request Forgery [here](https://owasp.org/www-community/attacks/csrf).

![[Screen_Shot_2022-07-01_at_1.30.52_pm.png|Screen Shot 2022-07-01 at 1.30.52 pm.png]]

```html
<form action="" method="post" novalidate>
{{ form.hidden_tag() }}

</form>
```

For each field required in the form, this template can be used.

The `name` field that is highlighted in the screenshot refers to variables that will be defined later. These **must** match the variables, and in each <p> block shown, they must be **identical**.

Create three of these blocks, the first for `name`, the second for `email` and the third for `message`.

![[Screen_Shot_2022-07-01_at_1.37.36_pm.png|Screen Shot 2022-07-01 at 1.37.36 pm.png]]

```html
<p>
	{{ form.name.label }}<br>
	{{ form.name(size=32) }}<br>
	{% for error in form.name.errors %}
	<span style="color: red;">[{{ error }}]</span>
	{% endfor %}
</p>
```

After the field, and just prior to the </form> add in the code for the submit button.

![[Screen_Shot_2022-07-01_at_1.41.23_pm.png|Screen Shot 2022-07-01 at 1.41.23 pm.png]]

```html
<p>{{ form.submit() }}</p>
```

At the end of the process, the code for the cell content should appear similar to this.

![[Screen_Shot_2022-07-01_at_1.41.51_pm.png|Screen Shot 2022-07-01 at 1.41.51 pm.png]]

Save the file.

# Database Table Creation

Previously the contact form was front-end only and had no functionality. To enable this functionality, a table in the database needs to be created for the data to be stored in.

The table fields that need to be created depend on the specific contact form being developed. The one in the example will need the following fields:

- Id
- Name
- Email Address
- Message
- Date Submitted

The `id **`and `Date Submitted` fields were not included in the contact form, but are required in the database regardless of what other fields you need.

> [!info] The instructions only show creating the fields indicated. If you have others, you will need to add these to your process.


Open the Database Tab in Pycharm.

![[Screen_Shot_2022-07-01_at_11.00.17_am.png|Screen Shot 2022-07-01 at 11.00.17 am.png]]

Right-click on the database name → New → Table.

![[Screen_Shot_2022-07-01_at_11.03.11_am.png|Screen Shot 2022-07-01 at 11.03.11 am.png]]

In the dialog that appears, name the table `contact`. 

Click on the plus button to add a new column/field and name this `id`. For this field, tick all the boxes.

> [!info]  Spelling & capitalisation is critical.


![[Screen_Shot_2022-07-01_at_11.06.05_am.png|Screen Shot 2022-07-01 at 11.06.05 am.png]]

Add the other fields to the table, for the other ones, **you do not need to tick any of the boxes.**

These options refer to specific restrictions for database tables, and entering data for data integrity, and not critical at this stage. These will be focused on and discussed in detail at another time.

> [!info]  Name all the fields as single words, and all lowercase. This makes it easier when coding the python side of the app.


![[2022-07-01_11-07-46.2022-07-01_11_09_55.gif|2022-07-01 11-07-46.2022-07-01 11_09_55.gif]]

Press **Execute** once completed. The tables will appear in the database.

You may need to press the refresh button to have the database update.

![[Screen_Shot_2022-07-01_at_11.11.44_am.png|Screen Shot 2022-07-01 at 11.11.44 am.png]]

The table has been created and now will be able to be accessed from within Flask.

# Create Models.py

> [!info]  In flask, and other programming architectures, a ‘model’ refers to the computer modelling the data in the code.


Create a python file in the root directory of the project called `models.py`

> [!info]  For each database table that you’ll be reading from or writing to, you need a *model* created in this file.
Each table will have its corresponding **class** which will have the same name. Each class has a variable for each column set to the data type of the table in the database.


Copy the following code in. This will allow the models file to access the database configuration.

![[Screen_Shot_2022-07-01_at_4.03.29_pm.png|Screen Shot 2022-07-01 at 4.03.29 pm.png]]

```python
from app import db
from datetime import datetime
```

To create the model, Flask requires a **class** to be created for it to store the database records in memory when required. For instance, when the code loads a particular entry, or before writing to the database.

The syntax for this is `class Contact(db.Model):`

![[Screen_Shot_2022-07-01_at_4.04.15_pm.png|Screen Shot 2022-07-01 at 4.04.15 pm.png]]

Each field that is in the database will need it’s corresponding entry in the model class. For instance, the first field in the database is `id`, so the first variable created is the id.

As this id variable is the primary key, and autoincrements (as defined when creating the table), this needs to be indicated in the declaration.

![[Screen_Shot_2022-07-01_at_4.04.22_pm.png|Screen Shot 2022-07-01 at 4.04.22 pm.png]]

```python
class Contact(db.Model):
	id = db.Column(db.Integer, primary_key=True, autoincrement=True)
```

The next three fields are just TEXT fields, and can be defined as such.

![[Screen_Shot_2022-07-01_at_2.00.46_pm.png|Screen Shot 2022-07-01 at 2.00.46 pm.png]]

```python
name = db.Column(db.Text)
email = db.Column(db.Text)
message = db.Column(db.Text)
```

> [!info]  If your database differs from this example, you will need to modify the class definitions accordingly.


The final field is the `dataSubmitted` field. As this is stored as a DATETIME in the database, this needs to be declared as such.

![[Screen_Shot_2022-07-01_at_2.01.22_pm.png|Screen Shot 2022-07-01 at 2.01.22 pm.png]]

```python
dateSubmitted = db.Column(db.DateTime)
```

The last part to add is a function that automatically runs when a new contact object is created. 

This can be very complicated, however whenever a class is created in python (referred to as instantiating an object), a prebuilt function called `__init__` is executed if it is there. 

After the class definition, add the function

```python
def __init__(self, name, email, message):
		self.name = name
		self.email = email
		self.message = message
		self.dateSubmitted = datetime.today()
```

This function assigns all the variables as necessary, but importantly, also sets the dateSubmitted field to be the time of the computer when it’s submitted.

The final class should look similar to this one. 

![[Screen_Shot_2022-07-01_at_4.05.15_pm.png|Screen Shot 2022-07-01 at 4.05.15 pm.png]]

> [!info]  Python is extremely strict with indentation. Make sure the fields are indented (press tab) once after the class definition.


**Save the file.**

# Create Forms.py

To capture the form information submitted by the form, a Python class needs to be defined. This is similar to the idea of a model, which represents the database. Whereas this class ‘models’ the form.

Create a `forms.py` file in the root directory of the project.

Do this by right-clicking on the project folder and choose New → Python file. Call it `forms.py`.

![[Screen_Shot_2022-07-01_at_1.10.06_pm.png|Screen Shot 2022-07-01 at 1.10.06 pm.png]]

Add the necessary imports to the top of the file.

These imports are used to create the structure of the forms in memory. Additionally the `DataRequired` and `Email` validators easily allow the code to require data to be submitted for each field, and also force the email address entered by the user to be in a valid format.

![[Screen_Shot_2022-07-01_at_1.16.55_pm.png|Screen Shot 2022-07-01 at 1.16.55 pm.png]]

```python
from flask_wtf import FlaskForm
from wtforms.validators import DataRequired, Email
from wtforms import StringField, SubmitField, IntegerField
```

Create the class, indicating that it will be used as a `FlaskForm`.

![[Screen_Shot_2022-07-01_at_1.46.54_pm.png|Screen Shot 2022-07-01 at 1.46.54 pm.png]]

```python
class ContactForm (FlaskForm):
```

Similarly to the Model file, each field that will be collected by the form will need to have a corresponding variable.

> [!info]  The variable names **must** match the variables created in the contact.html form.


Each of these entries are defined as a `StringField`, meaning that they’ll be collecting text information - which matches the requirement of the database.

The text immediately inside the brackets - “Name”, “Email”, “Message” and “Submit” - will be displayed on the form in the place of the label (for instance `{{ form.name.label }}<br>` in contact.html). Submit will be displayed as the text on the submit button.

The `validators` forces the data to be required before submitting, and in the case of the email address, also forces it to be a valid email address.

![[Screen_Shot_2022-07-01_at_1.47.51_pm.png|Screen Shot 2022-07-01 at 1.47.51 pm.png]]

```python
name = StringField("Name", validators=[DataRequired()])
email = StringField("Email", validators=[DataRequired(), Email()])
message = StringField("Message", validators=[DataRequired()])
submit = SubmitField('Submit')
```

Save the file.

# Contact Route

The final step in the process is to implement the route in `app.py`. This is the part of the project which links the other parts (model & form) together, and directs what happens when the user accesses the URL (route), and what happens when they submit the form.

Start by opening `app.py` and adding code to import the two new classes created:

- Update the model import to include Contact, and
- import ContactForm from forms.

> [!info]  These lines of code need to be AFTER the `db=SQLAlchemy(app)` line of code.


![[Screen_Shot_2022-07-01_at_3.37.38_pm.png|Screen Shot 2022-07-01 at 3.37.38 pm.png]]

```python
from models import Contact
from forms import ContactForm
```

and creating a new route.

> [!info]  The error showing in ‘form’ will be fixed at the next step.


This code creates the route entry point for `/contact.html` and associates it with the `contact()` function.

The route also is defined with `, methods=["POST", "GET"])` which indicates that the route will not only display the c`ontact.html` page, but also accept data back to the server, through the form submit.

![[Screen_Shot_2022-07-01_at_3.31.46_pm.png|Screen Shot 2022-07-01 at 3.31.46 pm.png]]

```python
@app.route("/contact.html", methods=["POST", "GET"])
def contact():
	return render_template("contact.html", title ="Contact Us", form=form)
```

The first step for this function is to load the ContactForm to be displayed to the user. Add the following code to load the class and store it in a variable called `form`.

![[Screen_Shot_2022-07-01_at_3.48.45_pm.png|Screen Shot 2022-07-01 at 3.48.45 pm.png]]

```python
form = ContactForm()
```

## Time to Test!

Run the project and load the page to see if it works.

Flask has lots of moving parts that interact with each other, and therefore this is the first chance to test to see if it’s all working. 

Ideally, it all works and you should see a page similar to this example.

[http://127.0.0.1:5000/contact.html](http://127.0.0.1:5000/contact.html)

> [!info]  This site may appear different if you’ve included Bootstrap into the template.


![[Screen_Shot_2022-07-28_at_9.27.41_pm.png|Screen Shot 2022-07-28 at 9.27.41 pm.png]]

![[Screen_Shot_2022-07-01_at_3.49.48_pm.png|Screen Shot 2022-07-01 at 3.49.48 pm.png]]

## Form Submission

The form is displayed to the user but data can not be submitted as yet. The final step is to check if the user has pressed the submit button and then what to do with the information in the input boxes.

First, check if the user has pressed the submit button. This is done through the `validate_on_submit()` function.

![[Screen_Shot_2022-07-01_at_3.54.36_pm.png|Screen Shot 2022-07-01 at 3.54.36 pm.png]]

```python
if form.validate_on_submit():
```

Then create a new_contact object which contains all of the information in the form fields - name, email and message. 

![[Screen_Shot_2022-07-01_at_3.55.40_pm.png|Screen Shot 2022-07-01 at 3.55.40 pm.png]]

```python
new_contact = Contact(name=form.name.data, email=form.email.data, message=form.message.data)
```

This is where the benefit of doing all the previous work with forms and models, because this `new_contact` variable has all the information needed to write to the database, including the table and SQL data. Ordinarily, you would have to write the SQL to add it to the database in the correct format.

The last step is writing the `new_contact` data to the database.

“commit” means it actually writes it to the database.

![[Screen_Shot_2022-07-01_at_3.57.42_pm.png|Screen Shot 2022-07-01 at 3.57.42 pm.png]]

```python
db.session.add(new_contact)
db.session.commit()
```

Save the file.

## Test the Submission Process!

Hopefully the project functions just as it does below.

![[2022-07-01_16-10-08.2022-07-01_16_13_04.gif|2022-07-01 16-10-08.2022-07-01 16_13_04.gif]]

# Custom CSS in Forms.

To add CSS classes to form elements, you can do this by updating `forms.py`. 

For instance, applying CSS classes to the Submit button, to incorporate standard Bootstrap classes, you could update the submit button defintion by adding a `render_kw` attribute:

```python
submit = SubmitField('Submit', render_kw={"class": "btn btn-primary btn-block"})
```

You can add additional details using the standard dictionary structure (`key:value`). 
