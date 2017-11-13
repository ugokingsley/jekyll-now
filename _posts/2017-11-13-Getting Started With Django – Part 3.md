---
layout: post
title: Getting Started With Django – Part 3
---


Models: Designing an Initial Database Schema.

We are to create a database table for posts. We will chose the database engine that configured our database settings in the previous part. If you are used to dealing with the database directly through SQL queries, then you may find Django's approach to database access a bit different. Loosely speaking,
Django abstracts access to database tables through Python classes. To store, manipulate and retrieve objects from the database, the developer uses a Python-based API. In order to do this, SQL knowledge is useful but not required. This technique is best explained by example, so let’s get started.

Open models.py file in the djangoapp directory and paste the following code snippets.


	from django.db import models


	class Post(models.Model):
    		title = models.CharField(max_length=120)
    		content = models.TextField()
    		updated = models.DateTimeField(auto_now=True, auto_now_add=False)
    		timestamp = models.DateTimeField(auto_now=False, auto_now_add=True)
    		def __str__(self):
        		return self.title

The models package contains classes that are required to define models, so it is imported first.

• We define a class named Post , we have four table columns or fields: title, and it's of the type models.CharField for the title of the blog post; content, the blog post, updated, a real time timing of blog post when edited and timestamp, a real timing of blog post when posted initially. models.CharField  is one of many field types provided by Django.  Django. Below is a partial table of these types:

Field Type
Description
IntegerField
An integer.
TextField
A large text field.
DateTimeField
A date and time field.
EmailField
An email field with 75 chars max.
URLField
A URL field with 200 chars max.
FileField
A file-upload field.

For more information on model fields check the django documentation.

Now, issue the following command to create a table for the Link data model in
the database:

	python manage.py migrate

You may remember that we used this command in the previous chapter to create Django's own administrative tables. Whenever you add a data model, you need to issue this command in order to create its table in the database.

We to make this table accessible from the django admin interface. Add the code snippets below


	from django.contrib import admin
	from .models import *

	admin.site.register(Post)
The models package contains classes that are required to add models to admin interface, so it is imported first, then we import all models created in models.py; thereafter, we register the Post class, register as many classes as possible if you want to access them from django administrative interface.


Views: Creating a View to list our Blog posts
we are going to create our first view to list our blog post entry on the home page. Open the views.py file in the djangoapp directory and paste the following code snippets:

	from .models import Post
	from django.shortcuts import render

	def post_list(request):
    		list=Post.objects.all().order_by("-timestamp")
    		context={
        		"list": list,
    		}
    		return render(request, "post_list.html", context)
we use a function based view for this class, we have  class based views. We import the Post model from models.py and also the render function to render this view to the template. We create a function with a request argument, a variable called list that retrieves all post from the post model in descending order, ordering by the timestamp. We create a dictionary variable called context to hold the post data retrieved from the database and we return a render function with request, the template file and context as its argument.

We will continue in part 4 with the creation of a template to render our blog posts

