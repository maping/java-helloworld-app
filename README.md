# A Java Hello World Application
[![Build status](https://dev.azure.com/maping930883/java-helloworld-app/_apis/build/status/java-helloworld-app-Maven-CI)](https://dev.azure.com/maping930883/java-helloworld-app/_build/latest?definitionId=31)

## 1. Create a repo manually

### 1.1 Create a repo on GitHub
Click "Repositories",then click "New" button,input "java-helloworld-app", leave all other input as default, click "Create repository".

### 1.2 Create a Java helloword app by Maven
```console
$ cd /mnt/c/Users/pinm/code/maven/
$ mvn archetype:generate -DgroupId=com.mwit.javaapp -DartifactId=java-helloworld-app -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=1.0-SNAPSHOT -DinteractiveMode=false
```

### 1.3 Init repo 
```console
$ cd /mnt/c/Users/pinm/code/maven/java-helloworld-app
$ code README.md
$ git init
$ git add -A
$ git commit -m "add java maven archetype quickstart app"
$ git remote add origin https://github.com/maping/java-helloworld-app.git 
$ git push -u origin master
```

## 2. Create a repo automaticly by GitHub CLI

### 2.1 Download and install GitHub CLI
https://cli.github.com/

### 2.2 Generate a Personal Access Token with repo operation privilege
https://github.com/settings/tokens

### 2.3 Create a Java helloword web app by Maven
```console
$ cd /mnt/c/Users/pinm/code/maven/
$ mvn archetype:generate -DgroupId=com.mwit.javaapp -DartifactId=java-helloworld-app -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=1.0-SNAPSHOT -DinteractiveMode=false
```

### 2.4 Create a repo
```console
$ cd /mnt/c/Users/pinm/code/github/
$ gh repo create java-helloworld-app --public --confirm
```

### 2.5 Copy files
```console
$ cp -r /mnt/c/Users/pinm/code/maven/java-helloworld-app/* /mnt/c/Users/pinm/code/github/java-helloworld-app
$ cd /mnt/c/Users/pinm/code/github/java-helloworld-app
$ code README.md
$ git add -A
$ git commit -m "add java maven archetype quickstart app"
$ git push -u origin master
```

## 3. Compile and run application
```console
$ cd /mnt/c/Users/pinm/code/maven/java-helloworld-app
$ mvn clean package
$ java -jar target/java-helloworld-app-1.0-SNAPSHOT.jar
Hello World!
```

## 4. Create a CI/CD Pipeline
Change "package to "deploy", in "Advanced" section, check "Authenticate built-in Maven feeds".

## 5. Create Feed
Click "Artifacts", click "+ Create Feed", input "java-helloworld-app".

## 6. Deploy application
Edit pom.xml, add the repo to both your pom.xml's "repositories" and "distributionManagement" sections.
```code
<repositories>
    <repository>
      <id>java-helloworld-app</id>
      <url>https://pkgs.dev.azure.com/maping930883/java-helloworld-app/_packaging/java-helloworld-app/maven/v1</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
  </repositories>

  <distributionManagement>
    <repository>
      <id>java-helloworld-app</id>
      <url>https://pkgs.dev.azure.com/maping930883/java-helloworld-app/_packaging/java-helloworld-app/maven/v1</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
  </distributionManagement>
```
Add or edit the settings.xml file in ${user.home}/.m2.
```console
$ cp /mnt/c/software/apache/maven/conf/settings.xml /mnt/c/Users/pinm/.m2/
```
Add the server to your settings.xml's "servers" sections.
```code
    <server>
      <id>java-helloworld-app</id>
      <username>maping930883</username>
      <password>*******************************</password>
    </server>
```
>Attention: Generate a Personal Access Token with Packaging read & write scopes and paste it into the "password" tag.

>Attention: "id" in "server" section must match "id" in "repository" section.

```console
$ mvn clean package
$ mvn deploy
```
Click "Artifacts", "com.mwit.javaapp:java-helloworld-app Version 1.0-SNAPSHOT" shows up.

## 7. Replease 1.0
### 7.1 Add .gitignore file
```code
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties
.mvn/wrapper/maven-wrapper.jar
.settings
.vscode
.classpath
.project
release.properties
```
### 7.2 Modify pom.xml

### 7.3 Prepare Release
Before release, you much git push all local code to remote repo.
```console
$ mvn release:prepare
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.mwit.javaapp:java-helloworld-app >----------------
[INFO] Building java-helloworld-app 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-release-plugin:2.5.3:prepare (default-cli) @ java-helloworld-app ---
[INFO] Verifying that there are no local modifications...
[INFO]   ignoring changes on: **\pom.xml.next, **\release.properties, **\pom.xml.branch, **\pom.xml.tag, **\pom.xml.backup, **\pom.xml.releaseBackup
[INFO] Executing: cmd.exe /X /C "git rev-parse --show-toplevel"
[INFO] Working directory: C:\Users\pinm\code\github\java-helloworld-app
[INFO] Executing: cmd.exe /X /C "git status --porcelain ."
[INFO] Working directory: C:\Users\pinm\code\github\java-helloworld-app
[INFO] Checking dependencies and plugins for snapshots ...
What is the release version for "java-helloworld-app"? (com.mwit.javaapp:java-helloworld-app) 1.0: :
What is SCM release tag or label for "java-helloworld-app"? (com.mwit.javaapp:java-helloworld-app) java-helloworld-app-1.0: :
What is the new development version for "java-helloworld-app"? (com.mwit.javaapp:java-helloworld-app) 1.1-SNAPSHOT: :
......
```
>Attention: Run `mvn release:rollback` if you find someting wrong. This will delete release.properties and pom.xml.releaseBackup file, but won't delete Tag (local and remote).

### 7.4 Perform Release
```console
$ mvn release:perform
```
Click "Artifacts", found "com.mwit.javaapp:java-helloworld-app Version 1.0-SNAPSHOT" replaced with "Version 1.0".

>注意：手动删除 "Artifacts" 中的构件，会导致部署同版本的构件失败，好像 "Artifacts" 在哪里记录了所有的发布的构件，所以尽量一次做对。

### 7.5 Create a branch based on a Tag
```console
$ git checkout -b release/1.0 1.0
$ git push origin release/1.0:release/1.0
```

## 8. Delete repo
Click repo "maping/java-helloworld-app", then click "Settings", then drop down to "Danger Zone", click "Delete this repository".

# Reference
1. https://www.cnblogs.com/cowboys/p/10400784.html
