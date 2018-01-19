# docker-php-nginx-postgres-composer
Docker Compose configuration to run PHP 7.1 with Nginx, PHP-FPM, PostgreSQL 10.1 and Composer.

## Overview

This Docker Compose configuration lets you run easily PHP 7.1 with Nginx, PHP-FPM, PostgreSQL 10.1 and Composer.
It exposes 4 services:

* web (Nginx)
* php (PHP 7.1 with PHP-FPM)
* db (PostgreSQL 10.1)
* composer

The PHP image comes with the most commonly used extensions and is configured with xdebug.
The UUID extension for PostgreSQL has been added.
Nginx default configuration is set up for Symfony 4 (but can be easily changed) and will serve your working directory.
Composer is run at boot time and will automatically install the vendors.

## Install prerequisites

For now the project has been tested on Linux only but should run fine on Docker for Windows and Docker for Mac.

You will need:

* [Docker CE](https://docs.docker.com/engine/installation/)
* [Docker Compose](https://docs.docker.com/compose/install)
* Git (optional)

## How to use it

### Starting Docker Compose

Checkout the repository or download the sources.

Simply run `docker-compose up` and you are done.

Nginx will be available on `localhost:80` and PostgreSQL on `localhost:5432`.

### Using Composer

`docker-compose run composer <cmd>`

Where `cmd` is any of the available composer command.

### Using PostgreSQL

Default connection:

`docker-compose exec db psql -U postgres`

Using .env file default parameters:

`docker-compose exec db psql -U dbuser dbname`

If you want to connect to the DB from another container (from the `php` one for instance), the host will be the service name: `db`.

### Using PHP

You can execute any command on the `php` container as you would do on any docker-compose container:

`docker-compose exec php php -v`

## Change configuration

### Configuring PHP

To change PHP's configuration edit `.docker/conf/php/php.ini`.
Same goes for `.docker/conf/php/xdebug.ini`.

You can add any .ini file in this directory, don't forget to map them by adding a new line in the php's `volume` section of the `docker-compose.yml` file.

### Configuring PostgreSQL

Any .sh or .sql file you add in `./.docker/conf/postgres` will be automatically loaded at boot time.

If you want to change the db name, db user and db password simply edit the `.env` file at the project's root.

If you connect to PostgreSQL from localhost a password is not required however from another container you will have to supply it.

## Adding aliases

To avoid typing over and over again the same commands you can add two useful aliases in your shell's configuration (`.bashrc` or `.zshrc` for instance):

```
alias dcu="docker-compose up"
alias dcr="docker-compose run"
alias dce="docker-compose exec"
```

It then becomes way faster to execute a composer command for instance:

`dcr composer require --dev phpunit/phpunit`
