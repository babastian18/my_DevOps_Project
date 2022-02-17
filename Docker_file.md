### Open docker server
```
sudo su -
pwd
ll
vi Dockerfile
```
- entering vim add code below
``` 
FROM centos:latest
RUN yum install java -y
RUN mkdir /opt/tomcat
WORKDIR /opt/tomcat
ADD https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.16/bin/apache-tomcat-10.0.16.tar.gz .
RUN tar -xvzf <apache tomcat .tar.gz>
RUN mv <apache tomcat without .tar.gz only to version>/* /opt/tomcat
EXPOSE 8080
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
```
```
docker build -t mytomcat .
```
```
docker images
docker run -d --name mytomcat-server -p 8083:80080 mytomcat
docker ps -a
```

### Open docker web server
1. copy public ipv4 docker server + new container port `:8083`
2. try
3. stop it `docker stop <container names> mytomcat-server`

### Open docker server
1. `sudo su -` make sure root users root users
```
ls
cat Dockerfile
rm Dockerfile
```
```
ll
vi Dockerfile
```
```
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps 
```


