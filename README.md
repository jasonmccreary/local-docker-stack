# Local LAMP stack
A repository containing the Docker files to build the LAMP stack which I use for local development.

This is based on a [workshop given by Dana Luther](https://github.com/DanaLuther/HOD-Lemp-or-Lamp-stack). If you are interested in other setups or customizations, I defer to her. She is the master of all Docker things.


## Install Docker Desktop
https://www.docker.com/products/docker-desktop


## Images you will need to download:
- php:7.4-apache
- mysql:latest
- composer:latest
- phpmyadmin/phpmyadmin:latest

To download the images, use the `docker image` pull command:

```sh
docker image pull php:7.4-fpm
```


## Rebuild custom PHP image
```sh
cd images/
docker image build --no-cache -f Dockerfile-php-mysql -t jmac/php:7.4-apache-mysql --target=base . --build-arg PHP_TARGET=7.4-apache
```

## Start the stack
```sh
docker stack deploy -c docker-compose.yml dev
```

## Stop the stack
```sh
docker stack rm dev
```

## Create named volumes
```sh
docker volume create workspace --opt type=none --opt device=/Users/jasonmccreary/workspace --opt o=bind
docker volume create data --opt type=none --opt device=/Users/jasonmccreary/data --opt o=bind
```

## Add a generic database user
Since the MySQL `data` directory is configured to persist, this only needs to be run once per initial setup.


```sql
CREATE USER 'dbuser'@'%' IDENTIFIED BY 'dbpass';
GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'%';
```

**Note:** This user has access from any IP to avoid network configuration.
