---
layout: post
title: Getting Started With Django – Part 4
---

Templates: Creating a Template for the Home Page

we'd better separate Django views from HTML code generation before continuing with our application. Fortunately for us, Django provides a component that facilitates this task; it is called the template system. The idea of this system is simple, instead of embedding HTML code in the view, you store it in a separate file called a template. This template may contain placeholders for dynamic sections that are generated in the view. When generating a page, the view loads the template and passes dynamic values to it. In turn, the template replaces the placeholders with these values and generates the page. To help you better understand the concept, let's apply it to our post_list view. First of all, to keep our directory structure clean, we will create a separate folder called templates in the djangoproject folder. Next, we need to inform Django of our newly-created templates folder. So, open settings.py , look for the TEMPLATE_DIRS variable, and add the absolute path of your templates folder to it. If you don't want
to hard-code the path into settings.py , you can use the following little snippet that will also work:

	'DIRS': [os.path.join(BASE_DIR, "templates")],

we will also create a folder  called static in the djangoproject folder, to hold our website assets ( javascript, css and images) create the our_static folder inside static folder then extra three folders inside our_static folder named css, js and img to hold our css, javascript and image assets repectively. Add the snippets below in settings.py, below STATIC_URL = '/static/'  to inform django of the static directory:

	STATICFILES_DIRS=(
		os.path.join(BASE_DIR, "static", "our_static"),
	)

in the templates folder, create an html file called base.html and insert the code snippets into it.
	

The base.html file represents the stucture of the website, we do not have to repeat ourselves declaring a doctype in every html file.

We load our assets from the static folder by typing {% load staticfiles %} first on the templates, the our normal html file structure. We type {% include "navbar.html" %}, to include a navigation bar file called navbar.html, the code snnipets below:

	
We only write our codes once, following a popular cliché called DRY (Don’t Repeat Yourself).
All other contents of our website will be displayed within this code snippets
		
to demonstrate create another file called post_list.html, and past the following code snippets:

	{
to take on the website structure as defined in base.html, we type 	{% extends "base.html" %} above, first things first, on our new template. Remember from our post_list view function in views.py, we stated that we will render the view in post_list.html.


All contents to be rendered will be within the {% block content %}{% endcontent %} tags.
We display all post by using a foorloop.

Next we will create our urls to display the posts on the browser. As you may recall from the previous chapter, a file named urls.py was created when we started our project. This file contains valid URLs for our application, and maps each URL to a view that is a Python function. Open urls.py in the djangoproject directory and enter the following code snippets:

	from django.conf.urls import url
	from django.contrib import admin
	from djangoapp.views import post_list

	urlpatterns = [
	    url(r'^admin/', admin.site.urls),
	    url(r'^$', post_list, name='post_list'),
	]

firstly, we import the post_list view from the views.py and add url(r'^$', post_list, name='post_list'). The rest is added by default during installation of django. 

As you can probably tell, the file contains a table of URLs and their corresponding Python functions (or views). The table is called urlpatterns , and it initially contains example entries that are commented out. Each entry is a Python tuple that consists of a URL and its view. The URL syntax may look familiar to you, because it uses regular expressions. Django gives you a lot of flexibility by letting you specify URL patterns using this powerful string matching technique.

One last thing needs explaining before we see the view in action. The regular expression that we used will look a bit strange if you haven't used regular expressions before. It is a raw string that contains two characters, ^ and $ . r'' , which is the Python syntax for defining raw strings. If Python encounters such a raw string, then backslashes and other escape sequences are retained in the string, rather than interpreted in any way. In this syntax, backslashes are left in the string without change, and escape sequences are not interpreted. This is useful when working with regular expressions, because they often contain backslashes. In regular expressions, ^ means the beginning of the string, and $ means the end of the string. So ^$ basically means a string that doesn't contain anything; that is an empty string. Given that we are writing the view of the home page, the URL of the page is the root URL, and indeed it should be empty.

To see our post list, visit the admin interface from the browser, enter the address: 
	localhost:8000/admin
click on post and fill the form. When you are done visit the home page by entering the address:
	localhost:8000
to see the list of posts you have entered from the admin panel.

In the next section we will create an interface to create blog post instead of using the admin, edit blog post and delete blog posts.
