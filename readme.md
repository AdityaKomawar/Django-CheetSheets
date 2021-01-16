# Django Notes

- ## Setup Project

1. Install Django inside virtual environment

```
pipenv install django
```

---

2. Start virtual environment

```
pipenv shell
```

---

3. Start project in the current directory

```
django-admin startproject project_name .
```

---

4. Make app for project

```
manage.py startapp app_name
```

---

5. Add this app to the installed apps in **`settings.py`**

```python
INSTALLED_APPS = [

    ...

    # Apps created by me
    'app_name.apps.App_nameConfig',

]
```

---

6. Inistalize database

```
python manage.py migrate
```

---

7. Run server

```
python manage.py runserver
```

---

- ## Create database model

1. Go to **`app_name/models.py`**

```python
from django.db import models


class Post(models.Model):
    text = models.TextField()

    def __str__(self):
        return self.text[:50]
```

---

2. Activating Models

```
python manage.py makemigrations posts
```

> **Note:**
>
> Try to use app name after `makemigrations` command to make migration only for one app at a time, this will make it easy to see changes only related to this app and it will also help to revert back the changes if required.

---

3. Make those changes to the database

```
python manage.py migrate
```

---

4. Add our created model to admin

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

## Creating admin(superuser) to use our created models

1. Creating superuser

```
python manage.py create superuser
```

---

2. Run the server and go to **`http://127.0.0.1:8000/admin/`** login to the admin account and use created models to make some posts

---

## Displaying database content using **`view/template/urls`**

1. Using generic class ListView to pass contents to the template

```python
from django.views.generic import ListView
from .models import Post


class HomePageView(ListView):
    model = Post
    template_name = 'templates/home.html'
    content_object_name = 'all_posts_list' # renaming default object_list
```

---

2. Make project level **`/templates`** folder and create file **`templates/home.html`**

```html
<h1>Message board homepage</h1>
<ul>
  {% for post in all_posts_list %}
  <li>{{ post.text }}</li>
  {% endfor %}
</ul>
```

---

3. Telling our project to use this folder for templates

```python
TEMPLATES = [
    {
        import os

        ...

        'DIRS': [os.path.join(BASE_DIR, 'templates')], # new

        ...
    },
]
```

---

4. Setup urls: First Create make following changes in project level **`urls.py`**

```python
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('posts.urls')),
]
```

5. Them create app-level **`posts/urls.py`**

```python
from django.urls import path
from .views import HomePageView


urlpatterns = [
    path('', HomePageView.as_view(), name = 'home'),
]
```

---

6. Restart the server and go to **`http://127.0.0.1:8000/`**
   ![Home page image](/home.png)

---

7. Now add new posts by loging to admin panel and make new posts and see the changes

---

8.
