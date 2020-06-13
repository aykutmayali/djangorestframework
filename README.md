# djangorestframework

django rest framework>django-admin startproject myproject

in pycharm	// pip install djangorestframework

		//python manage.py startapp webapp


settings//
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'webapp'
]

webapp models.py//
from __future__ import unicode_literals

from django.db import models

class employees(models.Model):
    firstname=models.CharField(max_length=10)
    lastname=models.CharField(max_length=10)
    emp_id=models.IntegerField()

    def __str__(self):
        return self.firstname
django rest framework\myproject>python manage.py makemigrations

django rest framework\myproject>python manage.py createsuperuser

django rest framework\myproject>python manage.py migrate

django rest framework\myproject>python manage.py createsuperuser

django rest framework\myproject>python manage.py runserver
http://127.0.0.1:8000/ + admin login + create new employees


in webapp // add new py file serializers.py
from rest_framework import serializers
from .models import employees

class employeesSerializer(serializers.ModelSerializer):

    class Meta:
        model= employees
        fields='__all__'
in webapp // views.py

from __future__ import unicode_literals

from django.shortcuts import render

from django.http import HttpResponse
from django.shortcuts import get_object_or_404
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import employees
from .serializers import employeesSerializer

class employeeList(APIView):

    def get(self, request):
        employees1= employees.objects.all()
        serializer= employeesSerializer(employees1, many=True)
        return Response(serializer.data)
    def post(self):
        pass

in myproject // urls.py

from django.conf.urls import url
from django.contrib import admin
from rest_framework.urlpatterns import format_suffix_patterns
from webapp import views
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^employees/',views.employeeList.as_view()),
]


webapp admin.py//
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

from django.contrib import admin
from .models import employees

admin.site.register(employees)

# Register your models here.


http://127.0.0.1:8000/employees/
