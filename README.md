# django-tutorial
https://docs.djangoproject.com/en/4.2/contents/


## Default

### Check installation
```shell
python -m django --version
```

### Install django
```shell
pip install django
```

### Make a requirements.txt
```shell
pip freeze > requirements.txt
```

### Install dependancies
```shell
pip install -r requirements.txt
```

<br/>
<br/>

# Django

## Setting

### Database
```python
# mysite/settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

### Activate models
```python
# mysite/settings.py
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

## Command


### Create a project
```shell
# new directory
django-admin startproject project

# current directory
django-admin startproject project .
```


### Run server
```shell
# default port : 8000
python manage.py runserver

# changing the port : 8080
python manage.py runserver 8080
```


### Create a app
```shell
python manage.py startapp app
```


### Migrate
> need to create the tables in the database
```shell
python manage.py migrate
```


### Migration for app
```shell
python manage.py makemigrations polls
```


### SQL migrate
```shell
python manage.py sqlmigrate polls 0001
```


### Migrate
> create those model tables in your database
```shell
python manage.py migrate
```

### Step for Model Change
* Change your models(in **models.py**)<br/>
* Run ```python manage.py makemigrations``` to create migrations for those changes<br/>
* Run ```python manage.py migrate``` to apply those changes to the database


### Playing with the API
```shell
python manage.py shell
```

### Creating an admin user
```shell
$ python manage.py createsuperuser

Username : admin
Email address: admin@example.com
Password: 
Password (again):
```

<br/>
<br/>

# Django function

## ```path()```
<details>

### ```path()``` argument: route
> **route** is a string that contains a URL pattern.

### ```path()``` argument: view
> When Django finds a matching pattern, it calls the specified view function with an **HttpRequest** object as the first argument and any “captured” values from the route as keyword arguments.

### ```path()``` argument: kwargs
> Arbitrary keyword arguments can be passed in a dictionary to the target view.

### ```path()``` argument: name
> Naming your URL lets you refer to it unambiguously from elsewhere in Django, especially from within templates.
> 
</details>


## ```render()```
> It’s a very common idiom to load a template, fill a context and return an **HttpResponse** object with the result of the rendered template.


## ```get_object_or_404()```
> It’s a very common idiom to use **get()** and raise **Http404** if the object doesn’t exist.


<br>
<br>


# Django guide

## Templates

### Remove hardcoded URLs
```html
<!-- polls/index.html -->
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```
> you can remove a reliance on specific URL paths defined in your url configurations by using the **{% url %}** template tag
```html
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```