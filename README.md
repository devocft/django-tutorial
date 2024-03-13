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

## Django

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