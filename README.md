# docker-wordpress-nginx

This dockerfile exposes the lib directory of mysql and the wp-content directory of wordpress. It is based on: [eugenwares docker-wordpress-nginx](https://github.com/eugeneware/docker-wordpress-nginx) . We stripped out the included MySql to replace it with [sameersbns](https://github.com/sameersbn/docker-mysql) for better configurability. Follow the installation instruction and you'll be able to persit your containers configuration even if the docker-wordpress-nginx image has been removed.

## Installation

```
$ git clone https://github.com/atroo/docker-wordpress-nginx.git
$ cd docker-wordpress-nginx
$ sudo docker build -t docker-wordpress-nginx .
$ docker pull sameersbn/mysql:latest
```

## Usage

First of all we start the mysql container:

```
$ docker run --name mysql -d \
-e 'DB_USER=wordpress' -e 'DB_PASS=yourPwHere' -e 'DB_NAME=wordpress' \
-v /opt/mysqlhostdir:/var/lib/mysql \
sameersbn/mysql:latest
```

Afterwards we configure the wordpress-nginx container to run with the mysql container.

```
$ docker run --name docker-wordpress-nginx -d \
--link mysql:mysql \
-p 80:80 \
-v /opt/yourwwwroothostdir:/usr/share/nginx/www \
-e 'DB_PASSWORD=yourPwHere' -e 'DB_NAME=wordpress' \
docker-wordpress-nginx

```

And your done. Everything should be persisted to your host systems Filesystem and shall be relinkable in case you once removed the docker-wordpress-nginx container.