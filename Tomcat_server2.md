### Open jenkins web
- `admin` sebelah logout --> `configure` --> scroll down ke `password`, ganti yang gampang
- restart
- uname `admin` , pass (yang baru dibuat tadi)
- --> manage jenkins --> manage plugins
- available search for "deploy to container"
- install without restart
- manage jenkins --> manage credentials --> jenkins --> global credentials
- add credentials
- username  : `deployer`
- password  : `deployer`
- ID & desc : `tomcat_deployer`
ok

