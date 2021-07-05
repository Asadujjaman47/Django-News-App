# Django-News-App
![](media/images/1.png)

#1. Create project and app
django-admin startproject newsproject .<br>
django-admin startapp newsapp

#2.install newsapi<br>
pip install newsapi-python

#3.add app in settings<br>
INSTALLED_APPS = [<br>
    'newsapp',	#<-----------------<br>
    'django.contrib.admin',<br>
    'django.contrib.auth',<br>
    'django.contrib.contenttypes',<br>
    'django.contrib.sessions',<br>
    'django.contrib.messages',<br>
    'django.contrib.staticfiles',<br>
]


#4. join template<br>
import os<br>

TEMPLATES = [<br>
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',<br>
        'DIRS': [os.path.join(BASE_DIR, "templates")],		#<--------------<br>
        'APP_DIRS': True,<br>
        'OPTIONS': {<br>
            'context_processors': [<br>
                'django.template.context_processors.debug',<br>
                'django.template.context_processors.request',<br>
                'django.contrib.auth.context_processors.auth',<br>
                'django.contrib.messages.context_processors.messages',<br>
            ],
        },
    },<br>
]



#5.newsapp/views.py

# importing api
from django.shortcuts import render<br>
from newsapi import NewsApiClient

# Create your views here.
def index(request):<br>
	
	newsapi = NewsApiClient(api_key ='426c4bdc8ea540e0b2e73967c105f457')	#<-----(Create your API key)<br>
	top = newsapi.get_top_headlines(sources ='techcrunch')

	l = top['articles']
	desc =[]
	news =[]
	img =[]

	for i in range(len(l)):
		f = l[i]
		news.append(f['title'])
		desc.append(f['description'])
		img.append(f['urlToImage'])
	mylist = zip(news, desc, img)

	return render(request, 'index.html', context ={"mylist":mylist})


#6. Create templates/index.html,

<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
	<meta charset="utf-8">
	<title></title>

<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
<!-- Optional theme -->
</head>
<body>
	<div class="jumbotron" style="color:black">

	<h1 style ="color:white">
Get The latest news on our website
	</h1>

	</div>


	<div class="container">
	{% for new, des, i in mylist %}
			<img src="{{ i }}" alt="">
			<h1>news:</h1> {{ new }}
			{{ value|linebreaks }}

			<h4>description:</h4>{{ des }}
			{{ value|linebreaks }}

	{% endfor %}
	</div>

</body>
</html>




#7. newsproject/urls.py

from django.contrib import admin<br>
from django.urls import path<br>
from newsapp import views<br>

urlpatterns = [<br>
path('', views.index, name ='index'),<br>
	path('admin/', admin.site.urls),<br>
]


#8.Run the project<br>
Python manage.py runserver<br>

and go http://127.0.0.1:8000/

![](media/images/2.png)






