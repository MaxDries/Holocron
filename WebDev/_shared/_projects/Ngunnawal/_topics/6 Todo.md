---
tags:
  - "2022"
  - archived
---



# CRUD

![[https://www.atatus.com/glossary/crud/](WebDev/_shared/_projects/Ngunnawal/_images/CRUD.jpeg)

[https://www.atatus.com/glossary/crud/](https://www.atatus.com/glossary/crud/)

**Create**, **Read**, **Update** & **Delete** (CRUD) are the standard operations for use with databases, especially with SQL statements using a relational data system such as SQLite. 

You can read more on CRUD by accessing [this page](https://www.humio.com/glossary/crud/).

In this exercise, you’ll demonstrate each of these processes with a simple ToDo application.

# Create the Database Table

In the database tab, right click on the tables and choose New → Table in the popup menu.

![[Screen_Shot_2022-06-29_at_9.31.32_pm.png|Screen Shot 2022-06-29 at 9.31.32 pm.png]]

Name the table `todo`.

![[Screen_Shot_2022-06-29_at_9.38.27_pm.png|Screen Shot 2022-06-29 at 9.38.27 pm.png]]

Create the first field or column in the table, called `id` by clicking on the `+` button. 

Make sure you tick all the boxes as this will be the primary key and auto incremented. This means that the database itself will manage the numbering of the row and doesn’t need to be manually programmed.

![[Screen_Shot_2022-06-29_at_9.38.34_pm.png|Screen Shot 2022-06-29 at 9.38.34 pm.png]]

Create three more field (or columns). 

The first is called `text`. The type of this field should be `TEXT`.

The second is called `done`. The type of this field should be `BOOLEAN`.

When creating the fields, do not tick any of the boxes.

![[Screen_Shot_2022-06-29_at_10.07.40_pm.png|Screen Shot 2022-06-29 at 10.07.40 pm.png]]

Click Execute. Once that process is complete, expand the database tables, and todo. The fields created should appear.

![[Screen_Shot_2022-06-29_at_10.07.47_pm.png|Screen Shot 2022-06-29 at 10.07.47 pm.png]]

# Create Model

Open `models.py`

At the end of the file, and after any other classes, add the following `todo` class.

> [!info]  Like with any other classes in this case, the names of the class and the variables need to match what the the names are in the database. If you’ve named them different, you will need to make them match.


![[Screen_Shot_2022-06-29_at_10.09.05_pm.png|Screen Shot 2022-06-29 at 10.09.05 pm.png]]

```python
class todo (db.Model):
	id = db.Column(db.Integer, primary_key=True, autoincrement=True)
	text = db.Column(db.Text)
	done = db.Column(db.Boolean)
```

**Save** the file.

# Todo Template

Right click on the Templates folder and create a new HTML file called `todo.html`. Replace the contents with this standard code.

```python
{% extends 'programmingTemplate.html' %}

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

Replace the sidebar with the code to create a new entry in the todo list.

![[Screen_Shot_2022-06-29_at_10.15.54_pm.png|Screen Shot 2022-06-29 at 10.15.54 pm.png]]

```python
<form method="POST" action="/todo">
	<input type="text" name="text" placeholder="Add a new TODO item">
	<button type="submit" value="update">Create new Todo Entry</button>
</form>
```

Replace the cellContent2’s content with the following.

This code **loops** for each entry in the Todo list (how many rows are in the table). For each entry, it creates a checkbox to indicate in the task is done or not, and the text of the todo. 

It also adds an Update button for each entry.

An example of what it will create is:

![[Screen_Shot_2022-06-29_at_10.23.19_pm.png|Screen Shot 2022-06-29 at 10.23.19 pm.png]]

![[Screen_Shot_2022-06-29_at_10.20.41_pm.png|Screen Shot 2022-06-29 at 10.20.41 pm.png]]

```python
<h2>List</h2>
{% for todo in todos %}
	<p>
	<form method="POST" action="/todoedit/{{ todo.id }}">
		<input type="hidden" name="done" value="off">
		<input type="checkbox" onclick="redirect('/todoedit/{{ todo.id }}')" data-target="/todoedit/{{ todo.id }}" name="done">

		<input type="text" title="{{ todo.dateAdded }}" value="{{ todo.text }}" name="text" class="form-control">

		<button type="submit" value="update">Update</button>
	</form>
<form method="GET" action="/todoedit/{{ todo.id }}">
			<button type="submit" value="delete">Delete</button>
		</form>
{% endfor %}
```

**Save** the file.

# ‘Create Todo’ Route

Open `app.py` .

Update the Flask import line of code to include the `request` library.

![[Screen_Shot_2022-06-29_at_10.01.57_pm.png|Screen Shot 2022-06-29 at 10.01.57 pm.png]]

Update the following import. This allows this file to load and use the new class just created.

![[Screen_Shot_2022-06-29_at_9.51.34_pm.png|Screen Shot 2022-06-29 at 9.51.34 pm.png]]

## View Todos

Create a new route by copying this code into `app.py.`

```python
@app.route('/todo', methods=["POST", "GET"])
def view_todo():
	all_todo = db.session.query(todo).all()
	if request.method == "POST":
		new_todo = todo(text=request.form['text'])
		new_todo.done = False
		db.session.add(new_todo)
		db.session.commit()
		db.session.refresh(new_todo)
				return redirect("/todo")
	return render_template("todo.html", todos=all_todo)
```

### An Explanation for each line

| Line | Explanation |
| --- | --- |
| 41 | Creates the new route, called `/todo` and also adds the extra functionality of POST and GET methods. This means that this route will send data from the server to the browser (GET) and also accept data being sent back from the browser to the server (POST). POST is the common method to submit form data back to the server.
You can read more [here](https://pythonbasics.org/flask-http-methods/). |
| 42 | Defines the function name |
| 43 | Queries the database and retrieves the whole `todo` table. The results are stored into the `all_todo` variable. |
| 44 | Checks if the Create todo form  (in `cellContent1` or sidebar) is attempting to submit data back to the server (POST). |
| 45 | Creates a new variable - `new_todo` by collecting the text entered in the form. |
| 46 | Sets the done field to `False` as default. |
| 47-49 | Writes the new entry into the database temporarily, and then commits ****it to the database permanently. The refresh command updates the entry in memory to match the database. |
| 50 | After submitting the new ToDo entry to the database, this reloads the current page. |
| 51 | “Sends” the template to the browser with all the notes loaded from the previous query on line 34. |

![[Screen_Shot_2022-08-08_at_8.51.47_pm.png|Screen Shot 2022-08-08 at 8.51.47 pm.png]]

## Run and test the page.

Run the project and access the link:

[http://127.0.0.1:5000/todo](http://127.0.0.1:5000/todo)

You should see something like this:

![[Screen_Shot_2022-06-29_at_10.42.07_pm.png|Screen Shot 2022-06-29 at 10.42.07 pm.png]]

Create a new Entry and reload the page.

![[Screen_Shot_2022-06-29_at_10.42.56_pm.png|Screen Shot 2022-06-29 at 10.42.56 pm.png]]

# ‘Edit Todo’ Route

This route will handle both the update and delete functions.

Update the Flask import statement to also import the `redirect` library.

![[Screen_Shot_2022-06-29_at_11.18.53_pm.png|Screen Shot 2022-06-29 at 11.18.53 pm.png]]

Add the following code to `app.py`

![[Screen_Shot_2022-06-29_at_11.36.07_pm.png|Screen Shot 2022-06-29 at 11.36.07 pm.png]]

```python
@app.route("/todoedit/<todo_id>", methods=["POST", "GET"])
def edit_note(todo_id):
	if request.method == "POST":
		db.session.query(todo).filter_by(id=todo_id).update({
			"text": request.form['text'],
			"done": True if request.form['done'] == "on" else False
		})
		db.session.commit()
	elif request.method == "GET":
		db.session.query(todo).filter_by(id=todo_id).delete()
		db.session.commit()
	return redirect("/todo", code=302)
```

## Code explanation

| Line | Explanation |
| --- | --- |
| 44-45 | Creates the route and python function. This is different from other routes as it accepts a variable in the route. `<todo_id>` is anything that is in the url after the `/todoedit/` in the browser. For instance if the url is [http://127.0.0.1:5000/tododit/5](http://127.0.0.1:5000/tododit/5) then `todo_id` is 5. In this case, it will refer to the the entry in the table with 5 as the ID. |
| 46 | Checks if the form has been POSTed to the server. This will be True when the user submits the form in `todo.html` created by the line : `<form method="POST" action="/todoedit/{{ [todo.id](http://todo.id/) }}">` |
| 47-50 | This first queries the database to find the entry with the ID matching `todo_id` and updates it with the text in text field and the done field whether it is ticked or not. |
| 51 | Commits any changes to the database. |
| 52 | Checks if the form has been submitted using the GET method. This will be True when the user submits the form in `todo.html` created with the line: `<form method="GET" action="/todoedit/{{ [todo.id](http://todo.id/) }}">` |
| 53 | Finds the entry in the database with the ID matching `todo_id` and deletes it. |
| 54 | Commits the change to the database. |
| 55 | redirects the browser to `/todo` after any changes have been made. |

---

# Extension

This completes the core TODO task. There are many changes that could be made, for instance making the form ‘prettier’ by using an established CSS library such as [Bootstrap](https://getbootstrap.com).
