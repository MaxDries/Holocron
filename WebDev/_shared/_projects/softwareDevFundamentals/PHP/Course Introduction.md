---
tags: php
---

# Website Development Overview

Front-end website development refers to the client-side of web development, which deals with the design and user experience of a website. This includes technologies such as [[WebDev/_shared/HTML]], [[WebDev/_shared/CSS]], and JavaScript, which are used to build the layout, look and feel, and interactivity of a website.

Back-end website development, on the other hand, deals with the server-side of web development, which is responsible for the functionality of a website. This includes technologies such as PHP, Ruby, Python, and JavaScript, as well as databases like MySQL and MongoDB. Back-end developers are responsible for building and maintaining the code that runs on the server and powers the functionality of a website, such as data storage, processing, and retrieval.

In this course, the focus will be on the back-end functionality, with **PHP** being the main language for this. The course will also show how to interface with a database using **SQL**. Whilst building the site, you will also be learning some front-end technologies as well - HTML, CSS and Javascript.

![[overviewFrontBackEnd.png|web front and back end technologies.png]]

The code that will be written for this course will be executed in a number of different places - front end code (HTML, CSS and Javascript) are rendered and executed in the browser on the client device. The back-end code is executed on the web server, and this acts as an Intermediary between the client device and the database.

![[overviewPHPProcess.png|PHP process explained.png]]

# PHP Implementation

PHP is a **server side** scripting language. This means that PHP code runs on the server (not the client/browser) and delivers the necessary code to the client to display. 

For example, the server loads user information from the database and checks the username and password. If the password is correct, the browser is to display a message “login successful”. 

PHP co-exists with HTML and other Web Technologies. Unlike standard HTML, PHP requires a server to be running to process the PHP code.

PHP was created in 1994, so in terms of Programming Languages it is quite old, but it is very powerful and widespread.

PHP is used by itself but is also the main language in many other frameworks available, such as Wordpress and Laravel.

![[overviewPHPAndHTMLMix.png|PHP mixed in with HTML.]]

PHP mixed in with HTML.

![[overviewPHPInfoGraphic.jpeg]]

# Project Overview

Over this course of this term, you will develop a website which will allow the users to order products through a simple shopping cart system. The site will save these orders into a text file, and then allow users to load invoices for the orders.

# Practical Exercises

> [!info]  **Goal**
> In this task, you’ll be creating the accounts and installing the software you’ll be using for this course.


Create a Jetbrains Educational Account.

> [!info] Log in using Google, and use your school account!


