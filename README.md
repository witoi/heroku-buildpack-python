Django and Heroku Cookbook
==========================

Collection of snippets and scripts that solve certain problems when deploying
Django apps to Heroku.

Scripts in the `bin` directory are post-compile hooks that are invoked by
the `heroku-buildpack-python`'s
[compile](https://github.com/heroku/heroku-buildpack-python/blob/master/bin/compile)
step. You can install them by copying the `bin` directory into the root
directory of your Heroku application repository.

Installing NodeJS and Less compiler for static assets compilation in your Django app
------------------------------------------------------------------------------------

Heroku provides a bunch of different
[buildpacks](https://devcenter.heroku.com/articles/buildpacks) that target many
popular platforms like Python, Ruby, NodeJS and Java web apps and backends.
While this is great and allows you to deploy virtually anything with a simple
git command, the out-of-the-box solutions offer a limited set of utilities
that are available during the
[Slug compilation](https://devcenter.heroku.com/articles/slug-compiler) phase.
In particular no NodeJS, NPM or [LESS Compiler](http://lesscss.org/)
is available in the `heroku-buildpack-python`. This means that there
is no straightforward way of compiling `.less` stylesheets during
the app deployment.

Fortunately, the Python buildpack provides hooks for running pre-compile
and post-compile scripts. This can be used for customizing the compilation
step and running additional commands without the necessity of maintaining
a separate fork of Heroku's buildpack.
The only thing you need to do is to create a proper `bin/post_compile` bash
script in the root directory of your application.

The [bin](https://github.com/nigma/heroku-django-cookbook/tree/master/bin) directory
contains a set of scripts that can be used to install NodeJS/Less and invoke
the `manage.py compress` command in your Django application:

- [bin/post_compile](https://github.com/nigma/heroku-django-cookbook/tree/master/bin/post_compile)
- [bin/install_nodejs](https://github.com/nigma/heroku-django-cookbook/tree/master/bin/install_nodejs)
- [bin/install_less](https://github.com/nigma/heroku-django-cookbook/tree/master/bin/install_less)
- [bin/run_compress](https://github.com/nigma/heroku-django-cookbook/tree/master/bin/run_compress)

Just copy them over to your app reposiory and have your Less stylesheets
compiled with an assets compressor like
[Django Compressor](https://github.com/jezdez/django_compressor).

A note on hosting static files on Amazon S3. Remember to enable the
[environment variables](https://devcenter.heroku.com/articles/django-assets#config-vars-during-build)
if you are using django-storages and uploading static assets to S3:

`heroku labs:enable user-env-compile`


TBC
