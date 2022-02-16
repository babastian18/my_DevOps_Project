### Create Tomcat instance on AWS
- AMI = Amazon Linux 2 (HVM)
- t2.micro
- Add Tags
- Key `Name`, Value `Tomcat_Server`
- New Security Group `DevOps_Security_Group`
- ADD Rule `Custom TCP` , Port Range `8080` ..
- Checklist `I Acknowledge.. Choose exsiting key`

| Launch |
|--------|

### Connect to Mobaxterm with Ipv4 Address
```
amazon-linux-extras install java-openjdk11
```
```
java -version
cd /opt
ll
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.16/bin/apache-tomcat-10.0.16.tar.gz
ll
```
```
tar -xvzf <apache tomcat version .tar.gz>
```
```
ll
mv <apache version without .tar.gz> tomcat
ll
```
```
cd tomcat/
ll
cd bin
ll
./startup.sh
cd ../..
pwd
ll
cd tomcat/
find / -name context.xml
```
- copy yang urutan ketiga(host manager)
- copy paling bawah(manager)
```
vi <paste>
```
- di sebelah kiri <valve class name dan di sebelah kanan angka angka dibawahnya 0101001 setelah > tambahkan
- yg dikiri `<!--` , yg dikanan `-->`
- exit vim
```
vi <paste2> 
```
- sama dengan yang paste1
- exit vim
```
ll
cd bin
ll
./shutdown.sh
./startup.sh 
```
# Copy Ipv4 address tomcat server and open it
`<ipv4 address>:8080`
- host manager
```
cd ..
ll
cd conf/
ll
vi tomcat-users.xml
```
### entering vim
- role paling bawah hapus semua tukar dengan
- `<!--` hapus, panah `-->` yang dibawah juga hapus
 ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
   ```
- semua role rapetin ke kiri mentok
- exit vim
```
ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
```
```
echo $PATH
```
```
ln -s /opt/apache/bin/shutdown.sh /usr/local/bin/tomcatdown
tomcatdown
tomcatup
```

### open Tomcat web
-  go to manager app
-  username `tomcat` , password `s3cret`
-  sign in

