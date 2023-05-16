<h3>- ایجاد پروژه جنگویی</h3>

1- نصب پکیج جنگو
<pre>
pip install django
</pre>
<hr>

2- ایجاد پوشه دیتابیس
<pre>
mkdir database
</pre>
<hr>


3- ورود به داخل پوشه دیتابیس
<pre>
cd database
</pre>
<hr>

4- ایجاد پروژه جنگویی
<pre>
django-admin startproject projectname
</pre>
<hr>

5- ایجاد یک اپ
<pre>
python manage.py startapp appname
</pre>
<hr>

6- settings.py در فایل INSTALL_APPS اضافه کردن اپ به لیست 
<pre>
INSTALL_APPS = [
    #...
    'appname',
]
</pre>

<hr>

7- projectname در پوشه ی urls.py اضافه کردن اپ به فایل 

<pre>
from django.urls import path,include

urlpatterns = [
    #...
    path('', include('newapp.urls')),  
]
</pre>
<hr>

8- دستور اجرا کردن پروژه
<pre>
python manage.py runserver
</pre>
<hr>

9- و زدن این کد ها در فایلappname در پوشه ی urls.py ایجاد فایل
<pre>
from django.urls import path
from . import views

urlpatterns = [
    # : مثال api ایجاد
    path('users', views.users,name='users'),
    path('add', views.add , name='add'),
    path('delete', views.delete , name='delete'),
    path('update', views.update , name='update')
]
</pre>
<hr>

<h3>- ORM</h3>

10- انجام می شود models.py اضافه کردن جداول به دیتابیس در فایل
<pre>
class Users(models.Model):
    firstname = models.CharField(max_length=100)
    lastname = models.CharField(max_length=100)
    country = models.CharField(max_length=100)
</pre>
<hr>

11- این دستور را اجرا کنید models.py برای اعمال تغییرات
<pre>
python manage.py makemigrations
</pre>
<hr>

12- در دیتابیس هم این تغییرات انجام شود این دستور را اجرا کنید models.py برای اینکه تغییرات
<pre>
python manage.py migrate
</pre>
<hr>

<h3>- API</h3>

13- api نصب پکیج 
<pre>
pip install djangorestframework
</pre>
<hr>

<h3>- CORS</h3>

14- cors نصب پکیج 
<pre>
pip install django-cors-headers
</pre>
<hr>

15- بنویسید settings.py برای استفاده از این پکیج ها کد های زیر را در فایل
<pre>
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
</pre>
<hr>

16- views.py در فایل api و cors و json ایمپورت کردن کتابخانه های 
<pre>
from django.http import JsonResponse
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.decorators import api_view
import json
</pre>
<hr>

17- views.py نوشتیم باید آن را ایمپورت کنیم درmodels.py هرتابعی که در 
<pre>
from .models import Users
</pre>
<hr>

18- views.py نوشتن کوئری ها و تابع ها در فایل  
<pre>
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
</pre>
<hr>

<h3>- MY SQL</h3>

19- mysql دانلود 
<pre>
<a href="https://dl.yasdl.com/2022/Software/MySQL.8.0.31.Community_YasDL.com.rar?hj">https://dl.yasdl.com/2022/Software/MySQL.8.0.31.Community_YasDL.com.rar?hj</a>
</pre>
<hr>

20- mysql نصب پیکج 
<pre>
pip install mysqlclient
</pre>
<hr>


21- ویرایش دهید و اطلاعات دیتابیسی که در مای اسکیوال ایجاد کرده اید را بنویسید settings.py کد زیر را در فایل
<pre>
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
</pre>
<hr>

22- JS در fetch نحوه درست
  
  <pre>
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
</pre>
