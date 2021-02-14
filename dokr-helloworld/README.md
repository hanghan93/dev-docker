# dokr-helloworld

This module is useful to create a small java application, where you're going to create a executable jar file, in addition, you'll see how to create a docker image.


Here, I created Dockerfile manually, you can create Dockerfile in any directory with any name you want as per your convenience as long as you pass $contextDir and $dockerFile param in a plugin configuration, please check it out the pom.xml.

Once you're done with your setup, please run below commands in your terminal. This will create a jar file, copy the jar into the image, and deploy to the local docker image repository.
Note: Make sure you've running instance of the docker in your local machine.
```maven
cd dokr-helloworld
mvn clean package k8s:build -Pcontext-and-file
```