cookiecutter-django
=======================

A cookiecutter_ template for Django.

.. _cookiecutter: https://github.com/audreyr/cookiecutter

Features
---------

* For Django 1.7
* Twitter Bootstrap_ 3
* AngularJS_
* Settings management via django-configurations_
* Registration via django-allauth_
* User avatars via django-avatar_
* Procfile_ for deploying to Heroku
* Heroku optimized requirements
* Basic caching setup
* Grunt build for compass and livereload
* Basic e-mail configurations for send emails via SendGrid_
* Vagrant ready to go, provisioned with Ansible

.. _Bootstrap: https://github.com/twbs/bootstrap
.. _AngularJS: https://github.com/angular/angular.js
.. _django-configurations: https://github.com/jezdez/django-configurations
.. _django-allauth: https://github.com/pennersr/django-allauth
.. _django-avatar: https://github.com/jezdez/django-avatar/
.. _Procfile: https://devcenter.heroku.com/articles/procfile
.. _SendGrid: https://sendgrid.com/


Constraints
-----------

* Only maintained 3rd party libraries are used.
* PostgreSQL everywhere
* Environment variables for configuration (This won't work with Apache/mod_wsgi).


Usage
------

Let's pretend you want to create a Django project called "redditclone". Rather than using `startproject`
and then editing the results to include your name, email, and various configuration issues that always get forgotten until the worst possible moment, get cookiecutter_ to do all the work.

First, install ansible via your package manager. It will be needed once a vagrant VM is provisioned.

Then, get cookiecutter. Trust me, it's awesome::

    $ pip install --user cookiecutter

Now run it against this repo::

    $ cookiecutter https://github.com/brandonw/cookiecutter-django.git

You'll be prompted for some questions, answer them, then it will create a Django project for you.


**Warning**: After this point, change 'Daniel Greenfeld', 'pydanny', etc to your own information.

It prompts you for questions. Answer them::

    Cloning into 'cookiecutter-django'...
    remote: Counting objects: 550, done.
    remote: Compressing objects: 100% (310/310), done.
    remote: Total 550 (delta 283), reused 479 (delta 222)
    Receiving objects: 100% (550/550), 127.66 KiB | 58 KiB/s, done.
    Resolving deltas: 100% (283/283), done.
    project_name (default is "project_name")? Reddit Clone
    repo_name (default is "repo_name")? redditclone
    author_name (default is "Your Name")? Daniel Greenfeld
    email (default is "Your email")? pydanny@gmail.com
    description (default is "A short description of the project.")? A reddit clone.
    year (default is "Current year")? 2014
    domain_name (default is "Domain name")?


Enter the project and take a look around::

    $ cd redditclone/
    $ ls

Create a GitHub repo and push it there::

    $ git init
    $ git add .
    $ git commit -m "first awesome commit"
    $ git remote add origin git@github.com:brandonw/redditclone.git
    $ git push -u origin master

Now take a look at your repo. Don't forget to carefully look at the generated README. Awesome, right?