[https://www.jetbrains.com/community/education/#students](https://www.jetbrains.com/community/education/#students)
## Install PHPStorm (if not already installed)

Using the Jetbrains toolbox, Install PHPStorm. 

You can follow the video, here, but install PHPStorm instead.

[2020 07 19 - Install Pycharm Final.mp4](https://drive.google.com/file/d/1-2Z0MS-TXCvL807bcc8l4oCGx6GzIKd6/view?usp=drivesdk)

## Install XAMPP

> XAMPP is a completely free, easy to install Apache distribution containing MariaDB, PHP, and Perl.
> 

This allows you to run PHP code on a web server (Apache) that is running on your local machine.

Download the installer. Choose the latest version for your OS. At the time of writing, the latest version was `8.1.12`.

`Note, if you’re on macOS, choose the Installer version (not the VM).`

[Download](https://www.apachefriends.org/download.html)

Install XAMPP on your machine.

On Windows, leave the installation directory as the default - C:\xampp.

![[overviewXampp.png|Screenshot 2022-11-21 at 11.44.25 am.png]]

After installation, run XAMPP, click on the Manage Servers tab and start `Apache Web Server` and `MySQL Database` are running.

![[overviewXamppServers.png|Screenshot 2022-11-21 at 12.08.11 pm.png]]

## PHPMyAdmin

After the XAMPP installation, test to ensure that it’s all working by access PHPMyAdmin on your local machine.

[http://127.0.0.1/phpmyadmin/](http://127.0.0.1/phpmyadmin/)

![[overviewPHPMyAdmin.png|Screenshot 2022-11-21 at 12.11.19 pm.png]]

## Create a Github Account

> [!info]  If you already have a github account, you can skip this step.


1. Go to [https://github.com/join](https://github.com/join).
2. Using your school email address (...@schoolsnet.act.edu.au) create a Github account.

## Duplicate Project

Open the link to view the template project on Github for this project. 

[https://github.com/Lake-Tuggeranong-College/Software-Development-Fundamentals-PHP-Template](https://github.com/Lake-Tuggeranong-College/Software-Development-Fundamentals-PHP-Template)

Click on the **Use This Template** Button.

![[overviewUseTemplate.png|overviewUseTemplate.png]]

Click **Create New Repository** to make a copy of the project into your account.

![[overviewCreateNewRepo.png|overviewCreateNewRepo.png]]

Give the project a name, such as `Software-Development-Fundamentals-PHP` and then click **Create Repository from Template**. 

This will create a new repository in your account.

![[overviewRepoName.png|overviewRepoName.png]]

Once that’s completed, open PHPStorm and choose **Get from VCS.**

![[overviewGetFromVCS.png|overviewGetFromVCS.png]]

Select **Github** in the menu on the left, find the project you just created and click **Clone**.

![[overviewChooseRepo.png|overviewChooseRepo.png]]

The project will now open. If PHPstorm asks you if you trust the project, click Trust. 

Double click on `hello.php`. Click the popup to view the rendered page in the PHPStorm internal browser.

![[overviewHelloPHP.png|Screen Shot 2022-12-12 at 12.41.13 pm.png]]

When you first open the project, you will receive a **502 Bad Gateway** error. This is due to PHPStorm not knowing where PHP is installed on your computer. Click on **Configure PHP Interpreter** in the notification.

If PHPStorm shows the other notification, click **Don’t Ask Again**.

![[overviewBadGateway.png|overviewBadGateway.png]]

To configure the PHP Interpreter, click on the `...` button.

![[overviewCLIChoose.png|overviewCLIChoose.png]]

Click the `+` to add a new interpreter.

![[overviewNewInterpreter.png|overviewNewInterpreter.png]]

In the `+` menu that appears, if the PHP is not automatically detected, you’ll need to browse to where PHP is installed (c:\XAMPP), and then under `bin/php`. This is the file you wish to choose.

Click Ok and then Ok again.

![[overviewSetInterpreter.png|overviewSetInterpreter.png]]

Using Kubuntu in 6118, when configuring the PHP Interpreter in PHPStorm, set the PHP Executable to `/opt/lampp/bin/php`. This should detect it as PHP Version 8.2.0

![[overviewInterpreterVersion.png|Untitled]]

Close the preview window and then reopen it and you’ll see `Hello World` appear. This means that the project and PHP is configured correctly.

![[overviewHelloDisplay.png|overviewHelloDisplay.png]]

## Publish Project to Github

Open the PHP project in PHP.

![[overviewPHPStormView.png|Screenshot 2022-11-21 at 1.17.28 pm.png]]

Choose VCS→ Enable Version Control Integration.

![[overviewPHPStormEnableVCS.png|Screenshot 2022-11-21 at 1.14.11 pm.png]]

Choose Git and click Ok.

![[overviewVCSSetGit.png|Screenshot 2022-11-21 at 1.14.58 pm.png]]

Open Settings (File → Settings). Under Version Control Click Githhub and then click the + button to add a new Github account. Choose Login Via Github.

Log in to Github with your browser and authorise the connection.

![[overviewGitHubLogin.png|Screenshot 2022-11-21 at 1.12.28 pm.png]]

# Review

1. What was PHP original developed for?
2. Where did the name "bug" originate?
3. What is this "Hello World" you speak of?
4. In plain language, What is the difference between interpreted and compiled code?
	1. Expand on your explanation by adding technical details.
5. Is PHP compiled or interpreted? 
6. What is the best programming language to learn? Why?
