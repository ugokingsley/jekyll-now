---
layout: post
title: Getting Started With Django – Part 2
---

In this tutorial we are going to create our first django application, before that let’s create the admin superuser

Creating Administrator Account

1. apply migrations

-- python manage.py migrate


2.  create superuser (the administrator)

--- python manage.py createsuperuser


3.  login as the administrator

--start the server python manage.py runserver 5000


4. On your web browser address bar

-- localhost:5000/admin


5.  type in the superuser credentials entered earlier, you will be logged in to the django admin interface…



Creating A Django Application

let us create our first django application called daetacitystore

-- python manage.py startapp daetacitystore


we will continue with an overview of the root folder(inventory) and the django app(daetacitystore) directory


An overview of Django root folder and the django application directory

1. root folder(inventory)

 open the inventory folder with a text editor of your choice, I will be using pycharm.
    -- below is the directory structure

    inventory/

        daetacitystore/

        db.sqlite3

        manage.py

        inventory/

           __init__.py

           settings.py

           urls.py

           wsgi.py

The “inventory” subfolder: This folder is the actual python package of your project.
                It contains four files:

 __init__.py: Just for python, treat this folder as package.

 settings.py: As the name indicates, your project settings.

 urls.py: All links of your project and the function to call them

 wsgi.py: If you need to deploy your project over WSGI.





Setting Up Your Project
Your project is set up in the subfolder inventory/settings.py. Following are some important

options you might need to set:

--DEBUG = True

This option lets you set if your project is in debug mode or not. Debug mode lets you get

more information about your project's error. Never set it to ‘True’ for a live project.

However, this has to be set to ‘True’ if you want the Django light server to serve static

files. Do it only in the development mode.

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
Database is set in the ‘Database’ dictionary. The example above is for SQLite engine. Django also supports:

MySQL (django.db.backends.mysql)
PostGreSQL (django.db.backends.postgresql_psycopg2)
Oracle (django.db.backends.oracle) and NoSQL DB
MongoDB (django_mongodb_engine)


2. django app(daetacitystore) directory

      daetacitystore/

          migrations/

          __init__.py

          admin.py

          apps.py

          models.py

          tests.py

          views.py

__init__.py: Just to make sure python handles this folder as a package.
admin.py: This file helps you make the app modifiable in the admin interface.
apps.py: this file is used for app configuration
models.py: This is where all the application models are stored.
tests.py: This is where your unit tests are.
views.py: This is where your application views are.
At this stage we have our "daetacitystore" application, now we need to register it with our Django

project "daetacitystore". To do so, update INSTALLED_APPS tuple in the settings.py file of your

project (add your app name):

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'daetacitystore',
]
