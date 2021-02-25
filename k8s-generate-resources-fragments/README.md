# 5. k8s-generate-resources-fragments

Kubernetes fragments are user provided YAML files that can be enriched by the plugin. This allows expert users to use a plain configuration file with all their capabilities, but also to add project specific build information and avoid boilerplate code

Resource descriptors can be provided as external YAML files which will build a base skeleton for the applicable resource.

The "skeleton" is then post-processed by Enrichers which will complete the skeleton by adding the fields each enricher is responsible of (labels, annotations, port information, etc.). Maven properties within these files are resolved to their values.

With this model you can use every Kubernetes / OpenShift resource objects with all their flexibility, but still get the benefit of adding build information.

Let's start with one example, As we have seen in the previous module the generated yaml has imagePullPolicy: IfNotPresent by default, but we can change that value by using resource fragment file "deployment.yml".

Please create one file in src/main/jkube/deployment.yml and content of the file 
```yaml
spec:
  template:
    spec:
      containers:
        - imagePullPolicy: Always
      imagePullSecrets:
        - name: arc-aks-secret
```

Once you're done with your setup, please run below commands in your terminal. This will create a yaml file in a target/classes/META-INF/jkube/kubernetes directory.
```sh
cd k8s-generate-resources-fragments
mvn k8s:resource
```
As you see in yaml file, the generated yaml file 

```yaml
spec:
    template:
        spec:
          containers:
          - env:
            - name: KUBERNETES_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: HOSTNAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            image: acrfcngrptest.azurecr.io/hanghan93/k8s-generate-resources-fragments:latest
            imagePullPolicy: Always
            name: hanghan93-k8s-generate-resources-fragments
            securityContext:
              privileged: false
          imagePullSecrets:
          - name: arc-aks-secret
```