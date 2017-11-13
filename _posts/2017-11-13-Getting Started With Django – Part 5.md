
---
layout: post
title: Getting Started With Django – Part 5
---


Django Forms

Creating, validating and processing forms is an all too common task. Web applications receive input and collect data from users by means of web forms. So naturally Django comes with its own library to handle these tasks. In Django 0.96, the library is called newforms but it will be renamed to forms in Django 1.0 (once it has been released). In this section of the tutorial we’ll create a form to enable us enter post without using the admin interface, before that let us create the views for creating, displaying post detail, updating and deleting forms.

Open the views.py file and paste the following create function code snippets:

	from django.core.urlresolvers import reverse
	from django.shortcuts import render, get_object_or_404, redirect
	from .models import Post
	from django.http import HttpResponseRedirect, Http404
	from django.core.urlresolvers import reverse
	from .forms import PostForm
	from django.contrib import messages

	def create(request):
    		if not request.user.is_staff or not request.user.is_superuser:
        		raise Http404
    		form=PostForm(request.POST or None)
    		context={
        		"form":form,
    		}
   		if form.is_valid():
        		instance=form.save(commit=False)
       		title=form.cleaned_data.get("title")
       		content=form.cleaned_data.get("content")
        		instance.title=title
        		instance.content=content
        		instance.save()
        		return HttpResponseRedirect(reverse('post_list'))
    		return render(request,"create.html",context)
we first of all import all the necessary modules and function, then we check if the user of the website is a staff or a superuser(administrator), if not the view should return a 404 error page, next we create a variable called form that retrieves submitted posts. The PostForm() is created in a file called forms.py  (not added by django by default) in the djangoapp directory the code snippets in forms.py is pasted below:

	








	from django import forms
	from .models import Post

	class PostForm(forms.ModelForm):
    		class Meta:
        		model=Post
        		fields=['title','content']

    		def clean_content(self):
        		content= self.cleaned_data.get('content')
        		return content

    		def clean_title(self):
        		title=self.cleaned_data.get('title')
        		return title
we first import th forms module from django, thereafter wee import the Post model from models.py. We create a class called PostForm, with meta class of model Post and fields title and content. The timestamp and updated fields are filled automatically by django; we then create a method for validating user inputs.

From the create view, we rendered the create.html template, see github repo for code snippets, it’s similar to the other templates we created earlier


To create the post_display view, open the views.py file and paste the following post_display function code snippets:

	def post_display(request, post_id):
    		display = get_object_or_404(Post, id=post_id)
    		context = {
        		'display': display,
    		}
    		return render(request, 'post_display.html', context)
we create a variable called display that contains the value of an object retrieved from the database, a dictionary dictionary variable called context is created to hold the display data, that will be rendered in post_display.html template. This template will be rendered when the title of the blog post is clicked, it displays every thing about the blog post in detail is the homepage limits some content.













The post_update view retrieves a particular post from the database and renders it in the same form as the one used to create it.  Open the views.py file and paste the following post_update function code snippets:

	def post_update(request, pk=None):
    		if not request.user.is_staff or not request.user.is_superuser:
        		raise Http404
    		instance = get_object_or_404(Post, pk=pk)
    		form = PostForm(request.POST or None, request.FILES or None, 		instance=instance)
    		if form.is_valid():
       		instance = form.save(commit=False)
       		instance.save()
       		messages.success(request, "<a href='#'>Item</a> Saved", 		extra_tags='html_safe')
       		return HttpResponseRedirect(instance.get_absolute_url())
    		context = {
      		"title": instance.title,
      		"instance": instance,
      		"form":form,
   		}
    		return render(request, "create.html", context)
we check again if the user requesting an update is a staff or an administrator, else we return a 404 page.
We request the PostForm(); check if the new user input is valid and then save, as said earlier we render the create.html.



The post_delete view is a view for deleting blog posts. Open the views.py file and paste the following post_delete function code snippets:

	def post_delete(request, pk=None):
   		if not request.user.is_staff or not request.user.is_superuser:
      		raise Http404
   		instance = get_object_or_404(Post, pk=pk)
   		instance.delete()
   		messages.success(request, "Successfully deleted")
   		return redirect("/")
      
once we get and delete the object we want to, we display the “ successfully deleted ”  message and we are redirected to the homepage. 

Note: the messages, redirect,  get_object_or_404, etc., are imported  above. Check the create view code snippets above.


Edit the urls.py file to include the create, post display, update and delete urls.
See code snippets below:


	from django.conf.urls import url
	from django.contrib import admin
	from djangoapp.views import post_list, post_display, create, post_update, post_delete

	urlpatterns = [
    		url(r'^admin/', admin.site.urls),
    		url(r'^add/$', create, name='create'),
    		url(r'^$', post_list, name='post_list'),
    		url(r'^display/(?P<post_id>[0-9]+)/$', post_display, name="post_display"),
    		url(r'^display/(?P<pk>[\w-]+)/edit/$', post_update, name='update'),
    		url(r'^display/(?P<pk>[\w-]+)/delete/$', post_delete,name='delete'),
	]
