Simple Login and User Registration Application using Django
##############################################################
Ref: http://mayukhsaha.wordpress.com/2013/05/09/simple-login-and-user-registration-application-using-django/

django-admin.py startproject django-user-registration

django-admin.py startapp login

urls.py
########
1) 
from django.conf.urls import patterns, include, url
from login.views import *

urlpatterns += patterns('',
    url(r'^loginapp/$', 'django.contrib.auth.views.login'),
    url(r'^loginapp/logout/$', logout_page),
    url(r'^loginapp/accounts/login/$', 'django.contrib.auth.views.login'), # If user is not login it will redirect to login page
    url(r'^loginapp/register/$', register),
    url(r'^loginapp/register/success/$', register_success),
    url(r'^loginapp/home/$', home),
)

2) 
LOGIN_URL = '/accounts/login/'
LOGIN_REDIRECT_URL = '/accounts/profile/'

Settings.py
############
1)DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
        'NAME': 'django_db',                      # Or path to database file if using sqlite3.
        # The following settings are not used with sqlite3:
        'USER': 'XXXX',
        'PASSWORD': 'XXXX',
        'HOST': '127.0.0.1',                      # Empty for localhost through domain sockets or '127.0.0.1' for localhost through TCP.
        'PORT': '3306',                      # Set to empty string for default.
    }
}

2)
INSTALLED_APPS = (
		'login',
)

python manage.py syncdb

How to start server and access URLs
#####################################
python manage.py runserver

http://127.0.0.1:8000/loginapp/home      -> This will take you to login screen since we used decorator as shown below
http://127.0.0.1:8000/loginapp/register  -> This will take you to user registration page
http://127.0.0.1:8000/loginapp/register/success  -> Registration success page

views.py
#########
from django.contrib.auth.decorators import login_required
from django.contrib.auth import logout

@login_required
def home(request):
    return render_to_response(
    'home.html',
    { 'user': request.user }
    )

forms.py
##########
We make use of Django User table to to register a new user 
	
