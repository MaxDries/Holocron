---
tags:
  - "2022"
  - archived
---

## Create Project

Open Pycharm, and you’re greeted with the Project Browser. Click New Project in the top of the window.

![[configCreateProject.png|Screen Shot 2022-06-15 at 10.45.46 am.png]]

Choose the Flask option on the lefthand side, and then set the location of the project to be stored on the local drive.

Click Ok.

![[Screen_Shot_2022-06-15_at_10.46.41_am.png|Screen Shot 2022-06-15 at 10.46.41 am.png]]

The configured project opens and you’re presented with a barebones Flask project.

![[Screen_Shot_2022-06-15_at_10.50.21_am.png|Screen Shot 2022-06-15 at 10.50.21 am.png]]

You can test the project by clicking the Run button in the top toolbar, and then clicking the link in the Console at the bottom.

![[Screen_Shot_2022-06-15_at_10.51.10_am.png|Screen Shot 2022-06-15 at 10.51.10 am.png]]

![[Screen_Shot_2022-06-15_at_10.53.03_am.png|Screen Shot 2022-06-15 at 10.53.03 am.png]]

## Configure Python Environment (Requirements.txt)

Right click on your project name, choose New→ File.

![[Screen_Shot_2022-06-15_at_10.20.10_am.png|Screen Shot 2022-06-15 at 10.20.10 am.png]]

Enter `Requirements.txt` as the file name. The spelling is important - if you’re not sure, copy and paste the name.

![[Screen_Shot_2022-06-15_at_10.20.19_am.png|Screen Shot 2022-06-15 at 10.20.19 am.png]]

If Pycharm asks you to add the file to Git, then click Ok.

![[Screen_Shot_2022-06-15_at_10.20.25_am.png|Screen Shot 2022-06-15 at 10.20.25 am.png]]

Enter the following text into the file and Save the file.

```
alembic>=1.0.10
altgraph>=0.16.1
click>=7.0
ecdsa>=0.13.2
esptool>=2.6
Flask>=1.0.3
Flask-Login>=0.4.1
Flask-Migrate>=2.5.2
Flask-SQLAlchemy>=2.4.0
Flask-WTF>=0.14.2
future>=0.17.1
itsdangerous>=1.1.0
Jinja2>=2.10.1
macholib>=1.11
Mako>=1.0.10
MarkupSafe>=1.1.1
pefile>=2019.4.18
pur>=5.2.2
pyaes>=1.6.1
PyInstaller>=3.4
pyserial>=3.4
python-dateutil>=2.8.0
python-editor>=1.0.4
six>=1.12.0
SQLAlchemy>=1.3.4
Werkzeug>=0.15.4
WTForms>=2.2.1
email-validator>=1.2.1
```

After a moment, a yellow banner should appear saying that “Package Requirements” aren’t set. 

Click Install Requirements on this popup.

This may take a while to install.

![[mr-bean-waiting.gif|mr-bean-waiting.gif]]

![[Screen_Shot_2022-06-15_at_10.23.17_am.png|Screen Shot 2022-06-15 at 10.23.17 am.png]]

![[Screen_Shot_2022-06-15_at_10.34.28_am.png|Screen Shot 2022-06-15 at 10.34.28 am.png]]

Eventually, it will show a popup indicating that all the packages were installed successfully.

![[Screen_Shot_2022-06-15_at_10.35.55_am.png|Screen Shot 2022-06-15 at 10.35.55 am.png]]


> [!info] At this stage, the project is only stored locally. As you would be aware this is not ideal, so the next step involves publishing the project to Github.



## Share the project to Github

In Pycharm, choose “Enable Version Control Integration” under the VCS menu

![[Screen_Shot_2022-06-15_at_10.57.28_am.png|Screen Shot 2022-06-15 at 10.57.28 am.png]]

Leave the system as Git and click Ok.

![[Screen_Shot_2022-06-15_at_10.57.32_am.png|Screen Shot 2022-06-15 at 10.57.32 am.png]]

After a moment, Pycharm will create a popup along the bottom indicating that the repository has been created. 


> [!info] This repository is still only saved on the local computer!


![[Screen_Shot_2022-06-15_at_10.58.04_am.png|Screen Shot 2022-06-15 at 10.58.04 am.png]]

Add the `app.py` and requirements.txt files to the repository. Right click on the files and choose Git→ Add.

Do the same for the static and templates folders.


> [!info] DO NOT add the `venv` folder to Git.


![[Screen_Shot_2022-06-15_at_10.58.51_am.png|Screen Shot 2022-06-15 at 10.58.51 am.png]]

