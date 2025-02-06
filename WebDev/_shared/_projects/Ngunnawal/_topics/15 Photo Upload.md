---
tags:
  - "2022"
  - archived
---

The requirements for the site indicate that the users should be allowed to upload images to celebrate Ngunnawal Country. The site should enable this functionality with users self selecting images to upload once they’ve logged in, where they can see the images they’ve uploaded. There should also be a page where anyone (unregistered users, registered users and administrators) can see all the images uploaded across the site.

There are different approaches to this issue in terms of storing the images and data. Broadly, the development process can take one of two paths:

1. Store the images in the database
2. Store the images in the file system inside the project and then store the filename to the image in the database.

This tutorial will show option 2 for a number of reasons. The primary is that it reduces the size of the database. This may not be a show stopping issue for an application the size of this site, however in production, storing binary data, such as images, in a database can be problematic. 

> [https://medium.com/@vaibhav0109/should-i-use-db-to-store-file-410ee22268c7](https://medium.com/@vaibhav0109/should-i-use-db-to-store-file-410ee22268c7)
> 
> 
> ## **Why store in FileSystem ?**
> 
> - read/write to a DB is always slower than a file system.
> - your DB backups become huge, which makes restoration & replication slower.(Its a problem where daily size is in TB’s)
> - access to the files now requires going through your Application and Database layers.(It increases the application memory requirements)
> - allows usage of lightweight web-server/CDN/Amazon S3 for serving.
> - cost effective, file servers are much cheaper compared to database.
> - avoids database overhead with its CRUD operations.
> - easier in cases where files (video, audio etc) are to be shared with third party providers.
> 
> ## **Then why store files in DB ?**
> 
> - DB provides ACID compliance(Atomicity, Consistency, Isolation, Durability) for each row.
> - DB provides data integrity between the file and its metadata.
> - Database Security is available by default.
> - Backups automatically include files, no extra management of file system necessary.
> - Database indexes perform better than file system trees when more number of items are to be stored.
> - Images smaller than 64kb and the storage engine of your db supports inline Blobs (InnoDB puts only 767 bytes of a `TEXT` or `BLOB` inline, MyISAM puts inline), it improves performance further as no indirection is required ([Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference) is achieved, more about this can be read [here](https://www.geeksforgeeks.org/computer-organization-locality-of-reference-and-cache-operation/)).
> - File deletion/updation is always in sync with row operations, no extra maintenance needed.

# Users Photo Page

At the end of this process, the user photo page will look similar to this.

![[Screen_Shot_2022-09-06_at_9.10.34_pm.png|Screen Shot 2022-09-06 at 9.10.34 pm.png]]

## Project Configuration

For users to be able to upload images, there needs to be a directory created to store them.

Under Static, right-click on images and create a directory.

![[Screen_Shot_2022-09-06_at_9.22.57_pm.png|Screen Shot 2022-09-06 at 9.22.57 pm.png]]

Name this directory `userPhotos`.

This will be the directory where the photos will be stored when they’re uploaded by the users.

![[Screen_Shot_2022-09-06_at_9.23.02_pm.png|Screen Shot 2022-09-06 at 9.23.02 pm.png]]

## Database Table

Users will be able to upload anywhere between **0** and **many** images. 

The `user` table does not have the flexibility to store the details for potentially thousands of images. This would require many fields (columns) which would impose a limit, and could also be a lot of wasted space taken up with empty fields.

A arguably better solution would be to create another table which focuses on the images and simply stores the user id of the user that image belongs to.

> [!info] There is no upper limit on the number of images users can upload from a technical standpoint. As a developer you could impose a limit, but that is beyond the scope of this tutorial.


A new table will be created - `photos` - which will store the data, and link to the user id stored in the `user` table, as can be seen in the UML diagram shown here.

![[databaseuml.png|databaseuml.png]]

An example of the data that could be stored in the database is shown here.

![[Screen_Shot_2022-09-06_at_10.09.20_pm.png|Screen Shot 2022-09-06 at 10.09.20 pm.png]]

---

Right click on `main` in the database tab, and choose New → Table.

![[Screen_Shot_2022-09-06_at_9.45.08_pm.png|Screen Shot 2022-09-06 at 9.45.08 pm.png]]

Create the table columns as shown.

![[Screen_Shot_2022-09-06_at_9.52.40_pm.png|Screen Shot 2022-09-06 at 9.52.40 pm.png]]

Add a new Primary Key, setting the name to be `photoid` and set the column name to be `photoid`.

![[Screen_Shot_2022-09-06_at_10.02.44_pm.png|Screen Shot 2022-09-06 at 10.02.44 pm.png]]

Click Ok. This will create the table in the database.

![[Screen_Shot_2022-09-06_at_10.03.50_pm.png|Screen Shot 2022-09-06 at 10.03.50 pm.png]]

## Photo Upload Form

Open `forms.py` and update the import statements to allow the form to upload files.

![[Screen_Shot_2022-09-06_at_9.26.51_pm.png|Screen Shot 2022-09-06 at 9.26.51 pm.png]]

Create a new `class` called `PhotoUploadForm`, which contains the form fields for the user to select a file and set the title of the image.

![[Screen_Shot_2022-09-06_at_9.29.17_pm.png|Screen Shot 2022-09-06 at 9.29.17 pm.png]]

```python
class PhotoUploadForm(FlaskForm):
	title = StringField("Image Title", validators=[DataRequired()])
	image = FileField('Photo File Upload', validators=[FileRequired()])
	submit = SubmitField("Upload Photo")
```

## Photos Model

Open `models.py` and create a new model to match the database created.

![[Screen_Shot_2022-09-06_at_10.24.51_pm.png|Screen Shot 2022-09-06 at 10.24.51 pm.png]]

```python
class Photos(db.Model):
	photoid = db.Column(db.Integer, primary_key=True)
	title = db.Column(db.String(255))
	filename = db.Column(db.String(255))
	userid = db.Column(db.Integer)
	dateSubmitted = db.Column(db.DateTime)

```

To simply the process in the future, create a `__init__` method to assist in creating new Photo objects.

This function will make it easier to create new entries in the database when uploading images, in particular by automatically assigning the date time when the file is uploaded. This is done in the same way for the `contact` model.

![[Screen_Shot_2022-09-06_at_10.26.08_pm.png|Screen Shot 2022-09-06 at 10.26.08 pm.png]]

```python
def __init__(self, title, filename, userid):
		self.title = title
		self.filename = filename
		self.userid = userid
		self.dateSubmitted = datetime.today()
```

## Photos Page v1

Edit `template.html` by adding a link to the new page to be created in the navbar. Place it under the link to the Profile page.

![[Screen_Shot_2022-09-06_at_10.13.39_pm.png|Screen Shot 2022-09-06 at 10.13.39 pm.png]]

```html
<li class="nav-item">
	<a class="nav-link" href="/userPhotos">Photos</a>
</li>
```

Create a new HTML file in the templates folder called `userPhotos.html`. Insert the standard initial code.

This page requires two parts to it:

1. Displaying the form to upload a new image, and
2. Displaying the images uploaded by the user.

This stage of the tutorial focuses on part 1.

```python
{% extends 'template.html' %}

{# Place the content for the cellContent1 in this block #}
{% block cellContent1 %}
	Sidebar
{% endblock %}

{# Place the content for the cellContent2 in this block #}
{% block cellContent2 %}
	
{% endblock %}

{# Place the content for the cellContent3 in this block #}
{% block cellContent3 %}
	Footer
{% endblock %}
```

Displaying the form is very similar to other forms already created in this project. The code utilises the class created in `forms.py` to configure the different fields.

This code create the `title` field for the user to enter a title for the image.

It then creates a button to allow the user to select an image to upload. This is referred to as `image` in the code, similar to the title, but it is rendered differently on the webpage.

Finally, it generates the submit button.

```html
<div class="container text-left">
	<form method="post" enctype="multipart/form-data" action="">
		{{ form.hidden_tag() }}
		<div class="row">
			<p>
				{{ form.title.label }}<br>
				{{ form.title(size=64) }}<br>
				{% for error in form.title.errors %}
					<span style="color: red;">[{{ error }}]</span>
				{% endfor %}
			</p>
		</div>
		<div class="row">
			<p>
				{{ form.image.label }}<br>
				{{ form.image(size=64) }}<br>
				{% for error in form.image.errors %}
					<span style="color: red;">[{{ error }}]</span>
				{% endfor %}
			</p>
		</div>

		<p>{{ form.submit() }}</p>
	</form>
</div>
```

Save the file.

## Route v1

Now it’s time to create the code to upload photos.

Open `app.py` and update the import statements to include new libraries that will be needed, and the newly created `photo` model.

![[Screen_Shot_2022-09-06_at_10.37.54_pm.png|Screen Shot 2022-09-06 at 10.37.54 pm.png]]

Configure the project to set the uploads folder for the image, and set the allowed extensions. 

![[Screen_Shot_2022-09-06_at_10.31.29_pm.png|Screen Shot 2022-09-06 at 10.31.29 pm.png]]

```python
UPLOAD_FOLDER = './static/images/userPhotos/'
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif'}
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
```

Create a function, separate to the standard routes created so far, which will take in a filename and confirm that it has an allowed file extension, in this case it’s one of the following - png, jpg, jpeg, or gif.

This is to prevent users from uploading any file they wish to. 

![[Screen_Shot_2022-09-06_at_10.35.03_pm.png|Screen Shot 2022-09-06 at 10.35.03 pm.png]]

```python
def allowed_file(filename):
	return '.' in filename and \
		   filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS
```

Create a new route for the page. As this route will both send data to the user and allow uploading data, this needs to be set to be both GET and POST.

The page will need the form defined in `forms.py` and the current user details.

![[Screen_Shot_2022-09-06_at_10.39.10_pm.png|Screen Shot 2022-09-06 at 10.39.10 pm.png]]

```python
@app.route('/userPhotos', methods=['GET', 'POST'])
@login_required
def photos():
	form = PhotoUploadForm()
	return render_template("userPhotos.html", title="User Photos", user=current_user, form=form)
```

If you run the project now, and click on the Photos link in the navbar, you should see the form being rendered.

![[Screen_Shot_2022-09-06_at_10.40.23_pm.png|Screen Shot 2022-09-06 at 10.40.23 pm.png]]

The next step is to process the uploading of the file. There are a few steps to go through. First, the file and filename of the image needs to be collected. If the filename is an approved one, then the file will be uploaded and saved in the correct folder, and the data be written to the database.

Starting with collecting the image and filename, update the route to check if the user has pressed the submit button, and collect the appropriate data.

![[Screen_Shot_2022-09-06_at_10.47.17_pm.png|Screen Shot 2022-09-06 at 10.47.17 pm.png]]

```python
if form.validate_on_submit():
  new_image = form.image.data
  filename = secure_filename(new_image.filename)
```

After collecting the image data, the code will then need to confirm it has a correct file extension, using the `allowed_file()` function, then upload the file.

![[Screen_Shot_2022-09-06_at_10.49.22_pm.png|Screen Shot 2022-09-06 at 10.49.22 pm.png]]

```python
if new_image and allowed_file(filename):
	new_image.save(os.path.join(UPLOAD_FOLDER, filename))
```

Once the file has been uploaded, then create the new photo model, write it to the database, inform the user that it was successful, and reload the page.

![[Screen_Shot_2022-09-06_at_10.51.08_pm.png|Screen Shot 2022-09-06 at 10.51.08 pm.png]]

```python
photo = Photos(title=form.title.data, filename=filename, userid=current_user.id)
db.session.add(photo)
db.session.commit()
flash("Image Uploaded")
return redirect(url_for("photos"))
```

The final part of the route is to inform the user if the file upload was not allowed.

![[Screen_Shot_2022-09-06_at_10.52.52_pm.png|Screen Shot 2022-09-06 at 10.52.52 pm.png]]

```python
else:
	flash("The File Upload failed.")
```

## Test!

Run the site and access the page. Choose an image and set a title. Make sure the file gets uploaded correctly.

![[2022-09-06_22-54-25.2022-09-06_23_00_03.gif|2022-09-06 22-54-25.2022-09-06 23_00_03.gif]]

## Version 2 - displaying the users images

The page allows users to upload images, but they can’t see images they’ve already uploaded. This requires minor changes to the route and some changes to the html file to layout and display the images.

First, update the route by first collecting a list of all the images associated with the user that is currently logged in. Luckily, flask makes this easier than it sounds by providing some helper functions.

The code can query the Photos table and collect all records, filtered by the userid.

![[Screen_Shot_2022-09-06_at_11.03.46_pm.png|Screen Shot 2022-09-06 at 11.03.46 pm.png]]

```python
user_images = Photos.query.filter_by(userid=current_user.id).all()
```

Then pass this list of records - `user_images` - to the HTML page to display by updating the `return render_template` command at the end of the function.

That’s the only changes required in the route.

![[Screen_Shot_2022-09-06_at_11.04.58_pm.png|Screen Shot 2022-09-06 at 11.04.58 pm.png]]

Displaying the users images could be problematic, depending on how exactly you wished them to be displayed. In a simple fashion, they could be displayed using a standard CSS Grid layout with a number of columns, with rows being autogenerated.

Open `userPhotos.html` and insert the following code in whichever cellContent is appropriate. In this case, it’s been placed at the top of `cellContent2`. 

This code uses the CSS class `grid-container` and then uses a for loop to create as many `<div>`s as required  - one per image. 

> [!info] The images are resized to 20% width. This is done for simplicity sake, however there are much more elegant ways to achieve the same goals through CSS.


Save the File

![[Screen_Shot_2022-09-06_at_11.12.22_pm.png|Screen Shot 2022-09-06 at 11.12.22 pm.png]]

```python
<div class="grid-container">
	  {% for image in images %}
		  <div>{{ image.title }}<br>
			  <img src="/static/images/userPhotos/{{ image.filename }}" width="20%"></div>
	  {% endfor %}
</div>
```

Open `static/ngunnawal.css` and look for the `.grid-container` selector. If there is none, create it.

If it’s already there, you can modify it to be similar to the one shown, or use the existing version.

```css
.grid-container {
	display: grid;
	grid-template-columns: auto auto auto;
	grid-gap: 1em;
	padding: 1em;
}
```

You may also find an existing selector - `.grid-container > div`. This can be deleted if unwanted.

After editing the CSS to match your design choices, you can reload the site and view the Photos page.

![[Screen_Shot_2022-09-06_at_11.24.17_pm.png|Screen Shot 2022-09-06 at 11.24.17 pm.png]]

> [!info]  These images can be displayed however you wish. Investigate and test different options for displaying images in a gallery. Experiment!


## Version 3 - Extension

Currently the photos are displayed, however only small versions of them. To improve the User Experience, you could have these as thumbnails, and the user could click on them to load the larger image. This could require creating a new HTML page and route to display the single image, loading the image and relevant data.

# Security issue

Currently, if users upload photos, it stores the image in the `static/images/userPhotos` directory. There is an issue if users attempt to upload an image with the same name as a previous one. The system current simply ignores it and will override the existing image with the new one. Both users will then see the new image.

## How to solve the issue

To solve this issue, the file will need to be saved in the userPhotos directory with a unique filename. Once the file has been uploaded, the users don’t see the filename, only the image and the title, so it shouldn’t cause an issue.

In short, the filename variable when uploading needs to be overridden. Before this can be done, the extension needs to be extracted from the existing filename and added onto the end of the randomly generated one.

After the file extension is approved, extract the file extension and store is separately.

![[Screen_Shot_2022-09-07_at_9.47.12_am.png|Screen Shot 2022-09-07 at 9.47.12 am.png]]

```python
# Get the file extension of the file.
file_ext = filename.split(".")[1]
```

Generate a random number using the `uuid` library.

> [!info] Information on UUIDs and why they can be considered completely unique : [https://en.wikipedia.org/wiki/Universally_unique_identifier](https://en.wikipedia.org/wiki/Universally_unique_identifier)


![[Screen_Shot_2022-09-07_at_9.54.21_am.png|Screen Shot 2022-09-07 at 9.54.21 am.png]]

```python
import uuid
random_filename = str(uuid.uuid4())
```

Finally, override the filename variable, with the randomly generated uuid and the extension.

![[Screen_Shot_2022-09-07_at_9.54.03_am.png|Screen Shot 2022-09-07 at 9.54.03 am.png]]

```python
filename = random_filename + "." + file_ext
```

Upload an image and you’ll notice that the image is uploaded with a random filename but still with the same extension.

![[Screen_Shot_2022-09-07_at_9.51.47_am.png|Screen Shot 2022-09-07 at 9.51.47 am.png]]
