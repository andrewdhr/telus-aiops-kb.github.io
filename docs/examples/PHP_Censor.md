---
layout: default
title: PHP_Censor
parent: examples
---

# PHP_Censor Utilities
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
<p align="center">
    <img width="500" height="auto" src="docs/images/php-censor-black.png" alt="PHP Censor" />
</p>
   
   
**PHP Censor** is an open source, self-hosted, continuous integration server for PHP projects 
([PHPCI](https://www.phptesting.org) fork). [Official twitter @php_censor](https://twitter.com/php_censor).

PHP Censor versions:

| Version              | Latest   | Branch        | Status                                                                | Minimal PHP Version |
| :------------------: | :------: | :-----------: | :-------------------------------------------------------------------: | :-----------------: |
| `1.0` (Morty Smith)  | `1.0.16` | `release-1.0` | Old version (**UNSUPPORTED**)                                         | `>=5.6, <8.0`       |
| `1.1` (Birdperson)   | `1.1.6`  | `release-1.1` | Old version (**UNSUPPORTED**)                                         | `>=5.6, <8.0`       |
| `1.2` (Summer Smith) | `1.2.4`  | `release-1.2` | Old version (**UNSUPPORTED**)                                         | `>=5.6, <8.0`       |
| `1.3` (Jerry Smith)  | `1.3.5`  | `release-1.3` | Old stable version (**ONLY FIXES**)                                   | `>=5.6, <8.0`       |
| `2.0` (Rick Sanchez) | `2.0.5`  | `release-2.0` | Current stable version ([Upgrade from v1 to v2](docs/UPGRADE_2.0.md)) | `>=7.4`             |
| `2.1`                | WIP      | `master`      | Feature minor version (WIP)                                           | `>=7.4`             |

More [screenshots](docs/en/screenshots.md).

* [System requirements](#system-requirements)
* [Features](#features)
* [Changelog](#changelog)
* [Roadmap](#roadmap)
* [Installing](#installing)
* [Installing via Docker](#installing-via-docker)
* [Updating](#updating)
* [Configuring project](#configuring-project)
* [Migrations](#migrations)
* [Tests](#tests)
* [Documentation](#documentation)
* [License](#license)

## System requirements

* Unix-like OS (**Windows isn't supported**);

* PHP 7.4+ (with OpenSSL support and enabled functions: `exec()`, `shell_exec()` and `proc_open()`);

* Web-server (Nginx or Apache2);

* Database (MySQL/MariaDB or PostgreSQL);

* Beanstalkd queue;

## Features

* Clone project from [GitHub](docs/en/sources/github.md), [Bitbucket](docs/en/sources/bitbucket.md) (Git/Hg), 
[GitLab](docs/en/sources/gitlab.md), [Git](docs/en/sources/git.md), Hg (Mercurial), SVN (Subversion) or from local 
directory;

* Set up and tear down database tests for [PostgreSQL](docs/en/plugins/pgsql.md), [MySQL](docs/en/plugins/mysql.md) or 
[SQLite](docs/en/plugins/sqlite.md);

* Install [Composer](docs/en/plugins/composer.md) dependencies;

* Run tests for PHPUnit, Atoum, Behat, Codeception and PHPSpec;

* Check code via Lint, PHPParallelLint, Pdepend, PHPCodeSniffer, PHPCpd, PHPCsFixer, PHPDocblockChecker, PHPLoc, 
PHPMessDetector, PHPTalLint and TechnicalDebt;

* Run through any combination of the other [supported plugins](docs/en/README.md#plugins), including Campfire, 
CleanBuild, CopyBuild, Deployer, Env, Git, Grunt, Gulp, PackageBuild, Phar, Phing, Shell and Wipe;

* Send notifications to Email, XMPP, Slack, IRC, Flowdock, HipChat and 
[Telegram](https://github.com/LEXASOFT/PHP-Censor-Telegram-Plugin);

* Use your LDAP-server for authentication;

## Changelog

[Versions changelog](CHANGELOG.md).

## Roadmap

See [milestones](https://github.com/php-censor/php-censor/milestones).

## Installing

* Go to the directory in which you want to install PHP Censor, for example: `/var/www`:

```bash
cd /var/www
```

* Create project by Composer:

```bash
composer create-project \
    php-censor/php-censor \
    php-censor.local \
    --keep-vcs
```

Or download [latest archive](https://github.com/php-censor/php-censor/releases/latest) from GitHub, unzip it and run 
`composer install`.

* Create an empty database for your application (MySQL/MariaDB or PostgreSQL);

* Install Beanstalkd Queue (Optional, if you are going to use a queue with Worker):

```bash
# For Debian-based
aptitude install beanstalkd
# Check if the service is running:
/etc/init.d/beanstalkd status
# If it's not running, start it:
/etc/init.d/beanstalkd start
```

* Install PHP Censor itself:

```bash
cd ./php-censor.local

# Interactive installation
./bin/console php-censor:install

# Non-interactive installation
./bin/console php-censor:install \
    --url='http://php-censor.local' \
    --db-type=pgsql \
    --db-host=localhost \
    --db-pgsql-sslmode=prefer \
    --db-name=php-censor \
    --db-user=php-censor \
    --db-password=php-censor \
    --db-port=default \ # Value 'default': 5432 for PostgreSQL and 3306 for MySQL
    --admin-name=admin \
    --admin-password=admin \
    --admin-email='admin@php-censor.local' \
    --queue-host=localhost \
    --queue-port=11300 \
    --queue-name=php-censor

# Non-interactive installation with prepared config.yml file
./bin/console php-censor:install \
    --config-from-file=yes \
    --admin-name=admin \
    --admin-password=admin \
    --admin-email='admin@php-censor.local'
```

* [Add a virtual host to your web server](docs/en/virtual_host.md), pointing to the `public` directory within your new
PHP Censor directory. You'll need to set up rewrite rules to point all non-existent requests to PHP Censor;

* [Set up the PHP Censor Worker](docs/en/workers/worker.md);

## Installing via Docker

If you want to install PHP Censor as a Docker container, you can use 
[php-censor/docker-php-censor](https://github.com/php-censor/docker-php-censor) project.

## Updating

* Go to your PHP Censor directory (to `/var/www/php-censor.local` for example):

    ```bash
    cd /var/www/php-censor.local
    ```

* Pull the latest code from the repository by Git (If you want the latest `master` branch):

    ```bash
    git checkout master
    git pull -r
    ```

    Or pull the latest version:

    ```bash
    git fetch
    git checkout <version>
    ```

* Update the Composer dependencies: `composer install`

* Update the database scheme:

    ```bash
    ./bin/console php-censor-migrations:migrate
    ```

* Restart Supervisord workers (If you use workers and Supervisord):

    ```bash
    sudo supervisorctl status
    sudo supervisorctl restart <worker:worker_00>
    ...
    sudo supervisorctl restart <worker:worker_nn>
    ```
    
    Or restart Systemd workers (If you use workers and Systemd):
    
    ```bash
    sudo systemctl restart <worker@1.service>
    ...
    sudo systemctl restart <worker@n.service>
    ```

## Configuring project

There are several ways to set up the project:

* Add project without any project config (Runs "zero-config" plugins, including: Composer, TechnicalDebt, PHPLoc, 
PHPCpd, PHPCodeSniffer, PHPMessDetector, PHPDocblockChecker, PHPParallelLint, PHPUnit and Codeception);

* Similar to [Travis CI](https://travis-ci.org), to support PHP Censor in your project, you simply need to add a 
`.php-censor.yml` file to the root of your repository;

* Add project config in PHP Censor project page (And it will cancel file config from project repository);

The project config should look something like this:

```yml
setup:
  composer:
    action:    "install"
    directory: "."
test:
  php_unit:
    config: "phpunit.xml"
  php_mess_detector:
    allow_failures: true
  php_code_sniffer:
    standard: "PSR2"
  php_cpd:
    allow_failures: true
complete:
  email_notify:
    default_mailto_address: admin@php-censor.local
```

More details about [configuring project](docs/en/configuring_project.md).

## Migrations

Run to apply latest migrations:

```bash
cd /path/to/php-censor
./bin/console php-censor-migrations:migrate
```

Run to create a new migration:

```bash
cd /path/to/php-censor
./bin/console php-censor-migrations:create NewMigrationName
```

## Tests

```bash
cd /path/to/php-censor

./vendor/bin/phpunit --configuration ./phpunit.xml.dist --coverage-html ./tests/runtime/coverage -vvv --colors=always
```

For Phar plugin tests set 'phar.readonly' setting to Off (0) in `php.ini` config. Otherwise the tests will be skipped.  

For database tests create an empty 'test_db' database on 'localhost' with user/password: `root/<empty>` 
for MySQL and with user/password: `postgres/<empty>` for PostgreSQL (You can change default test user, password and 
database name in `phpunit.xml[.dist]` config constants). If connection failed the tests will be skipped.

## Documentation

[Full PHP Censor documentation](docs/en/README.md).

## License

PHP Censor is open source software licensed under the [BSD-2-Clause license](LICENSE).
