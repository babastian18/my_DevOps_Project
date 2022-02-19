## Task in this session
1. integrate ansible with jenkins
2. build an image and create container on ansible
3. using ansible to create containers
4. jenkins job to build an image onto ansible
5. create container on dockerhost using ansible playbook
6. jenkins CI/CD to deploy on container using ansible

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

### back to ansadmin ansible server
```
cd /opt
ll
cd docker
ll
```
```
ifconfig
```
- copy the ipaddress `inet` yang ke 2 (eth0)
```
sudo vi /etc/ansible/hosts
```
- add new ip address under the befored ip addr
```
[dockerhost]
<ip befored>

[ansible]
<ip>
```
```
cat /etc/ansible/hosts
```
```
ansible all -a uptime
```
```
ifconfig
```
- copy the `inet` (eth0)
```
ssh-copy-id <paste>
```
```
ssh-copy-id localhost
```
```
ansible all -a uptime
```
> should be connected now
```
pwd
vi regapp.yml
```
#### enterin vim
```
---
- hosts: ansible

  tasks:
  - name: create docker image
    command: docker build -t regapp:latest .
    args: 
     chdir: /opt/docker
```
- save it
#### exit vim
```
ansible-playbook regapp.yml --check
```
``` 
docker images
ansible-playbook regapp.yml
```
```
docker images
```
- should be created new one with `latest` version
```
docker login
```
#### login to dockerhub account
1. dockerID `21052002`, password `<usually>` 

```
docker images
docker tag <image id regapp latest> 21052002/regapp:latest
docker images
```
- push to dockerhub
```
docker push <21052002>/regapp:latest
```
- refresh the dockerhub web login account
- and see the new images that we have been pushed
```
vi regapp.yml
```
#### entering vim
```
- hosts: ansible

  tasks:
  - name: create docker image
    command: docker build -t regapp:latest .
    args: 
     chdir: /opt/docker
     
  - name: create tag to push image onto dockerhub
    command: docker tag regapp:latest 21052002/regapp:latest
    
  - name: push docker image
    command: docker push 21052002/regapp:latest
```
#### exit vim
```
ansible-playbook regapp.yml --check
```
> should be `skipping` all
```
cat regapp.yml
cat /etc/ansible/hosts
```
> should be 2 user ip `dockerhost`,and `ansible`. and copy the `ansible` ipv4 addr
```
ansible-playbook regapp.yml --limit localhost <paste here>
```
```
docker images
```
- check and refresh the dockerhub web

### Go to jenkins web
- go to `configure` inside the `Copy_Artifacts_onto_Ansible` job
- scroll down to `exec command`
```
ansible-playbook /opt/docker/regapp.yml
```
- enable `Poll Scm` too with schedule `*****` or every minutes
- apply & save

### Open gitbash
- make sure ur inside the `hello-world` directory projects
```
cd webapp/src/main/webapp/
ll
vi index.jsp
```
- change anything u want from the code
```
git add .
git commit -m "updated 3"
git status
git push origin master
```
### open jenkins web and docker hub 
- see the automate
- refresh the dockerhub pages

### go back to ansadmin ansible server
``` 
vi deploy_regapp.yml
```
#### entering vim
```
---
- hosts: dockerhost
  tasks:
  - name: create container
    command: docker run -d --name regapp-server -p 8082:8080 21052002/regapp:latest
```
#### exit vim
```
ansible-playbook deploy_regapp.yml --check
```
> should be `skipping`

### Login to docker host server
- specify uname `ec2-user`
```
sudo su -
docker images
service docker start
service docker status
docker images
docker ps -a
```
- remove the `exited` status container called `regapp:v1`
```
docker rm -f <container id>
```
```
docker image prune
docker rmi regapp:v1 tomcat
chmod 777 /var/run/docker.sock
```

### back to ansadmin ansible server
```
ansible-playbook deploy_regapp.yml
```

### back to docker host server
- check the latest container has been created from ansible
```
docker images
docker ps -a
```
### open the web docker host
- ipv4 addr + `:8082/webapp/`
> should be changed

### open ansadmin ansible server
```
cat deploy_regapp.yml
vi deploy_regapp.yml
```
```
---
- hosts: dockerhost
  tasks:
  - name: stop existing container
    command: docker stop regapp-server
    ignore_errors: yes
    
  - name: remove the container
    command: docker rm regapp-server
    ignore_errors: yes
    
  - name: remove image
    command: docker rmi 21052002/regapp:latest
    ignore_errors: yes
    
  - name: create container
    command: docker run -d --name regapp-server -p 8082:8080 21052002/regapp:latest
```
```
ansible-playbook deploy_regapp.yml --check
```
> if sucess
```
ansible-playbook deploy_regapp.yml
```
    
### Check to dockerhost server
```
docker images
docker ps -a
```
