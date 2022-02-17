### Open docker server
`sudo su -` make sure as root users
```
cat /etc/passwd
cat /etc/group
```
- create user as `dockeradmin`
```
useradd dockeradmin
passwd dockeradmin
```
- makes an easy passwd `123` example
- add `dockeradmin` user to `docker group`
```
usermod -aG docker dockeradmin
```
- change authentication from EC2 instance
```
vi /etc/ssh/sshd_config
```
#### into vim
1. find scroll `# To disable Tunneled` part
2. delete `#` from `#PasswordAuthentication yes` menu (no1)
3. then add `#` to `PasswordAuthentication no` menu (no3)
#### exit vim
```
service sshd reload
```
> note : dont turn off the connection
### Open duplicate tab session 
- login as `dockeradmin`
- password `123`
- exit it

### Login to Jenkins
1. uname `admin` 
2. password
3. manage jenkins --> manage plugins --> availabe
4. search for `publish over ssh`
5. install without restart
7. manage jenkins --> configure system
8. scroll to `publish over SSH` menu
9. `ssh Server` menu, then `add`
10. name `dockerhost`
11. hostname `<private ipv4 address from docker server instance>`
12. username `dockeradmin`
13. `advanced` menu
14. checklist the `use password authenticaton` box
15. pass phrase/password `123`
16. TEST CONFIGURATION
17. if `success` apply & save
 
- Dashboard --> new item `BuildAndDeployOnContainer`
- copy from `BuildAndDeployJob`
- delete `Post-build Actions` before
- change to `send build artifacts over SSH`
- name `dockerhost`
- source files `webapps/target/*.war`
- remove prefix `webapp/target`
- remote directory `<empty>`
- Apply & Save

### open docker server
```
sudo su - dockeradmin
ll
```
- copy artifact webapp.war from `dockeradmin` to `docker container`
```
exit
```
```
cd /opt
ll
mkdir docker
ll
```
```
chown -R dockeradmin:dockeradmin docker
ll
```
```
ls -ld
cd ..
cd /root
ll
mv Dockerfile /opt/docker/
cd /opt/docker/
ll -l
```
```
chown -R dockeradmin:dockeradmin /opt/docker
ll
```
### Open jenkins job
- `builanddeployandcontainer` --> configure
- scroll down to `Remote directory`
- `//opt//docker`
- apply & save
- `Build Now`

### back to docker server
```
ls
ll
date
```
```
vi Dockerfile
```
#### entering vim
- add command after `run cp -r` with code below
```
COPY ./*.war /usr/local/tomcat/webapps
```
### exit vim
```
docker build -t tomcat:v1 .
```
```
docker images
docker run -d --name tomcatv1 -p 8086:8080 tomcat:v1
```

### Open docker web server 
- with public ipv4 docker server + `:8086` port from latest container + `/webapp/`

### Open jenkins
- configure the `BuildAndDeployOnContainer` job
- scroll to `Exec command` and add the code below
```
cd/opt/docker;
docker build -t regapp:v1 .;
docker run -d --name registerapp -p 8087:8080 regapp:v1
```
- apply & save

### Open docker server
- make sure as `root@dockerhost docker`
```
docker images
docker ps -a
docker stop <container ID tomcatv1 +demotomcat +tomcat:latest>
docker ps -a
```
- delete all stopped container
``` 
docker container prune
```
`docker ps -a`
- should be empty
- `docker image prune -a`
- `docker images`
- should be empty
- `docker ps -a`
- should be empy too
