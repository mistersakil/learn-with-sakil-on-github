## Learning Linux

#### Linux basic cli commands

---

- Check Linux OS Name and Version `hostnamectl`
- Back to the root directory `cd /` or `cd ~`
- Set os the static hostname `hostnamectl set-hostname your-new-hostname`
- Install a basic editor inside the container or linux distribution (if needed) `apt update && apt install nano -y`

## Learning Docker

#### Standalone docker Installationiand configuration on linux

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

```dockerCompose
version: '3.8'

services:
  app:
    build:./
    container_name: php-test-app-container
    volumes:
      - ./src:/var/www/htdocs
    depends_on:
      - db
    networks:
      - app-network

  webserver:
    image: nginx:latest
    container_name: nginx-container
    ports:
      - "80:80"
    volumes:
      - ./src:/var/www/htdocs
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app
    networks:
      - app-network

  db:
    image: mysql/mysql-server:latest
    container_name: mysql-container
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: php_docker_test_db
      MYSQL_USER: user
      MYSQL_PASSWORD: "12345678"
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - app-network

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin-container
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8111:80"
    depends_on:
      - db
    networks:
      - app-network

  node:
    image: node:22
    container_name: node
    volumes:
      - ./src:/usr/src/app
    working_dir: /usr/src/app
    tty: true
    command: sh -c "while true; do sleep 1000; done"
    networks:
      - app-network

volumes:
  db_data:

networks:
  app-network:
    driver: bridge

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

**Dockerfile**

```dockerFileConfig
FROM php:8.2-fpm

# Install PHP extensions
RUN apt-get update && apt-get install -y \
    libzip-dev zip unzip curl \
    && docker-php-ext-install pdo pdo_mysql zip

# Optional: install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/htdocs
```

**Container up and down**

```buildContainer
docker compose up -d

docker compose up -d --build

docker compose down

docker-compose down --volumes (flush volumes)
```
