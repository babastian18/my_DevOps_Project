### Create Docker EC2 Intstance
- `Amazon Linux 2 AMI`
- `t2.micro`
- Add Tags, name `Name`, value `Docker-Host`
- `Select an existing Security Group`, `DevOps_Security_Group`
- `Choose existing key pair DevOps_Project_key RSA`

| Launch |
|--------|

### Open docker server from MobaXterm
- login as : `ec2-user`
- install docker
```
sudo su - 
yum install docker -y
```
```
service docker status
service docker start
service docker status
```
```
docker images
docker ps
docker ps -a
docker --help
```
```
vi /etc/hostname
```
- change the name to `dockerhost`
- `init 6` to reboot server
- login as `ec2-user`
- `sudo su -`

### Open docker hub
**[Docker Hub](https://hub.docker.com/)**
1. search for tomcat images

### Docker server
```
service docker status
service docker start
docker pull tomcat
```
```
docker images
docker run -d --name tomcat-container -p 8081:8080 tomcat
```
```
docker ps -a
```

### Access Docker from eksternally 
1. Open AWS Instance
2. Open docker security groups link as a new link tab
3. Edit inbound rules
4. add rule `custom tcp` with port range `8081-9000`, `anywhere`, `0.0.0.0/0`
5. save
6. copy public ipv4 docker server + `:8081`

### Docker server
8. login to docker tomcat container
```
docker exec -it tomcat-container /bin/bash
ls
cd webapps.dist
ls
cp -R * ../webapps/
cd ../webapps
ls
```

### Open docker web server
1. refresh pages

### Open docker server
```
exit
docker ps -a
docker stop tomcat-container 
docker ps -a
docker ps
```
```
docker run -d --name tomcat2 -p 8082:8080 tomcat:latest
docker ps
```

### Open docker web
1. change the port with new container port `:8082`









