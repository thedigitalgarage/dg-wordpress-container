# Digital Garage Docker image for [Wordpress](https://github.com/WordPress/WordPress).

This image is designed specifically to support the deployment of WordPress via Docker and Kubernetes via the Digital Garage. This image is a bit more configurable than the [official Wordpress Docker image](https://hub.docker.com/_/wordpress/).


# What is WordPress?

WordPress is a free and open source blogging tool and a content management system (CMS) based on PHP and MySQL, which runs on a web hosting service. Features include a plugin architecture and a template system. WordPress is used by more than 22.0% of the top 10 million websites as of August 2013. WordPress is the most popular blogging system in use on the Web, at more than 60 million websites. The most popular languages used are English, Spanish and Bahasa Indonesia.

```
wikipedia.org/wiki/WordPress
```

![wordress]
[wordress]: https://raw.githubusercontent.com/docker-library/docs/01c12653951b2fe592c1f93a13b4e289ada0e3a1/wordpress/logo.png


# How to use this image

```
$ docker run --name some-wordpress --link some-mysql:mysql -d wordpress
```
The following environment variables are also honored for configuring your WordPress instance:

-e WORDPRESS_DB_HOST=... (defaults to the IP and port of the linked mysql container)
-e WORDPRESS_DB_USER=... (defaults to "root")
-e WORDPRESS_DB_PASSWORD=... (defaults to the value of the MYSQL_ROOT_PASSWORD environment variable from the linked mysql container)
-e WORDPRESS_DB_NAME=... (defaults to "wordpress")
-e WORDPRESS_TABLE_PREFIX=... (defaults to "", only set this when you need to override the default table prefix in wp-config.php)
-e WORDPRESS_AUTH_KEY=..., -e WORDPRESS_SECURE_AUTH_KEY=..., -e WORDPRESS_LOGGED_IN_KEY=..., -e WORDPRESS_NONCE_KEY=..., -e WORDPRESS_AUTH_SALT=..., -e WORDPRESS_SECURE_AUTH_SALT=..., -e WORDPRESS_LOGGED_IN_SALT=..., -e WORDPRESS_NONCE_SALT=... (default to unique random SHA1s)

If the WORDPRESS_DB_NAME specified does not already exist on the given MySQL server, it will be created automatically upon startup of the wordpress container, provided that the WORDPRESS_DB_USER specified has the necessary permissions to create it.

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used:
```
$ docker run --name some-wordpress --link some-mysql:mysql -p 8080:80 -d wordpress
```
Then, access it via http://localhost:8080 or http://host-ip:8080 in a browser.

If you'd like to use an external database instead of a linked mysql container, specify the hostname and port with WORDPRESS_DB_HOST along with the password in WORDPRESS_DB_PASSWORD and the username in WORDPRESS_DB_USER (if it is something other than root):
```
$ docker run --name some-wordpress -e WORDPRESS_DB_HOST=10.1.2.3:3306 \
    -e WORDPRESS_DB_USER=... -e WORDPRESS_DB_PASSWORD=... -d wordpress
```


# ... via docker-compose
Example docker-compose.yml for wordpress:
```
version: '2'

services:

  wordpress:
    image: wordpress
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_PASSWORD: example

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: example
```
Run docker-compose up, wait for it to initialize completely, and visit http://localhost:8080 or http://host-ip:8080.


# Adding additional libraries / extensions

This image does not provide any additional PHP extensions or other libraries, even if they are required by popular plugins. There are an infinite number of possible plugins, and they potentially require any extension PHP supports. Including every PHP extension that exists would dramatically increase the image size.

If you need additional PHP extensions, you'll need to create your own image FROM this one. The [documentation](https://github.com/docker-library/docs/blob/master/php/README.md#how-to-install-more-php-extensions) of the `php` [image](https://github.com/docker-library/docs/blob/master/php/README.md#how-to-install-more-php-extensions) explains how to compile additional extensions. Additionally, the `wordpress` [Dockerfile](https://github.com/docker-library/wordpress/blob/618490d4bdff6c5774b84b717979bfe3d6ba8ad1/apache/Dockerfile#L5-L9) has an example of doing this.

The following Docker Hub features can help with the task of keeping your dependent images up-to-date:

* [Automated Builds](https://docs.docker.com/docker-hub/builds/#repository-links) let Docker Hub automatically build your Dockerfile each time you push changes to it.
* [Repository Links](https://docs.docker.com/docker-hub/builds/#repository-links) can ensure that your image is also rebuilt any time wordpress is updated.


# Supported Docker versions

This image is officially supported on Docker version 1.12.1.

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see the [Docker installation documentation](https://docs.docker.com/installation/) for details on how to upgrade your Docker daemon.