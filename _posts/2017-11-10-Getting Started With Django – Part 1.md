---
layout: post
title: Getting Started With Django – Part 1
---
Django is a free open-source web framework, written in Python, It is maintained by the Django Software Foundation (DSF), an independent organization established as a non-profit.
Django’s primary goal is to ease the creation of complex, database-driven websites. Django emphasizes reusabibilty and “pluggability” of components, rapid development, and the principle of DRY (Don’t repeat yourselt). Python is used throughout, even for settings files and data models. Django also provides an optional administrative create, read, update and delete interface that is generated dynamically through introspection and configured via admin models.



“Django started as an internal project at the Lawrence Journal-World newspaper in 2003. The web development team there often had to implement new features or even entire applications within hours. Therefore, Django was created to meet the fast deadlines of journalism websites, whilst at the same time keeping the development process clean and maintainable.”

This tutorial gives a complete understanding of Django
This tutorial is designed for developers and beginners who want to learn how to develop quality web applications using the techniques and tools offered by Django

prerequisites

before we proceed, the following tools should be available:

1. A text editor or IDE; for instance: Pycharm, notepad++, sublime text etc. but for this tutorial we will make use of pycharm.
2. Terminal or Command Prompt for Windows OS
3. Internet Access.

Major Goal

To fully understand the concepts used in this tutorial, we are going to build a blogging application where the administrator creates articles for users of the website to read, the admin can also edit and delete blog posts. The fundamentals of CRUD (Create Read Update and Delete) in Django is explained. 

To follow through, we recommend you go to this repo: https://github.com/ugokingsley/djangogirlsPH  to clone or download the code snippets. Why the waste of time, let’s get started.



Django – Environment Set Up

Django development environment consists of installing and setting up Python a virtual environment and Django. You can move along with us by going to your terminal or command prompt and choose a desired location to save your files.

to set up a virtual environment we need to install pip (python 3.6 installation came along with pip)

		sudo easy_install pip

    
Note: currently running on linux-ubuntu OS. But the django installation and everything in this section is same across all OS

3. Install a virtual environment

		sudo pip install virtualenv

4. Create a new virtual environment :

		virtualenv env 
or
		virtualenv -p python3 env for python 3


note: env is the name of the virtual environment; you can give any name of your choice

5.  Go to the env directory

		cd env

to activate the virtual environment.

		on OS X: source bin/activate

		on windows: .\Scripts\activate


to be sure your virtual environment is activated make sure the directory name is in bracket, at the beginning of the last line


note: make sure your virtual environment is active as you work

6.  installing django, move outside your env folder, still in your virtual environment, type

		pip install django to download the latest version of django or

		pip install django to download a specific version, in this case 1.ll



7. To finalize this section of the tutorial, let’s create a django project called djangoproject

		django-admin.py startproject djangoproject



8. to see our project live on the server, type

		python manage.py runserver

in some cases you may have port conflicts since the server runs in port 8000. you can change the port by typing
		python manage.py runserver 5000

in our case we used port 5000


9. in your web browser enter in the address bar

  		localhost:5000


10 . press ctrl + c to exit the server


we’ve reached a milestone, we will continue in part 2...
