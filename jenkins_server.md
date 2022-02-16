### Create Instances On AWS
- AMI      = Amazon Linux 2
- t2.micro
- ADD tags = key (Name), value (Jenkins_Server)
new security grup :
- name     = Jenkins_Security_Group
- Desc     = Jenkins_Security_Group
- ADD Rule = Custom TCP 8080
Create a new key pair
- RSA
- DevOps_Project_key
- Download key pair

| L A U N C H |
|-------------|

public ipv4 address copy
--MobaXterm--
SSH --> Advanced --> use private key
Specify uname = ec2-user

sudo su -
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install epel-release
amazon-linux-extras install epel
y
amazon-linux-extras install java-openjdk11
yum install jenkins
y

service jenkins start

AWS 
----     
jenkins security --> open security grup

open Jenkins
------------
public ipv4 address + /8080

copy link to jenkins server
copy password
select plugins to install

jenkins server
--------------
cat/etc/hostname --> jenkins-server
hostname jenkins-server

yum install git
git --version

open jenkins
------------
manage jenkins --> manage plugins
github install without restart

manage jenkins --> global tool configuration
name (Git) , path (git)
apply --> save

new item (PullCodeFromGithub), freestyle project
desc      : Pull code from github
SCM       : git
repo url  : https://github.com/babastian18/hello-world.git
apply, save
build now
console output. Building in workspace (copy to jenkins server)

jenkins_server
--------------
cd (Building in workspace)
ll

INTEGRATE MAVEN WITH JENKINS
----------------------------
before entering vim
------------------
find / -name jvm
/usr/lib/jvm (copy)
cd /usr/lib/jvm
ll (find java jdk 11 paling atas)
find / -name java-11*
(copy usr lib paling atas)
next
----
sudo su - 
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-3.8.4-bin.tar.gz
ll, find apache-maven-(copy, paste after -xvzf below)
tar -xvzf ..
ll, find apache maven without bin.tar.gz then copy and paste below after mv command
mv ..
ll (find maven)
cd maven
ll
cd bin
ll
./mvn -v
cd ~
pwd
ll -a (find .bash_profile)
vi .bash_profile

(entering vim)
--------------
dibawah fi tambahkan
M2_HOME=/opt/maven
m2=/opt/maven/bin
JAVA_HOME=(paste disini)

path bin tambahkan
:&JAVA_HOME:$M2_HOME:$M2

(exit vim)
--------

echo $PATH
source .bash_profile
echo $PATH
(copy dari usr/lib sampe _64)
mvn -v

Go to Jenkins
-------------
manage jenkins --> manage plugins
Available
"Maven integration"
install without restart
--> back to dashboard
global tool config
JDK MENU = add JDK (turn off install auto)
  name      : java-11
  java_home : (paste disini)

MAVEN MENU = add maven (turn off install auto)
  name        : maven-(version)
  maven_home  : /opt/maven

APPLY SAVE
new item (named : FirstMavenProject (select maven project)
Desc  : first maven project
SCM   : Git (repo url = https://github.com/babastian18/hello-world.git )
Goals and option  : clean install

apply, save, build

Go TO Jenkins_Server
--------------------
sudo su - 
cd /var/lib/jenkins/
ll
cd workspace/
ll
cd FirstMavenProject
ll
cd webapp/
ll
cd target/

Go TO Jenkins
-------------
workspace --> webapp --> target --> webapp.war
Dashboard








