# Quickly create a Django project with API , ORM , and commands



1- نصب پکیج جنگو

pip install django
<hr>

2- ایجاد پروژه جنگویی

django-admin startproject projectname
<hr>

3- ورود به داخل پوشه پروژه

cd projectname
<hr>

4- ایجاد یک اپ

python manage.py startapp appname
<hr>


5- settings.py در فایل INSTALL_APPS اضافه کردن اپ به لیست 
INSTALL_APPS = [
    #...
    'appname',
]

6- projectname در پوشه ی urls.py اضافه کردن اپ به فایل 
from django.urls import path,include
urlpatterns = [
    #...
    path('', include('newapp.urls')),
]

7- دستور اجرا کردن پروژه
python manage.py runserver

8- و زدن این کد ها در فایلappname در پوشه ی urls.py ایجاد فایل   
from django.urls import path
from . import views
urlpatterns = [
    # : مثال api ایجاد

    path('users', views.users,name='users'),
    path('add', views.add , name='add'),
    path('delete', views.delete , name='delete'),
    path('update', views.update , name='update')
]

9- انجام می شود models.py اضافه کردن جداول به دیتابیس در فایل 
class Users(models.Model):
    firstname = models.CharField(max_length=100)
    lastname = models.CharField(max_length=100)
    country = models.CharField(max_length=100)

10- این دستور را اجرا کنید models.py برای اعمال تغییرات  
python manage.py makemigrations

11- در دیتابیس هم این تغییرات انجام شود این دستور را اجرا کنید models.py برای اینکه تغییرات  
python manage.py migrate

12- api نصب پکیج 
pip install djangorestframework

13- cors نصب پکیج  
pip install django-cors-headers

14- بنویسید settings.py برای استفاده از این پکیج ها کد های زیر را در فایل    
INSTALLED_APPS = [
    #...
    'rest_framework',
    'corsheaders',
]

MIDDLEWARE = [
    #...
    'corsheaders.middleware.CorsMiddleware',
]

CORS_ORIGIN_ALLOW_ALL = True
CORS_ALLOW_CREDENTIALS = True

15- views.py در فایل api و cors و json ایمپورت کردن کتابخانه های 
from django.http import JsonResponse
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.decorators import api_view
import json

16- views.py نوشتیم باید آن را ایمپورت کنیم درmodels.py هرتابعی که در 
from .models import Users

17- views.py نوشتن کوئری ها و تابع ها در فایل  
@api_view(['GET'])
def users(request):
    user = Users.objects.get(id=1)

    data = {
        'id': user.id,
        'firstname': user.firstname,
        'lastname': user.lastname,
        'country': user.country
    }

    return JsonResponse({'data': data})


@api_view(['POST'])
def add(request):
    data = json.loads(request.body)
    firstname = data['username']
    lastname = data['lastname']

    user = Users(firstname=firstname, lastname=lastname, country='mmd')
    user.save()

    return Response({'status': 'ok'})



@api_view(['DELETE'])
def delete(request):
    data = json.loads(request.body)
    userId = data['id']

    Users.objects.filter(id=userId).delete()

    return JsonResponse({'status': 'ok'})


@api_view(['PUT'])
def update(request):
    data = json.loads(request.body)
    userId = data['id']
    firstname = data['username']
    lastname = data['lastname']

    user = Users.objects.get(id=userId)
    user.firstname = firstname
    user.lastname = lastname
    user.save()

    return JsonResponse({'status': 'ok'})

18- mysql دانلود  
https://dl.yasdl.com/2022/Software/MySQL.8.0.31.Community_YasDL.com.rar?hj

19- mysql نصب پیکج 
pip install mysqlclient

20- ویرایش دهید و اطلاعات دیتابیسی که در مای اسکیوال ایجاد کرده اید را بنویسید settings.py کد زیر را در فایل  
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'projectname',
        'USER': 'root',
        'PASSWORD': 'root',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}

21- js در fetch نحوه درست
  
  //READ
  fetch('http://127.0.0.1:8000/users', {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json'
    },
  })
  .then(response => {
    return response.json()
  })
  .then(response => {
    let users = [response.data]
    users.forEach(user =>{
      idUser.value = user.id
      editusername.value = user.firstname
      editlastname.value = user.lastname
    })
  })
  .catch(error => {
    console.log(error);
  })

let formData = {
    "id": userid.value,
    "username": username.value,
    "lastname": lastname.value,
    }

    //CREATE
    fetch('http://127.0.0.1:8000/add', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
    })
    .then( response => {
    console.log(response);
    })
    .catch(error => {
    console.log(error);
    });
    
    // DELETE
    fetch('http://127.0.0.1:8000/delete', {
    method: 'DELETE',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
    })
    .then( response => {
    console.log(response);
    })
    .catch(error => {
    console.log(error);
    });


  let formData = {
    "id": idUser.value,
    "username": editusername.value,
    "lastname": editlastname.value,
  }
    //UPDATE
    fetch('http://127.0.0.1:8000/update', {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
  })
  .then( response => {
    console.log(response);
  })
  .catch(error => {
    console.log(error);
  });
