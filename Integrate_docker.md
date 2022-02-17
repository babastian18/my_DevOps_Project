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
 





