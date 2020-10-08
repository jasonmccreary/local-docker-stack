# Local LAMP Stack using Docker
A repository containing the Docker files to build a LAMP stack which I use for local development on macOS.

This is based on a [workshop given by Dana Luther](https://github.com/DanaLuther/HOD-Lemp-or-Lamp-stack). From that I have written a tutorial for [installing Apache, MySQL, and PHP on macOS using Docker](https://jasonmccreary.me/articles/install-apache-php-mysql-macos-docker-local-development/).

If you're just getting started with Docker, I recommend following the tutorial. Otherwise, if you are a welcome to review the notes below.


## Install Docker Desktop
While I use this for development on macOS, you may use this for any platform. To being, install the [Docker Desktop Client](https://www.docker.com/products/docker-desktop).


## Images used:
- php:7.4-apache
- mysql:latest
- composer:2
- phpmyadmin/phpmyadmin:latest

To pre-download the underlying images, use the `docker pull` command:

```sh
docker pull php:7.4-apache
docker pull mysql:latest
docker pull composer:2
```

## Rebuild custom PHP image
```sh
docker image build --no-cache -f images/Dockerfile-php-apache -t lamp .
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

## Granting the `dbuser` access
Since the MySQL `data` directory is configured to persist, this only needs to be run once per initial setup.


```sql
GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'%';
```

**Note:** This user has access from any IP to avoid additional network configuration.
