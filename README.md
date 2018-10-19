# Django Practice

Practice Django

- Tutorial: [django project intro][1]
- OS: Ubuntu 16.04 LTS
- Python version: 2.7.12

## Install guide

### MySQL database with docker

```bash
sudo apt-get install docker.io
sudo docker pull mysql

MYSQL_USER="yho"
MYSQL_DATABASE="django_db"
MYSQL_CONTAINER_NAME="django-mysql"
MYSQL_ROOT_PASSWORD="1111"
MYSQL_PASSWORD="1111"
MYSQL_LOCAL_VOLUME="/opt/mysql"
sudo docker run \
  --detach \
  --volume ${MYSQL_LOCAL_VOLUME}:/var/lib/mysql \
  --env MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD} \
  --env MYSQL_USER=${MYSQL_USER} \
  --env MYSQL_PASSWORD=${MYSQL_PASSWORD} \
  --env MYSQL_DATABASE=${MYSQL_DATABASE} \
  --name ${MYSQL_CONTAINER_NAME} \
  --publish 3306:3306 \
  mysql;

sudo apt-get install mysql-client
```

> ref
> - [Docker로 MySQL 사용하기][2]

### Django - an official release with pip

```bash
sudo python get-pip.py
sudo pip install virtualenv
sudo pip install Django
```

> ref
> - [Installation - pip][3]
> - [Installation - virtualenv][4]

### Verifying

```bash
python -m django --version
```

## Writting django app

### Creating a project

`mysite` is project directory name.

```bash
cd {project-path}
django-admin startproject mysite
```

### Development server

```bash
cd mysite
python manage.py runserver
```

Then,

```bash
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

8월 11, 2017 - 15:50:53
Django version 1.11, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

### Creating the polls app

In mysite directory:

```bash
python manage.py startapp polls
```

### Write first view

Write `polls/views.py` file.

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

Write `polls/urls.py` file.

```python
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```

Write `mysite/urls.py` file.

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```

### Database setup

```bash
python manage.py migrate
```

### Creating models

Write `polls/models.py` file.

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

### Activating models

Add polls app's AppConfig to `mysite/settings.py` file.

```python
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

Execute command.

```bash
python manage.py makemigrations polls
```

Then,

```bash
Migrations for 'polls':
  polls/migrations/0001_initial.py
    - Create model Choice
    - Create model Question
    - Add field question to choice
```

[1]: https://docs.djangoproject.com/en/1.11/intro/
[2]: http://gyuha.tistory.com/490
[3]: https://pip.pypa.io/en/stable/installing/
[4]: https://virtualenv.pypa.io/en/stable/installation/