
# Docker Compose and WordPress

## Setup

### Requirements

+ [Docker](https://www.docker.com/get-started)
+ Openssl for creatng the SSL cert. Install using Homebrew `brew install openssl`

### Setup environment variables

Easily set your own local domain, db settings and more. Start by creating `.env` files, like the examples below.

#### For Docker and the cli scripts

Copy `.env-example` in the project root to `.env` and edit your preferences.

Example:

```dotenv
IP=127.0.0.1
APP_NAME=myapp
DOMAIN="myapp.local"
DB_HOST=mysql
DB_NAME=myapp
DB_ROOT_PASSWORD=password
DB_TABLE_PREFIX=wp_

```

#### For WordPress

Copy `.env-example` in the `src` folder to `.env` and edit your preferences.

Use the following database settings:

```dotenv
DB_HOST=mysql
DB_NAME=myapp
DB_USER=root
DB_PASSWORD=password
```

### Create SSL cert

#### macOS and Linux

```shell
cd cli
./create-cert.sh
```

> Note: OpenSSL needs to be installed.

### Windows

Use the bat file in in `./cli/windows scripts/create-cert.bat`

### Trust the cert

#### Add to macOS Keychain

Chrome and Safari will trust the certs using this script.

> In Firefox: Select Advanced, Select the Encryption tab, Click View Certificates. Navigate to where you stored the certificate and click Open, Click Import.

```shell
cd cli
./trust-cert.sh
```

#### Windows

Follow the instructions in  `./cli/windows scripts/trust-cert.txt`

### Add the local domain in /etc/hosts

To be able to use for example `https://myapp.local` in our browser, we need to modify the `/etc/hosts` file on our local machine to point the custom domain name. The `/etc/hosts` file contains a mapping of IP addresses to URLs.

#### macOS and Linux

```shell
cd cli
./setup-hosts-file.sh
```

> The helper script can both add or remove a entry from /etc/hosts. First enter the domain name, then press "a" for add, or "r" to remove. Follow the instructions on the screen.

#### Windows

Follow the instructions in  `./cli/windows scripts/setup-hosts-file.txt`

## Install WordPress and Composer packages (plugins/themes)

```shell
docker-compose run composer install
```
> If you have Composer installed on your computer you can also use `cd src && composer install`

### Update WordPress Core and Composer packages (plugins/themes)

```shell
docker-compose run composer update
```

## Run

```shell
docker-compose up -d
```

Docker Compose will start all the services for you:

```shell
Starting myapp-mysql    ... done
Starting myapp-composer ... done
Starting myapp-phpmyadmin ... done
Starting myapp-wordpress  ... done
Starting myapp-nginx      ... done
```

🚀 Open [https://myapp.local](https://myapp.local) in your browser

## PhpMyAdmin

PhpMyAdmin comes installed as a service in docker-compose.

🚀 Open [http://127.0.0.1:8080/](http://127.0.0.1:8080/)  in your browser

## Notes:

When making changes to the Dockerfile, use:

```bash
docker-compose up -d --force-recreate --build
```

## Tools

#### wp-cli

```shell
docker exec -it myapp-wordpress bash
```

Login to the container

```shell
wp search-replace https://olddomain.com https://newdomain.com --allow-root
```

Run a wp-cli command like this

> You can use this command first after you've installed WordPress using Composer as the example above.

***

### Useful Docker Commands

Login to the docker container

```shell
docker exec -it myapp-wordpress bash
```

Stop

```shell
docker-compose stop
```

Down (stop and remove)

```shell
docker-compose down
```

Cleanup

```shell
docker-compose rm -v
```

Recreate

```shell
docker-compose up -d --force-recreate
```

Rebuild docker container when Dockerfile has changed

```shell
docker-compose up -d --force-recreate --build
```
