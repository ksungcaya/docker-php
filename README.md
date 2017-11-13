## Introduction
The base image used in this custom php image is from ```ubuntu:16.04```'s build from [docker hub](https://hub.docker.com/_/ubuntu/).

## Owner and Group
Default configuration for user and group:
```bash
user = www-data
group = www-data
```

## Extensions
The image contains the following pre-installed php extensions as per the version:

* PHP_VERSION-mcrypt
* PHP_VERSION-gd
* PHP_VERSION-mysql
* PHP_VERSION-pgsql
* PHP_VERSION-sqlite3
* PHP_VERSION-imap
* PHP_VERSION-mbstring
* PHP_VERSION-xml
* PHP_VERSION-curl
* PHP_VERSION-bcmath
* PHP_VERSION-mongo
* PHP_VERSION-intl
* php-memcached
* php-memcache
* php-redis
* php-apcu

#### Extra flavors
In addition, helpful packages for php development are pre-installed.

* composer
* curl
* git
* zip
* unzip

## Xdebug
For performance optimization, there's a separate ```Dockerfile.debug``` file for building php image with **xdebug** installed.

## How to use this image
Basic usage can be found on ```php```'s official [docker hub](https://hub.docker.com/_/php/) repository.

#### Port
The image exposes ```port 9000``` for outside access.

Notes:

1. While the image is building, it will run the following command:

    ```bash
    mkdir /root/.ssh && echo "StrictHostKeyChecking no " > /root/.ssh/config
    ```
    This command will allow you to mount your host's ssh keys if your app will use a private repository package.
    You can ignore this if you're not planning to use a private repository, it's safe!


## Tagging

#### PHP
```bash
docker build -t $vendor/docker-php:$version .
```

#### With Xdebug
```bash
docker build -f Dockerfile.debug -t $vendor/docker-php:xdebug-$version .
```

#### Try it out!
```bash
docker run --rm -it $vendor/docker-php:$version php -v
```