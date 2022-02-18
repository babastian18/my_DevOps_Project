## Task in this session
1. inegrate ansible with jenkins
2.

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
