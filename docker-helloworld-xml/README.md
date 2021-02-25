# 2. Build Docker image using the XML build configuration

This module is useful to build a Docker image of the small java application using the kubernetes-maven-plugin xml configuration.
Here, I've not created a Dockerfile manually but with the help of xml plugin configuration we can generate a Dockerfile.

In the build section of the <image> we can tell a plugin how to create an image, for more details please check the official documentation here [k8s:build](https://www.eclipse.org/jkube/docs/kubernetes-maven-plugin#jkube:build)
```xml
<build>
    <from>openjdk:8</from>
    <assembly>
        <targetDir>/deployments</targetDir>
        <inline>
            <fileSets>
                <fileSet>
                    <directory>${project.build.directory}/docker-helloworld-xml-1.0-SNAPSHOT-spring-boot.jar</directory>
                </fileSet>
            </fileSets>
        </inline>
    </assembly>
    <workdir>/deployments</workdir>
    <cmd>java -jar docker-helloworld-xml-1.0-SNAPSHOT-spring-boot.jar</cmd>
</build>
```
Please clone the repository if you haven't yet using the below command
```git
cd folder/to/clone-into/
git clone https://github.com/hanghan93/jkube-k8s-maven.git
```

Once you're done with your setup, please run below commands in your terminal. This will create a jar file, copy the jar into the image, and deploy to the local docker image repository.
Note: Make sure you've the running instance of the docker in your local machine.
```sh
cd docker-helloworld-xml
mvn clean package k8s:build
```

You can check docker image in your local repository by running below command.
 ```sh
docker image ls
 ```

Once image is ready, we have to create a docker container to run the image.
 ```sh
docker container create --name helloworld-xml hanghan93/docker-helloworld-xml
docker container start helloworld-xml
docker logs helloworld-xml
 ```