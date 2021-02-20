# 1. docker-helloworld-simple
### You'll see in this module..
#### 1. Learn how to use jkube-k8s-maven plugin
#### 2. Use manually created Dockerfile and build image using that file
This module is useful to create a small java application, where you're going to create a executable jar file, in addition, you'll see how to create a docker image.


Here, I created Dockerfile manually, you can create Dockerfile in any directory with any name you want as long as you pass $contextDir and $dockerFile param in a plugin configuration, please check it out the pom.xml file.

Please clone the repository if you haven't yet using the below command
```git
cd folder/to/clone-into/
git clone https://github.com/hanghan93/jkube-k8s-maven.git
```

Once you're done with your setup, please run below commands in your terminal. This will create a jar file, copy the jar into the image, and deploy to the local docker image repository.
Note: Make sure you've the running instance of the docker in your local machine.
```sh
cd docker-helloworld-simple
mvn clean package k8s:build
```
> Note: If docker is not running in your machine then you'll end up with below error in the console log
```log
[ERROR] Failed to execute goal org.eclipse.jkube:kubernetes-maven-plugin:1.1.0:build (default-cli) on project docker-helloworld-simple: Execution defaul
t-cli of goal org.eclipse.jkube:kubernetes-maven-plugin:1.1.0:build failed: No <dockerHost> given, no DOCKER_HOST environment variable, no read/writable
 '/var/run/docker.sock' or '//./pipe/docker_engine' and no external provider like Docker machine configured -> [Help 1]
[ERROR]
```
You can check docker image in your local repository by running below command.
 ```sh
docker image ls
 ```
![Screenshot](https://github.com/hanghan93/jkube-k8s-maven/blob/master/docker-helloworld-simple/src/main/resources/refImg/docker.JPG)