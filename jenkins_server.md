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

|-------------|
| L A U N C H |
|-------------|

public ipv4 address copy
--MobaXterm--
SSH --> Advanced --> use private key
Specify uname = ec2-user

