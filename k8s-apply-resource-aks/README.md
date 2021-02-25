# 5. k8s-apply-resource-aks

In this module, we will cover the following

1. How to install Azure CLI
2. Login to Azure and connect AKS using command line interface.
3. Verify cluster node is running
4. Verify Kubernetes cluster has access to connect an ACR (Azure container registry)
5. Apply the yaml resources to Kubernetes cluster
6. Check the running status of pods

##### How to install Azure CLI
Azure has provided CLI to interect with Azure services from your local machine [click here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) to see steps to install the Azure CLI in your machine.

##### Login to Azure and connect AKS using command line interface.
To login to Azure run the following commands in CLI
```bash
az login -u <username> -p <password>
``` 
There are methods as well to connect Azure from CLI, please [click here](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli) to see it.

Now it's time to install kubectl, it can be done very easily by following command.
```bash
az aks install-cli
```
To configure kubectl to connect to your Kubernetes cluster, use the az aks get-credentials command. This command downloads credentials and configures the Kubernetes CLI to use them.

```bash
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster

e.g.

C:\Users\onepoint>az aks get-credentials --resource-group hardikpocs --name akshardikpoc
Merged "akshardikpoc" as current context in C:\Users\onepoint\.kube\config
```


By default, the credentials are merged into the .kube/config file so kubectl can use them.

##### Verify cluster node is running
To verify the connection to your cluster, use the kubectl get command to return a list of the cluster nodes.
```bash
kubectl get nodes
```
##### Validate an ACR is accessible from an AKS cluster.
```
az aks check-acr --acr
                 --name
                 --resource-group
                 [--subscription]
```

```log
e.g 
az aks check-acr --name akshardikpoc --resource-group hardikpocs --acr acrhardikpocs.azurecr.io

Merged "akshardikpoc" as current context in C:\Users\onepoint\AppData\Local\Temp\tmpy1cmktt2
Error attaching, falling back to logs: Internal error occurred: error attaching to container: container not running (**)
[2021-02-25T10:56:07Z] Checking host name resolution (acrhardikpocs.azurecr.io): SUCCEEDED
[2021-02-25T10:56:07Z] Canonical name for ACR (acrhardikpocs.azurecr.io): ****.cloudapp.azure.com.
[2021-02-25T10:56:07Z] ACR location: **
[2021-02-25T10:56:07Z] Loading azure.json file from /etc/kubernetes/azure.json
[2021-02-25T10:56:07Z] Checking managed identity...
[2021-02-25T10:56:07Z] Cluster cloud name: AzurePublicCloud
[2021-02-25T10:56:07Z] Kubelet managed identity client ID: 5cc*
[2021-02-25T10:56:09Z] Validating managed identity existance: SUCCEEDED
[2021-02-25T10:56:09Z] Validating image pull permission: **FAILED**
[2021-02-25T10:56:09Z] ACR acrhardikpocs.azurecr.io rejected token exchange: ACR token exchange endpoint returned error status: 403. body: {"errors":[{"code":"DENIED","message":"retrieving permissions failed"}]}
```

As you see in the logs, image pull permission has failed that means AKS doesn't have permission to remote repository. To solve that let's create a Secret, the Kubernetes uses an image pull secret to store information needed to authenticate to your registry.

Create an image pull secret with the following kubectl command:

```
kubectl create secret docker-registry <secret-name> \
    --namespace <namespace> \
    --docker-server=<container-registry-name>.azurecr.io \
    --docker-username=<service-principal-ID> \
    --docker-password=<service-principal-password>

e.g.
kubectl create secret docker-registry regcred --docker-server=acrhardikpocs.azurecr.io --docker-username=acrhardikpocs --docker-password=**
```
To understand the contents of the regcred Secret you just created, start by viewing the Secret in YAML format:
```
kubectl get secret regcred --output=yaml
```

Once you have Secret, you can configure it in the Kubernetes fragment file src\main\jkube\deployment.yml like below.

```yaml
spec:
  template:
    spec:
      containers:
        - imagePullPolicy: Always
      imagePullSecrets:
        - name: regcred
```

##### Apply Kubernetes resource yaml file to Kubernetes cluster

Let's run the final command and see output:

``` java
mvn clean package -Djkube.docker.username=acrhardikpocs -Djkube.docker.password=** k8s:build k8s:resource k8s:push k8s:apply
```
Here, 
| Goal  | Description |
| :---| :---|
|k8s:build  | Build Docker image as per the plugin configuration  |
|k8s:resource  | It will generate Kubernetes manifest file as per the plugin configuration and resources fragment  |
|k8s:resource  |Push Docker image to ACR  |
|k8s:apply  | Deploy resources to AKS|

##### Check the running status of pods
```
kubectl get pods -w
kubectl pod logs *podname*
```