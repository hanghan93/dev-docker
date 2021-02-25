# Kubernetes maven plugin

The kubernetes-maven-plugin brings Java applications on to Kubernetes. It provides a tight integration into Maven and benefits from the build configuration already provided.

```xml
        <dependency>
            <groupId>org.eclipse.jkube</groupId>
            <artifactId>kubernetes-maven-plugin</artifactId>
            <version>${jkube.version}</version>
        </dependency>
```
In this module we are going to cover the following.

1. [Build Docker image using the external Dockerfile provided.](https://github.com/hanghan93/jkube-k8s-maven/tree/master/docker-helloworld-simple)
2. [Build Docker image using the XML build configuration.](https://github.com/hanghan93/jkube-k8s-maven/tree/master/docker-helloworld-xml)
3. [Build and Push Docker image to remote repository (Azure Container Registry).](https://github.com/hanghan93/jkube-k8s-maven/tree/master/docker-helloworld-acr)
4. [Generate the Kubernetes resource descriptors.](https://github.com/hanghan93/jkube-k8s-maven/tree/master/k8s-generate-resources)
5. [Generate the Kubernetes resource descriptors using the external resource fragment file.](https://github.com/hanghan93/jkube-k8s-maven/tree/master/k8s-generate-resources-fragments)
6. [Create image pull secret, configure and apply resources to AKS (Azure Kubernetes Service)](https://github.com/hanghan93/jkube-k8s-maven/tree/master/k8s-apply-resource-aks)

