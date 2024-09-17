# Building RESTful APIs using Django Rest Framework

### Prerequisites

Before learning Django Rest Framework, make sure that you have a working knowledge of the following:

 - [Python](https://www.python.org/): High level general purpose programming language.
 - [Django](https://www.djangoproject.com/): Web framework for Python.

### What Is Django Rest Framework?
Django Rest Framework is a popular library for building RESTful APIs with Django. It provides powerful tools for building APIs, including serialization, authentication, and permissions. You can easily create APIs that support multiple formats like JSON and XML that can be consumed by a variety of client applications. To demonstrate the power of Django Rest Framework, we will create a simple API.

### Why use Django Rest Framework?
Although you can create a fullstack using the default Django framework, it creates a highly coupled application. If you want to switch out the Django frontend for another frontend like React or Vue or even porting your application to a mobile view, it will be very difficult and time consuming. However, with Django Rest Framework, Django will only be responsible for the backend logic which will allow simple changing of frontends and reduces coupling.

### Set-Up

 We first Install virtualenv by running ```python3 -m pip install virtualenv```. This will ensure that we have the necessary package installed to create and manage virtual environments.

 We now set up the virtual environment and activate it using ```python3 -m virtualenv –p `which python3.11` venv ``` and ```source venv/bin/activate``` respectively in the project directory. Note that ```-p `which python3.11` ``` is placed to avoid common issues with python file directory locations. Also, you can replace `python3.11` with the latest available version of python.

 We now install Django and Django Rest Framework using the command ```pip install django djangorestframework```.

 Once installed, we can start a new Django project by running: ```django-admin startproject <project-name>```. This generates the default Django folder structure including several files.

 we may now create a new Django app by running: ```python manage.py startapp <app-name>```.
 
 The project structure should be similar to the following:
 
 ```
 project-name/
│
├── app-name/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── serializers.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
│
├── project-name/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
└── manage.py
 ```
 
 In order to add Django Rest Framework to your project, open the settings.py file and add 'rest_framework' to the INSTALLED_APPS list: 
 ```
INSTALLED_APPS = [
    ..., 
    'rest_framework',
]
 ```
 
 While in the settings.py file, you should also add:
```
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.AllowAny'
    ]
}
```
This allows all your API endpoints to be used by authenticated and unauthenticated requests. However, this should only be used when testing and proper authentication and permissions should be added once the application is ready for production. "REST_FRAMEWORK" is also where you will place any other default configurations such as default authentication, renderers, pagination, etc. For the other Django Rest Framework configuration options, visit the official [Django Rest Framework website](https://www.django-rest-framework.org/api-guide/settings/).


### Serializers
Since Django Rest Framework sends and receives data in JSON format, we need a way to convert Django Model data into a form that can be stored and transported in JSON format. This is exactly what serializers do. Serializers convert model data into JSON format and data received in JSON format back into model instances. Additionally, serializers also do data validation. To use serializers it is best practice to create a new file in your app called "serializers.py" and write all your serializers in that file. 

This is what a serializer looks like:
```
from rest_framework import serializers
from .models import ExampleModel

class ExampleSerializer(serializers.ModelSerializer):
    class Meta:
        model = ExampleModel
        fields = ['field1', 'field2']
```
To create the serializer, declare a new class called "Meta" inside the serializer. Then specify which model the serializer is for. Finally, specify the exact fields you want to serialize. It can be tedious to type out every field for every serializer you create so you can use ```fields = '__all__'```. If you want to serialize all fields except some specific ones, you can use ```exclude = ['field1', ...]``` instead of the fields attribute.


### Views
Views in Django Rest Framework are mostly the same as regular Django views. The minor differences are that Django Rest Views inherit from the APIView class instead of the View class. They also return a REST Response class instead of rendering a Django template or redirecting to another URL.

Here is an example of a Django Rest View:
```
from rest_framework.views import APIView
from rest_framework.response import Response

class ExampleAPIView(APIView):
    def get(self, request):
        # Your logic here to fetch data or perform an action
        data = {
            'message': 'Hello, this is a basic Django Rest Framework view!',
            'status': 'success'
        }
        
        return Response(data)
```
You can handle other HTTP requests by overriding their respective methods just like regular Django Views.

Writing views can be repetitive and time consuming so Django provides many generic views that you can use with minimal code. You only need to override methods in generic views if you want to customize their logic, otherwise you only need to specify a few values to use the view.

Here are all the generic views that Django Rest Framework includes:
```
CreateAPIView
    -implements the create method
    -called using HTTP POST request

RetrieveAPIView
    -implements the retrieve method
    -called using HTTP GET request

ListAPIVIew
    -implements the list method
    -called using HTTP GET request
    -must add queryset attribute or get_queryset method

UpdateAPIView
    -implements the update method
    -called using HTTP PUT or HTTP PATCH request

DestroyAPIView
    -implements destroy method
    -called using HTTP DELETE request
```

There are also generic views that combine multiple generic views:
```
ListCreateAPIView

RetrieveUpdateAPIView

RetrieveDestroyAPIView

RetrieveUpdateDestroyAPIView
```
These views essentially combine the functionality of the generic views that were mentioned above. This allows for one API endpoint to do different things based on the type of HTTP request that was sent to the endpoint.


### Customizing Generic Views

To customize the logic of generic views, you must override the perform_create, perform_update, or perform_destroy methods. 

For example, you could override the perform_create method to automatically make the user attribute of your model the current user when you are creating a new instance of the model.
```
from rest_framework.generics import CreateAPIView
from .serializers import YourModelSerializer

class CreateExample(CreateAPIView):
    serializer_class = YourModelSerializer

    def perform_create(self, serializer):
        # Customize logic here, for example, setting the user attribute
        user=self.request.user
        serializer.save(user=user)
```

 ### Django ORM
 As Django is a backend framework, it needs a database to store any data used by the web application. Django supports relational databases through its own built-in ORM. An [ORM](https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/) (Object Relational Mapper) provides a layer of abstraction from the database, allowing the user to define relations and make queries using objects, rather than having to write raw SQL code. This makes ORMs suitable for working with object-oriented programming. Another benefit of ORMs is that the application is separated from the actual database implementation, which means the database can be changed (for example, from SQLite to PostgreSQL once the app is ready for production) without affecting the app.

### An Example API
 
After installing and configuring Django Rest Framework, we may now create a simple API. We first create a new model to represent our API resource. To illustrate the use of Django Rest Framework, we'll create a simple model for a todo item with two fields: its title as a CharField, i.e., a string and its complete as a BooleanField, i.e., a boolean value. We first create a new file called models.py in the API app folder by adding the following code:

 In order to add Django Rest Framework to your project, open the settings.py file and add 'rest_framework' to the INSTALLED_APPS list: ```INSTALLED_APPS = [..., 'rest_framework']```
  
 After installing and configuring Django Rest Framework, we may now create a simple API. The first step is learning how to define a model in the database.

 ### Defining a model

Using the Django ORM, we can define models in the database by creating Python classes. Each table is represented by a class, with the columns represented by attributes. Django provides field types for various SQL data types. 

Django also comes with some predefined models, such as the [User model](https://docs.djangoproject.com/en/4.2/topics/auth/default/#user-objects) with built-in authentication. We won't be using the User model in this tutorial, but it can be very useful for applications that require users to have accounts. 

To create our API, we must first create a new model to represent our API resource. To illustrate the use of Django Rest Framework, we'll create a simple model for a to-do item with two fields: its title as a string, and its completed status as a boolean value. We first create a new file called models.py in the API app folder and define our model:

``` {python}
from django.db import models

class TodoItem(models.Model):
    title = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)
```

For this model, we have used a CharField for the string title. A CharField is used for small amounts of text, with a default max length of 255 characters. We have defined the max length to be 200 characters in this case. We have also used a BooleanField for the boolean completion status, and we have explicitly defined the default value to be False. Thus, whenever a value isn't provided for the completed field, it will automatically be set to False. 

Django has many more field types to represent various data types. Aside from the max length and default value, there are also other options that can be passed to these field types, such as whether a value can be null and whether a value can be blank. More information about field types and field options can be found [here](https://docs.djangoproject.com/en/4.2/ref/models/fields/).

Now that we have created a new model, we must create a migration for these changes to reflect in the database.

### Migrations
A migration is a file that tells the ORM what changes have been made to the models. Whenever we make changes to our models, we must run the following two commands:
- ```python3 manage.py makemigrations```: This command creates a migration file containing all the changes made since the last migration, similar to a Git history. These changes are not auto-applied as changing the database structure is an important (and potentially dangerous) operation that can result in data loss.
- ```python3 manage.py migrate```: This command applies changes from any unapplied migration files to the database using SQL code. This syncs the state of the database tables with the models defined as Python classes.

Be VERY careful with your migrations. If you accidentally delete a migration file, edit the migration file by hand, or edit the same model as someone else at the same time, you could create migration errors that are very complicated to resolve. If you are ever unsure, consult the [documentation](https://docs.djangoproject.com/en/4.2/topics/migrations/)!  

### Querying models
Once the database is fully set up with our models, we can write code that queries the database to find the specific data that we want. Each model has a ```.objects``` attribute, and all queries are methods of this attribute. The most basic query would be to get all the objects in that table - for our Todo class, this is done using ```TodoItem.objects.all()```, which returns a QuerySet (basically an unordered list) of all the TodoItem objects. There are many other queries found [here](https://docs.djangoproject.com/en/4.2/topics/db/queries/) that can be used to filter based on specific requirements and create and update objects.

 ### Creating an API

Assuming we want to create an API for your TodoItem model that allows clients to create, read, update, and delete TodoItem instances. We may define a serializer class in serializers.py in the app API folder that converts TodoItem instances to JSON. We add the following code in serializers.py:

``` {python}
from rest_framework import serializers
from .models import TodoItem

class TodoItemSerializer(serializers.ModelSerializer):
    class Meta:
        model = TodoItem
        fields = ('id', 'title', 'completed')
```

The code above converts instances of the Todo model to JSON format so that they can be returned in HTTP responses. This is where the TodoItemSerializer class comes in. It is defined in the serializers.py file and inherits from the ModelSerializer class provided by Django Rest Framework and defines the fields that should be serialized and the model that the serializer should be based on. In our case, we want to serialize the id, title, and completed fields of the TodoItem model, so we include these fields in the Meta class of the serializer.

Here is an example of what the serialization looks like:

``` {python}
from .models import TodoItem
from .serializers import TodoItemSerializer

todo1 = TodoItem.objects.create(title="Finish Assignment", completed=False)
todo2 = TodoItem.objects.create(title="Wash Dishes", completed=True)

serializer = TodoItemSerializer([todo1, todo2], many=True)
serialized_data = serializer.data

print(serialized_data)
```
The output of the print statement are our model instances but in JSON format
```
[
    {"id": 1, "title": "Finish Assignment", "completed": false},
    {"id": 2, "title": "Wash Dishes", "completed": true}
]
```

Next, we'll create a view to handle incoming HTTP requests (GET, POST, PUT, PATCH and DELETE). We create a new file called views.py in the API app folder and add the following code:

``` {python}
from rest_framework.generics import ListCreateAPIView, RetrieveUpdateDestroyAPIView
from .models import TodoItem
from .serializers import TodoItemSerializer

class TodoItemList(ListCreateAPIView):
    serializer_class = TodoItemSerializer
    
    def get_queryset(self):
        return TodoItem.objects.filter(completed=True)

class TodoItemDetail(RetrieveUpdateDestroyAPIView):
    serializer_class = TodoItemSerializer
    queryset = TodoItem.objects.all()


```
The code above uses the TodoItemSerializer we previously created to convert the instances to the correct JSON format. To illustrate further on this and its use cases, we consider the TodoItemList view and note that it queries the database by filtering Todo items that are completed. We do this by overriding the get_queryset method of the ListCreateAPIView class and return only the TodoItem instances where completed is True, i.e., Todo item is completed, using the filter method: ```TodoItem.objects.filter(completed=True)```. This way, when the TodoItemList view is accessed with a GET request, only the completed TodoItem instances will be returned in the response.

Finally, we can add a URL configuration for our API. We create a new file called urls.py in the API app folder and add the following code:

``` {python}
from django.urls import path
from .views import TodoItemList, TodoItemDetail

urlpatterns = [
    path('todos/', TodoItemList.as_view()),
    path('todos/<int:pk>/', TodoItemDetail.as_view()),
]
```

All in all, we have now built a simple API with the ability to list, create, update and delete todo list items. One can test the API by starting the Django development server using ``` python3 manage.py runserver ``` and visiting http://localhost:8000/todos/ in a web browser. Running this command will also show any errors that need to be fixed in the shell, and the server continuously checks for errors and updates whenever the code is changed.

### Extra Resources

Here are some official documentation and helpful tutorials for getting familiar with Django Rest Framework

#### Documentation
- [Django Rest](https://www.django-rest-framework.org/)
- [Django Rest API Guide](https://www.django-rest-framework.org/api-guide/)
- [Django Rest Views](https://www.django-rest-framework.org/api-guide/generic-views/)
- [Django Rest Testing](https://www.django-rest-framework.org/api-guide/testing/)

####  Tutorials
- [Django for Beginners](https://www.youtube.com/watch?v=F5mRW0jo-U4)
- [Learn Django](https://learndjango.com/tutorials/official-django-rest-framework-tutorial-beginners)
- [Real Python](https://realpython.com/django-rest-framework-quick-start)
- [Youtube (Corey Schafer)](https://www.youtube.com/watch?v=Uyei2iDA4Hs&list=PL-osiE80TeTtoQCKZ03TU5fNfx2UY6U4p)
