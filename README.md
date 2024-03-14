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

### Running test
```shell
python manage.py test app
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

### Use generic views: Less code is better
> * The **detail()** and **results()** views are very short – and, as mentioned above, redundant. **The index()** view, which displays a list of polls, is similar.
> * These views represent a common case of basic web development: getting data from the database according to a parameter passed in the URL, loading a template and returning the rendered template. Because this is so common, Django provides a shortcut, called the “generic views” system.
* **Amend URLconf**
  ```python
  # polls/urls.py
  ...
  path("<int:pk>/", views.DetailView.as_view(), name="detail"),
  path("<int:pk>/results/", views.ResultView.as_view(), name="results"),
  ...
  ```
  * The name of the matched pattern in the path strings of the second and third patterns has changed from <question_id> to <pk>.
  * This is necessary because we’ll use the DetailView generic view to replace our detail() and results() views, and it expects the primary key value captured from the URL to be called "pk".
* **Amend views**
  ```python
  ...
    class IndexView(generic.ListView):
        template_name = "polls/index.html"
        context_object_name = "latest_question_list"

        def get_queryset(self):
            """Return the last five published questions."""
            return Question.objects.order_by("-pub_date")[:5]


    class DetailView(generic.DetailView):
        model = Question
        template_name = "polls/detail.html"


    class ResultView(generic.DetailView):
        model = Question
        template_name = "polls/results.html"
  ...
  ```
  * Remove our old index, detail, and results views and use Django’s generic views instead.
  * Each generic view needs to know what model it will be acting upon.
  * This is provided using either the model attribute or by defining the get_queryset() method
  * By default, the **DetailView** generic view uses a template called **\<app name>/\<model name>_detail.html.**
    * It would use the template **"polls/question_detail.html"**.
  * Similarly, the **ListView** generic view uses a default template called **\<app name>/\<model name>_list.html**; we use template_name to tell ListView to use our existing **"polls/index.html"** template.
  * The templates have been provided with a context that contains the **question** and **latest_question_list** context variables.
  * For **DetailView** the **question** variable is provided automatically – since we’re using a Django model (**Question**), Django is able to determine an appropriate name for the context variable.
  * However, for ListView, the automatically generated context variable is **question_list**.
  * To override this we provide the **context_object_name** attribute, specifying that we want to use **latest_question_list** instead.
  
  <a href="https://docs.djangoproject.com/en/4.2/topics/class-based-views/">Generic views documentation</a>
  

<br>
<br>

# Project description

## ```polls/templates/polls/detail.html```

### Form
```html
<!-- polls/templates/polls/detail.html -->
<form action="{% url 'polls:vote' question.id %}" method="post">
...
```
> * Using **method="post"** (as opposed to **method="get"**) is very important, because the act of submitting this form will alter data server-side.

### Radio button
```html
<!-- polls/templates/polls/detail.html -->
    ...

    {% for choice in question.choice_set.all %}
      <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" />
      <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
    {% endfor %}

    ...
```

> * The above template displays a radio button for each question choice.
> * The **value** of each radio button is the associated question choice’s ID.
> * The **name** of each radio button is **"choice"**.
> * That means, when somebody selects one of the radio buttons and submits the form, it’ll send the POST data **choice=#** where # is the ID of the selected choice.


### ```forloop.counter```
> * **forloop.counter** indicates how many times the for tag has gone through its loop


### ```{% csrf_token %}```
> * Since we’re creating a POST form (which can have the effect of modifying data), we need to worry about Cross Site Request Forgeries.
> * All POST forms that are targeted at internal URLs should use the **{% csrf_token %}** template tag.


## ```polls/views.py```

### ```request```
```python
selected_choice = question.choice_set.get(pk=request.POST["choice"])
```
> * **request.POST** is a dictionary-like object that lets you access submitted data by key name.
> * **request.POST['choice']** returns the ID of the selected choice, as a string.
> * **request.POST** values are always strings.
> * **request.POST['choice']** will raise KeyError if choice wasn’t provided in POST data.

### ```HttpResponseRedirect```
> * After incrementing the choice count, the code returns an **HttpResponseRedirect** rather than a normal **HttpResponse**.
> * **HttpResponseRedirect** takes a single argument: the URL to which the user will be redirected.
> * You should always return an **HttpResponseRedirect** after successfully dealing with POST data

### ```reverse()```
> * This function helps avoid having to hardcode a URL in the view function.
> * It is given the name of the view that we want to pass control to and the variable portion of the URL pattern that points to that view.
> 
```"/polls/3/results/"```
> * where the **3** is the value of **question.id**. This redirected URL will then call the **'results'** view to display the final page.