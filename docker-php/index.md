# Docker PHP desde cero

> https://x-team.com/blog/docker-compose-php-environment-from-scratch/

## Instalar Docker

    brew cask install docker

## Crear estructura de directorios

    mkdir -p docker/{web,db,php}
    tree docker public

## Contenido de inicio

    mkdir public
    echo '<?php phpinfo(); ?>' > public/index.php

## Dockerfile para la base de datos

```docker
FROM mariadb:latest

CMD ["mysqld"]

EXPOSE 3306

```

Prueba: `docker build --tag mariadb_phpdev .`


## Dockerfile para nginx

```docker
FROM nginx:alpine

CMD ["nginx"]

EXPOSE 80 443

```

## Dockerfile para php-fpm

```docker
FROM php:fpm-alpine

CMD ["php-fpm"]

EXPOSE 9000
```

