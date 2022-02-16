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
- Dashboard --> new item `BuildAndDeployJob` and `Maven projects`
- desc `build and deploy job with tomcat server`
- SCM `git` 
- repo url
```
- https://github.com/babastian18/hello-world.git
```
- goals and options `clean install`
- post build actions, choose `Deploy war/ear to a container`

### GO to jenkins server
```
sudo su -
cd /var/lib/jenkins/workspace
ll
cd FirstMavenProject
ll
cd webapp/target/
ll
pwd
```
### back to jenkins web
- war/ear files `webapp/target/webapp.war` change below
- anti style glob ? change to
- `**/*.war`
- add container `tomcat 8x remote` 
- credentials `deployer`
- tomcat url `http://<ipv4addresstomcatserver + :8080/`
- apply save
- build now (file yang akan dibuat ada di git hello world (webapp --> src/main.. --> index.jsp))

### Go to tomcat server
```
cd ..
ll
cd webapps/
ll
```

### GO to tomcat web
- manager app --> choose webapp/ di bagian kiri

### Open gitbash CMD (hanya mengubah isi dari tampilan web yang tadi)
```
cd C:/Users/KEVINONG/Desktop/Bastian/DevOpsProject/hello-world
cd webapp/src/main/webapp/
vi index.jsp
```
### entering vim
- delete all code dan ganti dengan 
```
<form action="action_page.php">
  <div class="container">
    <h1>Register</h1>
    <p>Please fill in this form to create an account.</p>
    <hr>

    <label for="email"><b>Email</b></label>
    <input type="text" placeholder="Enter Email" name="email" id="email" required>

    <label for="psw"><b>Password</b></label>
    <input type="password" placeholder="Enter Password" name="psw" id="psw" required>

    <label for="psw-repeat"><b>Repeat Password</b></label>
    <input type="password" placeholder="Repeat Password" name="psw-repeat" id="psw-repeat" required>
    <hr>

    <p>By creating an account you agree to our <a href="#">Terms & Privacy</a>.</p>
    <button type="submit" class="registerbtn">Register</button>
  </div>

  <div class="container signin">
    <p>Already have an account? <a href="#">Sign in</a>.</p>
  </div>
</form>
```
- exit vim
```
git status
git add . 
git status
git commit -m "ganti isi web ke model form html"
git push origin master
```
-if cannot (video deploy artifacts on tomcat server detik 5.30)

### Back to jenkins
- BuildAndDeployJob --> build now
- restart tomcat web server
