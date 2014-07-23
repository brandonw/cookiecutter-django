{{cookiecutter.project_name}}
==============================

{{cookiecutter.description}}


LICENSE: BSD

Settings
------------

{{cookiecutter.project_name}} relies extensively on environment settings which **will not work with Apache/mod_wsgi setups**. It has been deployed successfully with both Gunicorn/Nginx and even uWSGI/Nginx.

For configuration purposes, the following table maps the '{{cookiecutter.project_name}}' environment variables to their Django setting:

======================================= =========================== ============================================== ===========================================
Environment Variable                    Django Setting              Development Default                            Production Default
======================================= =========================== ============================================== ===========================================
DJANGO_AWS_ACCESS_KEY_ID                AWS_ACCESS_KEY_ID           n/a                                            raises error
DJANGO_AWS_SECRET_ACCESS_KEY            AWS_SECRET_ACCESS_KEY       n/a                                            raises error
DJANGO_AWS_STORAGE_BUCKET_NAME          AWS_STORAGE_BUCKET_NAME     n/a                                            raises error
DJANGO_CACHES                           CACHES                      locmem                                         memcached
DJANGO_DATABASES                        DATABASES                   See code                                       See code
DJANGO_DEBUG                            DEBUG                       True                                           False
DJANGO_EMAIL_BACKEND                    EMAIL_BACKEND               django.core.mail.backends.console.EmailBackend django.core.mail.backends.smtp.EmailBackend
DJANGO_SECRET_KEY                       SECRET_KEY                  CHANGEME!!!                                    raises error
DJANGO_SECURE_BROWSER_XSS_FILTER        SECURE_BROWSER_XSS_FILTER   n/a                                            True
DJANGO_SECURE_SSL_REDIRECT              SECURE_SSL_REDIRECT         n/a                                            True
DJANGO_SECURE_CONTENT_TYPE_NOSNIFF      SECURE_CONTENT_TYPE_NOSNIFF n/a                                            True
DJANGO_SECURE_FRAME_DENY                SECURE_FRAME_DENY           n/a                                            True
DJANGO_SECURE_HSTS_INCLUDE_SUBDOMAINS   HSTS_INCLUDE_SUBDOMAINS     n/a                                            True
DJANGO_SESSION_COOKIE_HTTPONLY          SESSION_COOKIE_HTTPONLY     n/a                                            True
DJANGO_SESSION_COOKIE_SECURE            SESSION_COOKIE_SECURE       n/a                                            False
======================================= =========================== ============================================== ===========================================

* TODO: Add vendor-added settings in another table

Getting up and running
----------------------

1. Run ``vagrant up`` to create, start, and provision your development virtual machine.
2. Run ``vagrant ssh`` to ssh into the newly provisioned virtual machine.
3. Run ``sudo su - postgres`` to switch to the postgres user in order to configure the database.
4. Run ``createuser --pwprompt`` to create a new database user with a password.
5. Run ``createdb {{cookiecutter.repo_name}}`` to create a new database for your project.
6. Change directory to ``/vagrant`` which is the shared folder between the virtual machine and your local project.
7. Update ``{{cookiecutter.repo_name}}/config/settings.py``'s ``DATABASE``: ``postgres://user:pass@localhost/{{cookiecutter.repo_name}}``
8. Run ``python {{cookiecutter.repo_name}}/manage.py syncdb`` to create tables
9. Run ``python {{cookiecutter.repo_name}}/manage.py migrate`` to install migrations
10. Run ``python {{cookiecutter.repo_name}}/manage.py createsuperuser`` to create a superuser
11. Run ``python {{cookiecutter.repo_name}}/manage.py collectstatic`` to collect all static files from both your project and any dependencies.
12. Move all static files found from ``staticfiles`` into ``{{cookiecutter.repo_name}}/static``. This will allow nginx to maintain its existing
    static files route, while still allowing you to easily change your local static files. Steps 10 and 11 will need to be repeated any time
    the static files outside of your local project change, otherwise nginx will not see the changes made.

You can now run ``grunt serve`` to serve the app, which you can then view from your host at localhost:8000. This will enable auto-reloading
of the uwsgi server on any static file changes, including Sass / Comapss CSS compilation.

While your ``vagrant ssh`` session is running ``grunt serve``, you can now code from your host through the shared folder. It's time to write the code!!!

Deployment
------------

Run these commands to deploy the project to Heroku:

.. code-block:: bash

    heroku create --buildpack https://github.com/heroku/heroku-buildpack-python
    heroku addons:add heroku-postgresql:dev
    heroku addons:add pgbackups:auto-month
    heroku addons:add sendgrid:starter
    heroku addons:add memcachier:dev
    heroku pg:promote DATABASE_URL
    heroku config:set DJANGO_CONFIGURATION=Production
    heroku config:set DJANGO_SECRET_KEY=RANDOM_SECRET_KEY_HERE
    heroku config:set DJANGO_AWS_ACCESS_KEY_ID=YOUR_AWS_ID_HERE
    heroku config:set DJANGO_AWS_SECRET_ACCESS_KEY=YOUR_AWS_SECRET_ACCESS_KEY_HERE
    heroku config:set DJANGO_AWS_STORAGE_BUCKET_NAME=YOUR_AWS_S3_BUCKET_NAME_HERE
    heroku config:set SENDGRID_USERNAME=YOUR_SENDGRID_USERNAME_HERE
    heroku config:set SENDGRID_PASSWORD=YOUR_SENDGRID_PASSWORD_HERE
    git push heroku master
    heroku run python {{cookiecutter.repo_name}}/manage.py syncdb --noinput --settings=config.settings
    heroku run python {{cookiecutter.repo_name}}/manage.py migrate --noinput --settings=config.settings
    heroku run python {{cookiecutter.repo_name}}/manage.py createsuperuser
    heroku open

