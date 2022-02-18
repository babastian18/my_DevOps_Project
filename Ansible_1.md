## Task in this session
1. setup ansible server
2. integrate docker with ansible
 
### Create Ansible Server Instance
- Amazon Linux 2 AMI
- t2.micro
- add tags key `name`, value `Ansible_server`
- `Select an exsiting security group` , `DevOps_Project_Security_Group`
- checklist `I acknowledge ...` , `DevOps_Project_key RSA`
- Launch

### Connect to Ansible
- copy public ipv4 address to MobaXterm
- specify username `ec2-user`

### Ansible server
```
sudo su -
vi /etc/hostname
```
- `ansible-server`
- `init 6`
```
sudo su -
useradd ansadmin
passwd ansadmin
```
- example pas `123`
```
visudo
```
#### Entering vim
- scroll down and find `## Same thing without a password`
- under the `# &wheel` add
```
ansadmin ALL=(ALL)    NOPASSWD: ALL
```
#### Exit vim
```
vi /etc/ssh/sshd_config
```
#### Entering vim again
- scroll down and find `# To disable tunneled clear text password..`
- hapus `#` yang ada di `#PasswordAuthentication yes` (1)
- tambahkan `#` yang ada di `PasswordAuthentication no` (3)
#### Exit vim
```
service sshd reload
```
- open new ssh tab with ipv4 address ansible server but without add private key or just connect in already
- login as `ansadmin` , password `123`
#### ansadmin user
```
ssh-keygen
```
- just enter it
```
sudo su - 
yum install ansible
```
- see to `To user, run` part
- copy the command without `sudo` , just `amazon-linux-extras ... `
```
python --version
ansible --version
```
#### Open new Docker ssh session 
- change the background terminal to make it better 
- after connected
```
sudo su -
useradd ansadmin
passwd ansadmin
```
- example, `123`
```
visudo
```
#### Entering vim
- scroll and find `## Same thing without a password`
- dibawahnya `# %wheel` tambahkan
```
ansadmin ALL=(ALL)    NOPASSWD: ALL
```
- exit
### Exit vimm
```
grep Password /etc/ssh/sshd_config
```

### Back to Ansible root server, not `ansadmin server`
```
vi /etc/ansible/hosts
```
- delete all the information

### Back to Docker server
```
ifconfig
```
- copy the private ip address
- `inet` yang atas, copy the number

### Back to ansible tab session
- paste the ip insde the last vim
- save
### exit vim
```
sudo su - ansadmin
```
```
ll
ll  -la
cd .ssh
ll
```
```
cd ..
ssh-copy-id <inet atas docker host>
```
### Back to docker server session
```
sudo su - ansadmin
ll  
ll -la
cd .ssh
ll
cat authorized_key
```
### back to ansible session
```
cat .ssh/id_rsa_pub
```
- copy all the key and paste to notepad
```
cd ..
ansible all -m ping
```
```
ansible all -m command -a uptime
```

