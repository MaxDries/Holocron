---
tags:
  - "2022"
  - archived
---

## Gallery.html

At this stage, the users can upload and see photos that they have uploaded. These instructions show how to develop a photo gallery page of all images uploaded, but all users. As the page will simply be loading existing images, there will be no changes to the database or `forms.py` or `models.py`.

The page will be *similar* to the userPhotos.html file as this page already displays some of the images (filtered by the user) where as this new page will be displaying all users.

With that in mind, duplicate userPhotos.html and name the file `gallery.html`.

![[Screen_Shot_2022-09-07_at_9.24.33_am.png|Screen Shot 2022-09-07 at 9.24.33 am.png]]

Leave the `grid-container` div block and remove the div block with the form. 

![[Screen_Shot_2022-09-07_at_9.27.01_am.png|Screen Shot 2022-09-07 at 9.27.01 am.png]]

After this process, your `gallery.html` should appear similar to this.

![[Screen_Shot_2022-09-07_at_9.29.08_am.png|Screen Shot 2022-09-07 at 9.29.08 am.png]]

Save the file.

## Route

Create a new route for the gallery page. This route will be very simple compared to other routes, as all it will need to do is read the entire table of images and send that to the gallery page to display.

```python
@app.route('/gallery.html')
def photo_gallery():
	all_images = Photos.query.all()
	return render_template("gallery.html", title="Photo Gallery", user=current_user, images=all_images)
```

### Version 3 - Extension
