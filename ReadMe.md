# Project Setup

> https://symfony.com/

> https://symfony.com/doc/current/setup.html

> composer create-project symfony/skeleton app2116

> php -S 127.0.0.1:8000 -t public

> symfony server:start

> composer dump-autoload

> composer create-project symfony/website-skeleton ./ "5.0.*"

``` 
<VirtualHost *:80>   
	ServerName local.app2116
	DocumentRoot "C:\xampp\htdocs\app2116\public" 
</VirtualHost>
```

```
127.0.0.1        local.app2116
```

> https://symfony.com/download

> symfony check:requirements

> composer require symfony/requirements-checker

> http://local.app2116/check.php

> composer remove symfony/requirements-checker


# Symfony Packages

> 



# Database 

> .env

```
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
```

> bin/console doctrine:database:create
 
> bin/console doctrine:schema:update --force

> bin/console doctrine:schema:drop -n -q --force --full-database

```
bin/console doctrine:schema:drop -n -q --force --full-database &&
rm migrations/*.php &&
bin/console doctrine:schema:update --force &&
bin/console doctrine:fixtures:load -n -q
```


# .htaccess file in public folder

```
<IfModule mod_rewrite.c>
    Options -MultiViews
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>

<IfModule !mod_rewrite.c>
    <IfModule mod_alias.c>
        RedirectMatch 302 ^/$ /index.php/
    </IfModule>
</IfModule>
```


# Commands

> bin/console

> bin/console make:controller WelcomeController

> bin/console doctrine:database:create

> bin/console make:entity

> bin/console make:migration

> bin/console make:fixture

> bin/console make:twig-extension

> bin/console doctrine:migrations:migrate

> bin/console debug:container

> bin/console debug:event

> bin/console cache:clear

> bin/console list doctrine

> bin/console doctrine:fixtures:load -n -q

> bin/console debug:router

> bin/console debug:autowiring

> bin/console help make:migration

> bin/console about

> bin/console debug:event-dispatcher

> bin/console debug:event-dispatcher kernel.request

> bin/console make:subscriber

> bin/console make:form VideoFormType

> bin/console swiftmailer:spool:send --time-limit=10

> bin/console make:functional-test

> bin/console make:test

> ./bin/phpunit

> php bin/phpunit

> vendor/bin/phpunit

> php bin/phpunit --coverage-test

> bin/console make:user

> bin/console security:check 

> bin/console make:voter

> bin/console make:entity --regenerate


# Swiftmailer

> composer require symfony/swiftmailer-bundle

> bin/console swiftmailer:spool:send --message-limit=10 --env=prod


# Heroku Deployment

## composer.json file update

```
To the composer.json file add the following entry at the "require" section:

"ext-sqlite3": "*", 
"ext-intl" : "*",
"ext-pdo_sqlite" : "*"

then run 

> composer update command.

Inside the Utils folder create cache folder, and copy data.db file from the var directory (this can be empty database).

Change the HerokuCache.php/FilesCache.php file from the Utils folder to look like the file from the downloadable resource from this lecture.
```

## .htaccess for https

```
    # RewriteCond %{HTTP:X-Forwarded-Proto} !https
    # RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```


## .gitignore to get database over heroku

```
/var/cache/
/var/log/
/var/sessions/
/var/spool/
```

> git rm -r --cached .

> git add .

> git commit -m "change"


## Installation and Deployment

> heroku login

> heroku create 

> heroku config:set APP_ENV=heroku

> heroku config:set APP_DEBUG=0

> heroku config:set APP_SECRET=xxxxxxxxxxxxxx

> heroku config:set DATABASE_URL=xxxxxxxxxxxxx

> echo 'web: $(composer config bin-dir)/heroku-php-apache2 public/' > Procfile

> git add .

> git commit -m "change"

> git push heroku master

> heroku open


# Git Repo Setup 

```
git init
git add .
git commit -m "project init"
git remote add origin https://github.com/primarypartition/app2115.git
git push -u origin master
```
