# Djanog-Rest-Api-Example

## Install Packages

```
# pipenv install django
# pipenv install djangorestframework
```
## Active virtual environment

```
# pipenv shell
```
## Create new django project


```
# django-admin startproject api-example
```
## Create new app languages


```
# python manage.py startapp languages
```

## Migrate databes


```
# python manage.py migrate
```
## Create super user


```
# python manage.py createsuperuser
```


## update `api-example/setting.py`


``` python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework', # add
    'languages' # add 
]

```
## update `api-example/urls.py`
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls')) # add 
]
```

## update `languages/models.py`

```python
from django.db import models

class Language(models.Model):
    name = models.CharField(max_length=50)
    paradigm = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```


## Migrate database
```
# python manage.py makemigrations
# python manage.py migrate
```


## update `languages/admin.py`

```python
from django.contrib import admin
from .models import Language

admin.site.register(Language)

```

## Touch `languages/serializers.py`


```python
from rest_framework import serializers
from .models import Language

class LanguageSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Language
        fields = ('id', 'url', 'name', 'paradigm')
```


## update `languages/views.py`

```python
from django.shortcuts import render
from rest_framework import viewsets
from .models import Language
from .serializers import LanguageSerializer

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer
```



## update `languages/urls.py`

```python
from django.urls import path, include
from . import views 
from rest_framework import routers 

router = routers.DefaultRouter()
router.register('languages', views.LanguageView)

urlpatterns = [
    path('', include(router.urls))
]

```
---

## Model Relationship

Delete previous objects

## Update `language/model.py`

```python
from django.db import models

class Paradigm(models.Model):
	name = models.CharField(max_length=50)

	def __str__(self):
		return self.name

class Language(models.Model):
    name = models.CharField(max_length=50)
    paradigm = models.ForeignKey(Paradigm,on_delete=models.CASCADE)

    def __str__(self):
        return self.name


class Programmer(models.Model):
	name = models.CharField(max_length=50)
	language = models.ManyToManyField(Language)

	def __str__(self):
		return self.name

```

## Make migrations and Migrate
```
# python manage.py makemigrations
# python manage.py migrate
```

## Update `language/serializers.py`

```python
from rest_framework import serializers
from .models import Language , Paradigm, Programmer

class LanguageSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Language
        fields = ('id', 'url', 'name', 'paradigm')


class ParadigmSerializer(serializers.HyperlinkedModelSerializer):
	class Meta:
		model = Paradigm
		fields = ('id', 'url', 'name')


class ProgrammerSerializer(serializers.HyperlinkedModelSerializer):
	class Meta:
		model = Programmer
		fields = ('id', 'url', 'name','language')

```


## Update `language/views.py`

```python
from django.shortcuts import render
from rest_framework import viewsets
from .models import *
from .serializers import *

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer


class ParadigmView(viewsets.ModelViewSet):
    queryset = Paradigm.objects.all()
    serializer_class = ParadigmSerializer


class ProgrammerView(viewsets.ModelViewSet):
    queryset = Programmer.objects.all()
    serializer_class = ProgrammerSerializer
```



## Update `language/urls.py`

```python
from django.urls import path, include
from . import views 
from rest_framework import routers 

router = routers.DefaultRouter()
router.register('languages', views.LanguageView)
router.register('paradigms', views.ParadigmView)
router.register('programmers', views.ProgrammerView)

urlpatterns = [
    path('', include(router.urls))
]

```



---
# Django rest framework permission

## update `api-example/urls.py`

``` python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls')),
    path('api-auth/',include('rest_framework.urls')), # add 
]

```

## update `languages/views.py`

```python
from django.shortcuts import render
from rest_framework import viewsets, permissions # add 
from .models import *
from .serializers import *

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,) # add 

```

# Default permission

## update `api-example/settings.py`

```python
# add this in settings.py
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    )
}


# REST_FRAMEWORK = {
#     'DEFAULT_PERMISSION_CLASSES': (
#         'rest_framework.permissions.AllowAny',
#     )
# }
```



---
# Json web token

## Simple jwt

## installations 


```
# pip install djangorestframework_simplejwt
```

## Update api-example/settings.py

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ),

    #add
    'DEFAULT_AUTHENTICATION_CLASSES':
        ('rest_framework_simplejwt.authentication.JWTAuthentication',)
}
```
## Update api-example/urls.py
```python
from django.contrib import admin
from django.urls import path, include
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView # add 

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls')),
    path('api-auth/',include('rest_framework.urls')),
    path('api/token/',TokenObtainPairView.as_view()), # add 
    path('api/token/refresh/',TokenRefreshView.as_view()), # add 
]
```

## touch `languages/send.py`

## Install requests package
```
# pipenv install requests
```

## update `languages/send.py`

```python

```
