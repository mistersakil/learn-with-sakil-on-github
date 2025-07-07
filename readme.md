## Learning Linux

#### Linux CLI Tools

- Check Linux OS Name and Version `hostnamectl`

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

#### Deploy a web service on docker container

Pull Nginx : `docker pull nginx`

Run web server:

```dockerRun
docker run --name web -d -p 80:80 nginx
OR
docker run --name web -d -p 8001:8111 nginx
```