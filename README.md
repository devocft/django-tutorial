# django-tutorial

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
<br/>

# Django

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

<br/>
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