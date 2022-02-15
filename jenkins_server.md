AMI = Amazon Linux 2
t2.micro

ADD tags = key (Name), value (Jenkins_Server)

new security grup 
name = Jenkins_Security_Group
Desc = Jenkins_Security_Group
ADD Rule
Custom TCP 8080

Create a new key pair
RSA
DevOps_Project_key
Download key pair

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
hostname jenkns-server

yum install git
git --version

open jenkins
------------
manage jenkins --> manage plugins
github install without restart

manage jenkins --> global tool configuration
name (Git) , path (git)
apply --> save







