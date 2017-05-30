<h1 align="center">Django Tutorial</h1>

### My personal repository to learn Django from [DjangoProject.com](https://docs.djangoproject.com/en/1.11/intro/)

<br>

<h2 align="center">Using <code>virtualenv</code></h2>

(note: used as a "container" environment for the project)

### Install

```bash
sudo pip install virtualenv
```

### Start a new `virtualenv` "container"

```bash
virtualenv directory/name
```

To use Python3 instead:

```bash
virtualenv -p Python3 directory/name
```

### Usage

#### activate

```bash
source bin/activate
```

#### deactivate

```bash
deactivate
```

#### example

notice how, while activated, python and pip point to the packages defined inside the environment:

![](./readme-assets/virtualenv-activate-example.png)


<h2 align="center">Install Django</h2>

With `virtualenv` [activated](#usage), run the following command:

```bash
pip install Django
```

#### example

![](./readme-assets/install-django-example.png)


<h1 align="center">The Django Project Tutorial</h1>

Tutorial Reference: [https://docs.djangoproject.com/en/1.11/intro/tutorial01/](https://docs.djangoproject.com/en/1.11/intro/tutorial01/)

<br>

### Start a new Django project

```bash
django-admin startproject nameOfYourSite
```

### Run the Development Django Web Server

```bash
python manage.py runserver
```

<br>

<h2 align="center">Models</h2>

### Models and Database Migration

from: [https://docs.djangoproject.com/en/1.11/intro/tutorial02/#activating-models](https://docs.djangoproject.com/en/1.11/intro/tutorial02/#activating-models)

_**The 3-Step Guide to Model Changes**_
* _**change**_ your models (in `models.py`)
* run `python manage.py makemigrations` to _**create**_ migration for those changes
* run `python manage.py migrate` to _**apply**_ those changes to the databases

<br>

### Django/Python3 Shell (important)

Rather than calling `python` in our `virtualenv`, we use

```bash
python manage.py shell
```

`manage.py` will set "the DJANGO_SETTINGS_MODULE environment variable, which gives Django the Python import path to your mysite/settings.py file."

<br>

<h2 align="center">Views</h2>

### `URLConf`

"A [`URLConf`](https://docs.djangoproject.com/en/1.11/intro/tutorial03/#overview) maps URL patterns to **views**."
See [`djnago.urls`](https://docs.djangoproject.com/en/1.11/ref/urlresolvers/#module-django.urls).

### Responsibility

From: [Django >> #write-views-that-actually-do-something](https://docs.djangoproject.com/en/1.11/intro/tutorial03/#write-views-that-actually-do-something)

(**Important**) Each view _**must be**_ responsible for at least 1 of 2 things:
* returning an `HttpResponse` object
* raising an HTTP 404 exception

The rest, you can determine...
* read database records
* use Django's template system or a 3rd party's
* generate a PDF, XML, ZIP, etc.
* execute Python libraries

### The `render()` shortcut method
From: [Django >> #a-shortcut-render](https://docs.djangoproject.com/en/1.11/intro/tutorial03/#a-shortcut-render)

**Motivation:** It is very common to perform the following idiom:
```Python
from django.http import HttpResponse
from django.template import loader

def view_name(request):

    # (1) load the template
    template = loader.get_template('app_name/index.html')

    # (2) fill the context dictionary
    context = {
        'context_dictionary': context_object,
    }

    # (3) return the required HttpResponse
    return HttpResponse(template.render(context, request))
```

**Shortcut:** Django provides the following as a Shortcut

```Python
from django.shortcuts import render

def view_name(request):
  # (1) fill the context
  context = {'context_dictionary': context_object}

  # (2) render loads the response and satisfies the HttpResponse
  return render(request, 'app_name/index.html', context)
```

### The `get_object_or_404()` shortcut method
From: [Django >> #a-shortcut-get-object-or-404](https://docs.djangoproject.com/en/1.11/intro/tutorial03/#a-shortcut-get-object-or-404)

**Motivation:** It is very common to perform the following idiom; in addition, not using the subsequent shortcut couples the _view_ and _model_ layers of Django architecture which is ill-advised:
```Python
from django.http import Http404
from django.shortcuts import render

def view_name(request):

  # (1) perform a get()
  try:
    var_name = Object.objects.get(<object_key>)

  # (2) if get() is undefined, raise an exception
  except Object.DoesNotExist:
    raise Http404("<Object> does not exist")

  # (3) render a response
  return render(request, 'app_name/view_name.html', {'var_name': var_name})
```

**Shortcut:** Django provides the following as a Shortcut

```Python
from django.shortcuts import get_object_or_404, render

def view_name(request):

  # (1) perform the try/except idiom
  var_name = get_object_or_404(<object>)

  # (2) render a response
  return render(request, 'app_name/view_name.html', {'var_name': var_name})
```

<br>

<h2 align="center">Templates</h2>
Note: Django will automatically search for a directory called `templates` in each application context.

### Template Namespacing

#### Folder Structure
(**Important**)
Django chooses application templates by *order*. To prevent confusion, it is convention to "namespace" your templates by "putting those templates inside another directory named for the application itself".

* <application_name>
  * ...
  * `templates`
    * <appliaction_name> _**(this is namespacing)**_
      * ...
      * ...
  * ...

#### URLs
(**Important**) To prevent confusion in URLs, defining the `app_name` variable in `urls.py` of your Django application is convention:

In your `urls.py` file:
```Python
# urls.py

from django.conf.urls import url
from . import views

app_name = '<app_name>'

urlpatterns = [
  # ...
  url(<regexp>, views.<view_name>, name='<view_name>'),
  # ...
]
```

In your `templates/<app_name>/<template_name>.html` file:
```html
<!-- templates/<app_name>/<template_name>.html -->

<a href="{% url '<app_name>:<view_name>' %}"
```
