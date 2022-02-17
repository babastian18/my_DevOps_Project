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





