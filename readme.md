## Learning Linux

#### Linux CLI Tools

* Check Linux OS Name and Version `hostnamectl`
* Install a basic editor inside the container or linux distribution (if needed) `apt update && apt install nano -y`

## Learning Docker

#### Standalone Docker Installation and configuration on Linux

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

#### Deploy nginx web server on docker container

**Pull nginx image:**  `docker pull nginx`

**Run web server:**

```dockerRun
docker run --name web1 -d -p 80:80 nginx (host port 8080 : nginx port 80)
OR
docker run --name web2 -d -p 8001:80 nginx (host port 8001 : nginx port 80)
OR
docker run --name web3 -d -p 8111:8111 nginx (host port 8001 : nginx port 8111)
```
**Run individual container:** `docker start yourContainerName`
**Stop individual container:** `docker stop yourContainerName`
**List of available container:** `docker ps -a`
**List of active container:** `docker ps`
**Remove individual container:** `docker rm yourContainerName`
**Container CLI/Bash (sync data):** `docker exec -it containerName bash`

