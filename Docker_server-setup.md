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
- `sudo su`

### Open docker hub
**[Docker Hub](https://hub.docker.com/)**
1. search for tomcat images

### Docker server
```
service docker status
service docker start
docker pull tomcat
```
