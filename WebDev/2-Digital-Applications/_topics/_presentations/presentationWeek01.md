# Welcome to Semester 2

- Continuing Flask development.
- Developing `A New Homepage`, further exploring Flask, databases etc

note: 
This project is building from last semester, but ok for new students too.
The basic website structure from last semester will be brought across (user management, base template, navbar etc), to focus more on further development. 
Many will be smaller modules of functionality, like a shopping list, resume builder etc.

---
# Holocron

- Source of Truth!
- Subject Content will be published here.
- Regularly receive updates.
- Obsidian (preferred) or Github

---
# Slides

- This file (and other 'presentation' files) can be opened as Slides in Obsidian.

![[/WebDev/2-Digital-Applications/_topics/_images/openSlidesView.png]]

---
# Preparation

- Install [Obsidian](https://obsidian.md/)
- [Holocron](https://github.com/Lake-Tuggeranong-College/Holocron)
	- Fork Holocron (this repository)
	- Clone repository to device
- Open Holocron in Obsidian
- Accept the GitHub Assignment

---
# Development Environment

- Project can be developed, either
	- GitHub Codespaces (online)
	- Visual Studio Code & Docker (offline)
- Visual Studio Code & Docker can't be run on Chromebook.
	- Instructions on Holocron
- Install SQLite into environment
- Test Environment

note:
Choose which development environment works for you. There is no right or wrong answer.

---
# Project Overview
- Flask, Python, HTML, CSS, etc
- SQLite Database
- Personal Homepage
	- To Do List
	- Shopping List
	- Resume Builder
	- etc.
---
# Project Restructure

- Project restructured to *hide* the existing code. 
![[/WebDev/2-Digital-Applications/_topics/_images/w1-ProjectRestructure.png]]
- Most development will be in `app.py`.
- Leave `bp_admin.py` & `bp_user_management.py` files alone.
- Uses the Flask `Blueprint` feature.

note: These files contain the majority of the routes and other code for user management (login/logout etc) and any administration routes.

---
# Questions?

If you have any questions, please ask!

ryan.cather@ed.act.edu.au or discord.