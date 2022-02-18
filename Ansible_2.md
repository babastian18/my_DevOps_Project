## Task in this session
1. integrate ansible with jenkins
2. build an image and create container on ansible

### Login to jenkins web
1. `manage jenkins` --> `configure system`
2. scroll down to `publish over ssh` section, and find `add` bottom
3. name `ansible-server`

### Back to ansadmin ansible server
```
ifconfig
```
- copy the private ip address, `inet` yang atas
### back to jenkins
4. hostname paste the ip address `<privateipaddress>`
5. username `ansadmin`
6. go to `advanced` mode
7. checklist `use password authentication`
8. passphrase/password `123`
9. apply
10. TEST CONFIGURATION
11. if `success` save it.
12. create new item
13. name `Copy_Artifacts_onto_Ansible`
14. copy from `BuildAndDeployOnContainer`
15. disable `poll scm`
16. in `post build actions` , `ssh server`, `name` change onto `ansible-server`
17. clear the `exec command` scripts.

### Back to ansadmin ansible server
```
cd /opt
ll 
sudo mkdir docker
ll
sudo chown ansadmin:ansadmin docker
ll
```
```
cd /docker
ll
```
- `ll` again after build the jenkins job

### Back to jenkins web
18. apply and save
19. `Build Now`

### back to ansadmin ansible server
```
clear
sudo yum install docker -y
cat /etc/group
```
```
sudo usermod -aG docker ansadmin
id ansadmin
```
```
service docker status
sudo service docker start
service docker status
```
```
clear
ll
```

### Back to ansadmin dockerhost server
```
sudo su -
ll
cd /opt/docker/
ll
cat Dockerfile
```
- copy the scripts

### back to ansadmin ansible server
```
vi Dockerfile
```
- paste the scripts
``` 
sudo chmod 777 /var/run/docker.sock
docker build -t regapp:v1 .
```
``` 
docker images
docker run -t --name regapp-server -p 8081:8080 regapp:v1
```
### go to aws server
1. copy ansible server public ipv4 address 
2. new tab and open the ip + `:8081/webapp/`