![[Screen_Shot_2022-06-15_at_11.01.57_am.png|Screen Shot 2022-06-15 at 11.01.57 am.png]]

Commit the changes with “init” as the message.

![[Screen_Shot_2022-06-15_at_10.59.06_am.png|Screen Shot 2022-06-15 at 10.59.06 am.png]]

Now it’s time to publish to Github.

Under the Git (previously VCS) menu, choose Git→Share Project on Github.

![[Screen_Shot_2022-06-15_at_11.04.57_am.png|Screen Shot 2022-06-15 at 11.04.57 am.png]]

Accept the default there and click Share.

After a moment Pycharm will inform you that the project has been uploaded to Github.

![[Screen_Shot_2022-06-15_at_11.04.42_am.png|Screen Shot 2022-06-15 at 11.04.42 am.png]]

## Import Previous Project Files

In your file Browser, find the project folder from your previous Ngunnawal Website Project. This will include the HTML, CSS, image and any other files.

Drag those files into the **static folder** in your project.  


> [!info] Hold down Alt to copy the files, not move them.


![[2022-06-15_11-14-45.2022-06-15_11_23_50.gif|2022-06-15 11-14-45.2022-06-15 11_23_50.gif]]

$\utilde {\color{black} \fcolorbox{darkorange}{darkorange}  {Commit and Push to Github}}$

# Web Servers Overview

[https://youtu.be/6ZDQiZHSYCE](https://youtu.be/6ZDQiZHSYCE)

# Configuration File

To access a database, you need to configure the project to have the correct information about the database - location, username and password (if required etc).

Create a new Python file in the root of the project called `config.py`.

![[2022-06-24_13-32-09.2022-06-24_13_32_54.gif|2022-06-24 13-32-09.2022-06-24 13_32_54.gif]]

Copy this code into the file and save.

```python
import os
basedir = os.path.abspath(os.path.dirname(__file__))

class Config(object):
SECRET_KEY = os.environ.get('SECRET_KEY') or 'you-will-never-guess'
# Database configuration
SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or 'sqlite:///' + os.path.join(basedir, 'ngunnawal.db')
SQLALCHEMY_TRACK_MODIFICATIONS = False
```

Open `app.py` and add the following line of code at the top, with the import statements.

```python
from config import Config
from flask_sqlalchemy import SQLAlchemy
```

![[Screen_Shot_2022-06-24_at_1.35.05_pm.png|Screen Shot 2022-06-24 at 1.35.05 pm.png]]

Further down `app.py`, after `app = Flask(__name__)`  configure the app to use the database that you configured.

```python
app.config.from_object(Config)  # loads the configuration for the database
db = SQLAlchemy(app)            # creates the db object using the configuration
```

# Create the Database

Click on the database tab on the right-hand side of Pycharm window.

![[Screen_Shot_2022-06-21_at_10.53.36_pm.png|Screen Shot 2022-06-21 at 10.53.36 pm.png]]

Click the + icon to create a new database.

![[Screen_Shot_2022-06-21_at_10.55.55_pm.png|Screen Shot 2022-06-21 at 10.55.55 pm.png]]

In the dropdown, choose **Data Source** → **SQLite.**

![[Screen_Shot_2022-06-21_at_10.56.31_pm.png|Screen Shot 2022-06-21 at 10.56.31 pm.png]]

- Why SQLite?

There are many Database environments available (more than shown in the list) and they all ultimately serve the same function - provide access to data. 

In this project, SQLite is chosen because unlike other Database systems, **this does not require a separate server to run**. SQLite is stored in a single file that can be stored in the project folder and accessed through Pycharm. 

The other database systems available require the installation and management of a database server. Possible, but requires more overhead and management.

SQLite is not recommended in a production environment due to scaling issues (it can’t handle as many simultaneous users as other systems), but is fine for our needs.


Click the + icon to create a new database file.

![[Screen_Shot_2022-06-21_at_11.21.10_pm.png|Screen Shot 2022-06-21 at 11.21.10 pm.png]]

Name the file `ngunnawal.db` and click Save.

![[Screen_Shot_2022-06-21_at_11.22.38_pm.png|Screen Shot 2022-06-21 at 11.22.38 pm.png]]

You **may** need to install some SQLite drivers. If, on your screen, it tells you to install or update drivers in the indicated spot, do so.

![[Screen_Shot_2022-06-21_at_11.24.49_pm.png|Screen Shot 2022-06-21 at 11.24.49 pm.png]]

Click Test Connection.

![[Screen_Shot_2022-06-21_at_11.26.30_pm.png|Screen Shot 2022-06-21 at 11.26.30 pm.png]]

Click Ok.
