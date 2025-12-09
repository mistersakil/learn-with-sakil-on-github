## Learning Docker

#### Linux basic cli commands

---

- Check Linux OS Name and Version `hostnamectl`
- Show directory listing `ls -l`
- Back to the root directory `cd /` or `cd ~`
- Copy Everything Inside a Folder `cp -r /copy/from/* /destination/path/`
- Copy Everything Inside a Folder including (.filename) files `cp -r /copy/from/. /destination/path/`
- Set os the static hostname `hostnamectl set-hostname your-new-hostname`
- Install a basic editor inside the container or linux distribution (if needed) `apt update && apt install nano -y`

#### Standalone docker Installation and configuration on linux

---

> Insure you already set host OS static hostname for further process - `hostnamectl set-hostname yourNewHostname`

```dockerInstall
# yum update -y
# yum install yum-utils device-mapper-persistent-data lvm2 wget telnet vim -y
# cd /etc/yum.repos.d/
# wget https://download.docker.com/linux/centos/docker-ce.repo
# yum install docker-ce docker-ce-cli containerd.io
# systemctl start docker
# systemctl status docker
# systemctl enable docker
# systemctl is-enabled docker
```

#### Running nginx web server on docker container

---

**Pull nginx image:** `docker pull nginx`

**Run web server:**

```dockerRun
docker run --name web1 -it -d -p 80:80 nginx (host port 8080 : nginx port 80)
OR
docker run --name web1 -d -p 80:80 nginx (host port 8080 : nginx port 80)
OR
docker run --name web2 -d -p 8001:80 nginx (host port 8001 : nginx port 80)
OR
docker run --name web3 -d -p 8111:8111 nginx (host port 8001 : nginx port 8111)
```

**Run individual container:** `docker start yourContainerName`

**Stop individual container:** `docker stop yourContainerName`

**Remove All Containers:** `docker rm -f $(docker ps -aq)`

**List of available container:** `docker ps -a`

**List of active container:** `docker ps`

**Remove individual container:** `docker rm yourContainerName`

**Remove all stopped container:** `docker container prune`

**Container CLI/Bash (sync data):** `docker exec -it containerName bash`

**List of available images:** `docker images`

**Delete individual image:** `docker rmi imageName`

**Delete all image:** `docker system prune`

**Default Nginx config file:** `etc/nginx/conf.d/default.conf`

#### Running mysql server server on docker container

---

**Pull mysql image:** `docker pull mysql/mysql-server:latest`

**Run mysql server:**

```mysqlServer
docker run --name mysqlServer -e MYSQL_ROOT_PASSWORD=12345678# -d mysql/mysql-server:latest
OR with PORT
docker run --name mysqlServer -e MYSQL_ROOT_PASSWORD=12345678# -p 3306:3306 -d mysql/mysql-server:latest

```

**Clear the MySQL CLI screen:** `Ctrl + L`

#### Very basic php application with docker

---

**See sample project structure**

```projectStructure
my-docker-project/
│
├── docker-compose.yml
├── nginx/
│   └── default.conf
├── Dockerfile
├── src/
│   └── index.php
```

**docker-compose.yml**

```dockerCompose.yml
services:
  app:
    build:
      context: .
    container_name: saas_crm_ihelp
    restart: always
    depends_on:
      - db
    networks:
      - saas_crm_ihelp_network

  webserver:
    image: nginx:latest
    container_name: saas_crm_ihelp_webserver
    restart: always
    ports:
      - "80:80"
    volumes:
      - .:/var/www
      - ./nginx/conf.d:/etc/nginx/conf.d
    depends_on:
      - app
    networks:
      - saas_crm_ihelp_network

  db:
    image: mysql:5.7
    container_name: saas_crm_ihelp_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
      MYSQL_DATABASE: ${DB_DATABASE}
    ports:
      - "3306:3306"
    volumes:
      - saas_crm_ihelp_db_data:/var/lib/mysql
    networks:
      - saas_crm_ihelp_network

  phpmyadmin:
    image: phpmyadmin
    container_name: saas_crm_ihelp_phpmyadmin
    restart: always
    environment:
      PMA_HOST: db
      UPLOAD_LIMIT: 256M
    ports:
      - "8001:80"
    depends_on:
      - db
    networks:
      - saas_crm_ihelp_network

  node:
    image: node:22
    container_name: node
    volumes:
      - ./src:/usr/src/app
    working_dir: /usr/src/app
    tty: true
    command: sh
    networks:
      - app-network

networks:
  saas_crm_ihelp_network:
    driver: bridge

volumes:
  saas_crm_ihelp_db_data:

```

**Using .env file for secrets inside docker-compose.yml file**

```usingDotEnv
.env

MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=php_docker_test_db
MYSQL_USER=sakil
MYSQL_PASSWORD=12345678

```

**nginx/default.conf**

```nginxConfig
server {
    listen 80;
    server_name localhost;

    root /var/www/htdocs;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

**Dockerfile for laravel application**

```dockerFileConfig
FROM php:8.2-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg-dev \
    libonig-dev \
    libxml2-dev \
    zip unzip curl git \
    libzip-dev \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Copy Laravel source code
COPY . /var/www

# Ensure necessary directories exist
RUN mkdir -p /var/www/storage/logs /var/www/bootstrap/cache

# Fix ownership and permissions
RUN chown -R www-data:www-data /var/www \
    && chmod -R ug+rwx /var/www/storage /var/www/bootstrap/cache

# Set Git safe directory to avoid dubious ownership error
RUN git config --global --add safe.directory /var/www

# Switch to www-data to install dependencies
USER www-data
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Switch back to root for runtime
USER root

# Final command: Clear caches, migrate, start PHP-FPM
CMD php artisan config:clear && \
    php artisan cache:clear && \
    php artisan route:clear && \
    php artisan migrate --force && \
    php-fpm

```

**Container up and down**

```buildContainer
docker compose up -d

docker compose up -d --build

docker compose down

docker compose down --volumes (flush volumes)
```

**Fix Laravel directory permissions at runtime**

```setLaravelStoragePermission
chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache
chmod -R 775 /var/www/storage /var/www/bootstrap/cache
```

### Insufficient memory limit detected
Insufficient memory limit detected! Current: 128M, Recommended: 512M
To fix this, run either command with increased memory:
php -d memory_limit=512M artisan world:install
php -d memory_limit=512M artisan db:seed --class=WorldSeeder