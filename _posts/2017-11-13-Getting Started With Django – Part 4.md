---
layout: post
title: Getting Started With Django – Part 4
---
Templates: Creating a Template for the Home Page

we'd better separate Django views from HTML code generation before continuing with our application. Fortunately for us, Django provides a component that facilitates this task; it is called the template system. The idea of this system is simple, instead of embedding HTML code in the view, you store it in a separate file called a template. 

This template may contain placeholders for dynamic sections that are generated in the view. When generating a page, the view loads the template and passes dynamic values to it. In turn, the template replaces the placeholders with these values and generates the page. To help you better understand the concept, let's apply it to our post_list view. First of all, to keep our directory structure clean, we will create a separate folder called templates in the djangoproject folder. Next, we need to inform Django of our newly-created templates folder. So, open settings.py , look for the TEMPLATE_DIRS variable, and add the absolute path of your templates folder to it. If you don't want
to hard-code the path into settings.py , you can use the following little snippet that will also work:

