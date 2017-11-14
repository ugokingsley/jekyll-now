
---
layout: post
title: Host Your First Website in Pythonanywhere
---

Host Your First Website in Pythonanywhere.

PythonAnywhere makes it easy to create and run Python programs in the cloud. You can write your programs in a web-based editor or just run a console session from any modern web browser. There’s storage space on the servers, and you can preserve your session state and access it from anywhere
To get started visit pythonanywhere.com to create an account.

1. Click on pricing and signup and then create a beginner account.
2. Fill the form and click on register to register.
3. Confirm the account in your email address.

Once logged in,

The first need you need to to is to create a new bash console, move to your dashboard and, in the consoles tab, click on bash. You will need to create a new virtual environment, paste the following in your bash console

	mkvirtualenv django17 –python=/usr/bin/python2.7

note: you change django17 to any name you like. But I will keep mine as django17, also you can change the version of python to any say 3.4 and so on .
Run the command by pressing enter. Once done, the virtual environment will be active, this can be seen by the (django17) in the far left corner of the console, if the virtual environment is not active type

	workon django17

in the bash console and type enter.
Still in the bash console, install django by typing

pip install django==1.10

choose your preferred version by replacing 1.10 with say 1.8, to install django version 1.8 , and so on.

To check whether django is installed paste the following in bash console

python -c "import django; print(django.get_version())"

the version of django installed will then be printed.
Create a new django project in your bash console by typing,

django-admin.py startproject devcorp

note: djangoproject is the name of my project, give yours anything you desire.
Open your dashboard in another tab of your web browser, click on the files tab on your dashboard to see your project, “devcorp ” files, if that’s what you named yours.
Follow this path djangoproject > djangoproject> to your settings.py file, and open it. Pythonanywhere comes with a built-in editor to work with; whenever you make a change in any file , click on the save button above, at the right corner to save your file.

note : your python manage.py runserver will not work on your console.

Instead of running manage.py, click on the web tab in your dashboard and the click on add a new web app, click next, and then manual configurations from the list. Select the python version you used when you install your virtual environment, for my case I choose python2.7. hit next again, then the web app configuration page shows up.
Scroll down to the virtualenv section, enter the path to your virtualenv:

/home/your_username/.virtualenvs/django17

note: django17 is the name of my virtual environment.
Click the button to submit.
In the code section, still in the configuration page: click the link in the WSGI configuration file:
delete the codes there and paste the following codes

# +++++++++++ DJANGO +++++++++++
# To use your own django app use code like this:
import os
import sys
#
## assuming your django settings file is at ‘/home/username/mysite/mysite/settings.py’
## and your manage.py is is at ‘/home/username/mysite/manage.py’
path = ‘/home/username/devcorp’
if path not in sys.path:
sys.path.append(path)
#
os.environ[‘DJANGO_SETTINGS_MODULE’] = ‘djangoproject .settings’
#
## then, for django >=1.5:
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()


note: change username to your username and devcorp to the name of your project, don’t forget to chick on the save button.
Reload your web page by going back to the web tab of your dashboard and then clicking reload djangoproject.pythonanywhere.com, where devcorp is the name of my django project.
Go to the link: djangoproject.pythonanywhere.com and see your new django website.

Note: whenever you make any change in your python project, make sure your reload the web application go to your bash console to sync your database:
enter cd devcorp, to move the root directory, and then, python manage.py migrate. For more information on how to create an administrator and other django fundamentals, see my other tutorials, “getting started with django”.
You need to set up your static file to make your css come through.
move to the files tab from your dashboard, and create directories in the following order:
static
 		static
			 -css
			 -img
			 -javascript
		static-only
			 -css
			-javascript
		media

copy the following codes below in your settings.py file below STATIC_URL= ‘/media/’
	if DEBUG:
	MEDIA_URL= ‘/media/’
	STATIC_ROOT=os.path.join(os.path.dirname(BASE_DIR), “static”, 	“static-only”)
	MEDIA_ROOT=os.path.join(os.path.dirname(BASE_DIR), “static”, 	“media”)
	STATICFILES_DIRS=(
	os.path.join(os.path.dirname(BASE_DIR), “static”, “static”)
	)

Go to your bash console and run python manage.py collectstatic
go to your web configuration file and in the static files section enter the static 			URL: /static/
set the directory path to :
	/home/username/static/static-only/

reload the web app, move to the admin page and then login ….
voila
