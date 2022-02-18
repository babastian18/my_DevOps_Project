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
`ansible-server`
`init 6`
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
