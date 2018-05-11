---
layout: post
title: Testing a Django Web Application – Unit Testing
---
This tutorial shows how to write and carry out unit testing of your website using Django's test framework. As we begin to build websites and other applications, and as interactions with such websites or application components become complex and not easy to investigate,  a small change in one component of the website or application can impact other areas and components, so more changes will be required to ensure everything keeps working and errors are not introduced as more changes are made.  One way to mitigate these problems is to write automated tests, which can easily and reliably be run every time you make a change. For example, manually navigating through pages to verify that it’s not rendering errors when a view  or model in your django application is altered could be a hell of a task and very frustrating; also it’s time consuming,  If we were to continue as we are, eventually we'd be spending most of our time testing, and very little time improving our code



Automated tests can really help with this problem! The obvious benefits are that they can be run much faster than manual tests, can test to a much lower level of detail, and test exactly the same functionality every time (human testers are nowhere near as reliable!) Because they are fast, automated tests can be executed more regularly, and if a test fails, they point to exactly where code is not performing as expected.

Django provides a test framework with a small hierarchy of classes that build on the Python standard unittest library. Despite the name, this test framework is suitable for both unit and integration tests. The Django framework adds API methods and tools to help test web and Django-specific behaviour. These allow you to simulate requests, insert test data, and inspect your application's output
for this tutorial, we are going to write a simple unit test for the application  we built in the getting started with django series. We are going to create a unit test for our models and views. Create a file structure as shown below in the djangoapp directory. The __init__.py should be an empty file (this tells Python that the directory is a package). Delete the /djangoapp/tests.py file that came pre-installed.

	Djangoapp/
		/tests/
			__init__.py
			test_models.py
			test_views.py
			


for the models unit test, let’s briefly look at the underlying model we testing:

	from django.db import models
	class Post(models.Model):
    		title = models.CharField(max_length=120)
    		content = models.TextField()
    		updated = models.DateTimeField(auto_now=True, auto_now_add=False)
    		timestamp = models.DateTimeField(auto_now=False, auto_now_add=True)
    		def __str__(self):
        		return self.title

for instance we want to test to see if any post that was created has a title and a corresponding content; add the following code snippets in the test_models.py file:


	from django.test import TestCase
	from ..models import Post

		class PostTest(TestCase):
    			def setUp(self):
        			Post.objects.create(title='keep trying', content='keep trying and 	dont ever quit you are almost there')
    			def test_post(self):
        			post = Post.objects.get(id=1)
        			expected_object_name = '%s' % (post.title)
        			self.assertEquals(expected_object_name,str(post))

we first import the TestCase class from the Django (or unittest)  framework.

setUp() is called before every test function to set up any objects that may be modified by the test (every test function will get a "fresh" version of these objects). We create a dummy post object: a title and a content.

test_post() is called to verify if the dummy post we created exists in the database. We have a number of test methods, which use Assert functions to test whether conditions are true, false or equal (AssertTrue, AssertFalse, AssertEqual). If the condition does not evaluate as expected then the test will fail and report the error to your console. In our case we use the  AssertEquals methods to check if the dummy post created is what is in the database.

For the views unit test we want to see if the post listing and the single post display will be executed correctly. let’s briefly look at the underlying views we testing:
		
	def post_list(request):
    		list=Post.objects.all().order_by("-timestamp")
    		context={
        		"list": list,
    		}
    		return render(request, "post_list.html", context)

	def post_display(request, post_id):
    		display = get_object_or_404(Post, id=post_id)
    		context = {
        		'display': display,
    		}
    		return render(request, 'post_display.html', context)

add the following code snippets in the test_views.py file:

	from django.test import TestCase, Client
	from django.urls import reverse
	from ..models import Post

	# initialize the APIClient app
	client = Client()

	class GetAllPostTest(TestCase):
    		def setUp(self):
        		Post.objects.create(
            		title="keep trying", content="keep trying and don't ever quit you 				are almost there")
        		Post.objects.create(
            		title="keep hope alive", content="though the sorrow may last for the night but joy comes in the morning")

    		def test_get_all_post(self):
        		response = client.get(reverse('post_list'))
        		self.assertEqual(response.status_code, 200)

	class GetSinglePostTest(TestCase):
    		def setUp(self):
        		self.post1 = Post.objects.create(title="keep trying", content="keep trying 			and don't ever quit you are almost there")
        		self.post2 = Post.objects.create(title="keep hope alive", content="though the sorrow may last for the night but joy comes in the morning")

    		def test_get_valid_single_post(self):
        		response = client.get(reverse('post_display', kwargs={'post_id': 			self.post1.pk}))
        		self.assertEqual(response.status_code, 200)

    		def test_get_invalid_single_post(self):
        		response = client.get(reverse('post_display', kwargs={'post_id': 30}))
        			self.assertEqual(response.status_code, 404)

we first import the TestCase and Client classes from the Django (or unittest) framework. To validate our view behaviour we use the Django test Client. This class acts like a dummy web browser that we can use to simulate GET and POST requests on a URL and observe the response. We can see almost everything about the response, from low-level HTTP (result headers and status codes) through to the template we're using to render the HTML and the context data we're passing to it. We can also see the chain of redirects (if any) and check the URL and status code at each step. This allows us to verify that each view is doing what is expected.

SetUp() is called before every test function to set up any objects that may be modified by the test (every test function will get a "fresh" version of these objects). We create a dummy post object: a title and a content.

test_get_all_post() retrieves all post rendered from the post_list view and also a status code 200 if available.

test_get_valid_single_post() retrieves individual post (I.e post display) rendered from the post_display view and also a status code 200 if available.

test_get_invalid_single_post() checks to see if an individual (I.e post display) rendered from the post_display view, based on primary does not exist and also returns a status code 404  if available.


The easiest way to run all the tests is to use the command:

	python3 manage.py test

This will discover all files named with the pattern test*.py under the current directory and run all tests defined using appropriate base classes (here we have a number of test files, but only /djangoapp/tests/test_models.py and /djangoapp/tests/test_views.py currently contains any tests.) By default the tests will individually report only on test failures, followed by a test summary.
Run the tests in the root directory. You should see an output like the one below.

		Creating test database for alias 'default'...
		System check identified no issues (0 silenced).
		....
		----------------------------------------------------------------------
		Ran 4 tests in 0.394s

		OK
		Destroying test database for alias 'default'...
