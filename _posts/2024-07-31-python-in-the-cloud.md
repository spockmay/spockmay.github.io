---
layout: post
title: "Python in the Cloud"
author: "spockmay"
categories: journal
tags: [computing, programming]
image: cyber_tree.png
---

I've written dozens of Python scripts for work over the past decade that start out as a tool for me and then evolve into a critical component of multiple people's jobs. That results in me having to teach everyone how to do things like: download code from gitlab/github, run and maintain a Python environment locally, upgrade their code without destorying settings files, etc. I'm now being asked to make a tool for the sales team to use and I have to draw the line. I can't teach an entire sales team to use and maintain software. So what I need to do is to move my code to the cloud where I can use devops tools to maintain the code, each person will always interface with the latest version, no one will need to have IDEs installed and managed, and I can actually access control the resources. Sounds perfect. Now how do I get there?

The first option into my head is Dockerized Django apps running on Amazon ECS behind Google Auth. Some other options:
* Anaconda provides a free/low-cost Python console in the cloud called [PythonAnywhere](https://www.pythonanywhere.com/)
That's about it as far as I can tell. Every other query I come up with for search (or even GenAI) leads to running scripts on an EC2 or Google Cloud instance.

# To do list
- [ ] build a Django UI for the script(s)
- [ ] learn how to containerize the solution
- [ ] put the Django app behind Google Auth
- [ ] deploy to Amazon ECS
- [ ] develop a way to maintain the code (what CI/CD do I need?)

All code for this project will be stored [here](https://github.com/spockmay/django-google-auth/tree/main)

# Step 1 - Hello World
First things first, let's get a simple Django app up and running in a container.

There's a handy tutorial [here](https://blog.logrocket.com/dockerizing-django-app/) that I'm starting with. Naturally though, the docker install instructions are broken so always go to the [source](https://docs.docker.com/engine/install/).

If you are new to Django, I highly recommend the [Hello World](https://djangoforbeginners.com/hello-world/) walkthrough instead of the Django Foundation's "getting started" project. It is significantly more accessible and minimal.

Once the app is made, containerizing it is pretty straightforward. I used the logrocket tutorial as well as a [dockerize.io](https://dockerize.io/guides/python-django-guide) tutorial to build up a minimal Dockerfile.

```sudo docker build -t python-django .```

The only problem that I had is that I forgot to add Django to my `requirements.txt` file so when I started the container I got a error asking about virtual environments. 

```sudo docker run -it -p 8000:8000 python-django```

I then wanted to make this a little more realistic of a Hello World by using the template system and instead of a view returnin the text, have the view return the template. This requires making the template directory (app/templates/app), populating some basic templates, updating the view, and possibly? adding `app` to the TEMPLATES / DIR in settings.py.

# Step 2 - Google Auth
The next big unknown for me is putting my Hello World behind Google Auth. There appear to be three ways to do this:
1. Roll your own
2. Use the Google SDK
3. Use the django-allauth plugin
For the sake of only doing what I can do, I'm going with option 3. [Hacksoft](https://www.hacksoft.io/blog/adding-google-login-to-your-existing-django-and-django-rest-framework-applications) has a good walkthrough on the process for approaches 1 and 2. Since I'm using 3 I'm going to use [this walkthrough](https://pylessons.com/django-google-oauth)

I found that I had to add `requests` and `jwt` to the `requirements.txt` to even get things to run.

As should be unsuprising, Google's interface has changed significantly from that in the walkthrough. Now you have to setup your app's consent page before you can do much of anything (make sure to add email address to scope!). THen you can go to Credentials > Create Credentials > OAuth client ID to setup the app as described.

Unfortunately after following everything it doesn't actually appear to work. I get the login prompt but the login fails. So back to the fundamental [docs](https://docs.allauth.org/en/latest/socialaccount/introduction.html)...

The first page of the docs clears up the earlier confusion about requirements, I had to specify the `socialaccounts` as a dependency and things work without requests or jwt having to be manually specified.

I think the main thing I was missing was a need to create user accounts within Django before authenticating via social. But that's not what I want to do, I really want any user that Google authenticates to get an account on the server. I will then use Google to limit auth to only those accounts I want to be able to have access. That is more maintainable than the other approach. After reading the full list of settings values in django-allauth's configuration I settled on the following for at least initial testing/validation.

```
SOCIALACCOUNT_PROVIDERS = {
        'google': {
            'SCOPE': [
                'profile','email'],
            'AUTH_PARAMS': {
                'access_type': 'offline',}, #'online',},
            'OAUTH_PKCE_ENABLED': True,
            'EMAIL_AUTHENTICATION': True,
            'SOCIALACCOUNT_EMAIL_AUTHENTICATION_AUTO_CONNECT': True,
            'SOCIALACCOUNT_ONLY': False, # eventaully change to True
            'FETCH_USERINFO': True,
            }
        }
```

With these and with the SITE_ID set correctly (2 in my case) I was finally able to login using Google!

I pretty easily added a logout (using the default Django auth logout) and the main page now shows different things depending on if you are logged in or not.

The biggest open item is that the standard `@login_required` decorator doesn't really work with allauth. It tries to direct me to the default login page (users/login).  Maybe I should force that page to go to my login page...

## Time passes...
So after a weekend not working on this I came back to it...and instead of getting the Google login button I got the Django default login page. ARGH! I spent hours messing with things until I realized a few points:
1. The default Django login page was hiding the fact that my login page won't load due to not knowing how to parse the `{% provider_login_url 'google'%}` statement. 
2. I had rebuilt my application but had NOT re-created the local Social Application settings.
So after fixing that, I was able to log in with Google and get back to where I was last week.

I then made the change I had been contemplating about the `@login_required` decorator where I changed by `urls.py` to move the login pages overtop of the default Django login pages. This now cleanely will redirect the user to the login page when they try to access the secret page behind auth directly.
```
path("accounts/login/", login_template, name='login'),
path('accounts/logout/', auth_views.LogoutView.as_view(template_name='users/logout.html'), name='logout'),
```

