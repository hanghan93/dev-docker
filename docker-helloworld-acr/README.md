# 3. docker-helloworld-acr
###In this module we will see how to generate a Dockerfile on the fly by xml configuration and push docker images to Azure Container Registry

This module is useful to push images to remote repository, here I've used Azure Container Registry (ACR), but you can use any other remote repo, approach will be the same.
###Prerequisite:
1. [Simple Docker image build using Dockerfile (Zero Config)](https://github.com/hanghan93/jkube-k8s-maven/tree/master/docker-helloworld-simple)
2. [Build Docker image using XML configuration](https://github.com/hanghan93/jkube-k8s-maven/tree/master/docker-helloworld-xml)
3. Create ACR

Please clone the repository if you haven't yet using the below command
```git
cd folder/to/clone-into/
git clone https://github.com/hanghan93/jkube-k8s-maven.git
```
The only difference from the [previous module](https://github.com/hanghan93/jkube-k8s-maven/tree/master/docker-helloworld-xml) is that push images to remote registry.
There are many ways to configure and authenticate a remote repo, please have a look at [official documentation](https://www.eclipse.org/jkube/docs/kubernetes-maven-plugin#authentication). Here I've shown only using XML configuration:
```xml        
<configuration>
    <registry>**.azurecr.io</registry>
    <authConfig>
        <push>
            <username>**</username>
            <password>**</password>
        </push>
    </authConfig>
</configuration>
```
Once you're done with your setup, please run below commands in your terminal
```sh
cd docker-helloworld-acr
mvn clean package k8s:build k8s:push
```
If everything is successful then you'll receive below logs in your console
```log
[INFO] --- kubernetes-maven-plugin:1.1.0:push (default-cli) @ docker-helloworld-acr ---
[INFO] k8s: Running in Kubernetes mode
[INFO] k8s: Building Docker image in Kubernetes mode
[INFO] k8s: The push refers to repository [**.azurecr.io/hanghan93/docker-helloworld-acr]
5aff8ed54279: Pushed      
02412b9dda81: Mounted from hanghan93/dokr-helloworld-acr 
d7b2c55f7e50: Mounted from hanghan93/dokr-helloworld-acr 
02f0a7f763a3: Mounted from hanghan93/dokr-helloworld-acr 
da654bc8bc80: Mounted from hanghan93/dokr-helloworld-acr 
4ef81dc52d99: Mounted from hanghan93/dokr-helloworld-acr 
909e93c71745: Mounted from hanghan93/dokr-helloworld-acr 
7f03bfe4d6dc: Mounted from hanghan93/dokr-helloworld-acr 
[INFO] k8s: latest: digest: sha256:a87338621b27a86f2c7c75199e32f0b6bdf4fdd4ab5bba3285c07e2761fd8ff5 size: 2006
```
At the end, please check the image in ACR
![Screenshot](https://github.com/hanghan93/jkube-k8s-maven/blob/master/docker-helloworld-acr/src/main/resources/img/tempsnip.png?raw=true)
