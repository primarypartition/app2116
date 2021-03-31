# Project Setup

> https://symfony.com/

> https://symfony.com/doc/current/setup.html

> composer create-project symfony/skeleton app2116/app1

> composer create-project symfony/skeleton app2116/app2

> composer create-project symfony/skeleton app2116/app3

> composer create-project symfony/skeleton app2116/app4

> php -S 127.0.0.1:8000 -t public

> symfony server:start

> composer dump-autoload

> bin/console cache:clear

> composer create-project symfony/website-skeleton ./ "5.0.*"

``` 
<VirtualHost *:80>   
	ServerName local.app2116.app1
	DocumentRoot "C:\xampp\htdocs\app2116\app1\public" 
</VirtualHost>

<VirtualHost *:80>   
	ServerName local.app2116.app2
	DocumentRoot "C:\xampp\htdocs\app2116\app2\public" 
</VirtualHost>

<VirtualHost *:80>   
	ServerName local.app2116.app3
	DocumentRoot "C:\xampp\htdocs\app2116\app3\public" 
</VirtualHost>

<VirtualHost *:80>   
	ServerName local.app2116.app4
	DocumentRoot "C:\xampp\htdocs\app2116\app4\public" 
</VirtualHost>
```

```
127.0.0.1        local.app2116.app1
127.0.0.1        local.app2116.app2
127.0.0.1        local.app2116.app3
127.0.0.1        local.app2116.app4
```

> https://symfony.com/download

> symfony check:requirements

> composer require symfony/requirements-checker

> http://local.app2116/check.php

> composer remove symfony/requirements-checker

> https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs

> bin/console messenger:consume -vv


# Symfony Packages

## app1

> https://symfony.com/doc/current/components/messenger.html

> composer require symfony/messenger

> .env update

```
MESSENGER_TRANSPORT_DSN=amqp://guest:guest@localhost:5672/%2f/messages
```

> composer require symfony/apache-pack

> composer require --dev symfony/profiler-pack

> composer require doctrine maker

## app2, app3, app4

> composer require symfony/messenger

> .env update

```
MESSENGER_TRANSPORT_DSN=amqp://guest:guest@localhost:5672/%2f/messages
```

# RabbitMQ

## app1

> https://www.rabbitmq.com/download.html

> https://www.rabbitmq.com/install-windows.html

> https://www.rabbitmq.com/which-erlang.html

> https://www.erlang.org/downloads

> cd C:\Program Files\RabbitMQ Server\rabbitmq_server-3.8.14\sbin

> Run cmd as admin: rabbitmq-plugins.bat enable rabbitmq_management

> http://localhost:15672/

> username: guest

> password: guest

> php -i|findstr "Thread"

> https://github.com/php-amqp/php-amqp

> https://pecl.php.net/package/amqp

> https://pecl.php.net/package-info.php?package=amqp&version=1.9.4

> https://pecl.php.net/package/amqp/1.9.4/windows


```
Before download, check if your PHP installation is thread safe or non-thread safe by entering php -i|findstr "Thread" in your terminal

Download thread safe or non-thread safe version of the extension for your PHP version from https://pecl.php.net/package/amqp. Look for the "DLL" link next to each release in the list of available releases

After download, copy the rabbitmq.4.dll and rabbitmq.4.pdb files to the PHP root folder and copy php_amqp.dll and php_amqp.pdb files to PHP\ext folder

Add extension=amqp to the php.ini file or

extension=php_amqp.dll > php.ini and extension_dir = "ext"

Check if the module is properly installed with php -m
```

> .env update

```
MESSENGER_TRANSPORT_DSN=amqp://guest:guest@localhost:5672/%2f/messages
```

> Add exchange

```
name: order.fanout
type: fanout

name: sms.direct
type: direct
```

> Add Queues

```
queues:
    order1.fanout:
    order2.fanout:

bind exchange to queue:
    name: order.fanout
```


```
queues:
sms.service1:
    binding_keys: ['sms1']
sms.service2:
    binding_keys: ['sms2']

bind exchange to queue:
    name: sms.direct
    routing key: sms1 and sms2
```


# Database 

## app1

> .env

```
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
MESSENGER_TRANSPORT_DSN=doctrine://default
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

> bin/console debug:messenger

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

> bin/console messenger:consume -vv


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
git remote add origin https://github.com/primarypartition/app2116.git
git push -u origin master
```
