# Synator Kubernetes Secret and ConfigMap synchronizer

Synator synchronize your Secrets and ConfigMaps with your desired namespaces

# Usage
Add annotation `synator/sync=yes` to Secret or ConfigMap. 
Optionally add one of these annotations in include specific destination
namespaces, or exclude the namespaces from the sync.
`synator/include-namespaces='namespace1,namespace2'`
`synator/exclude-namespaces='kube-system,kube-node-lease'`

# Reload pod when config upgraded
Add annotation `synator/reload: "secret:example"` to pod or deployment template
When secret example updated busybox pod will reload
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: busybox
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      name: busybox
  template:
    metadata:
      labels:
        name: busybox
      annotations:
        synator/reload: "secret:selam"
    spec:
      containers:
        - name: busybox
          image: busybox
          command:
            - "sleep"
            - "1h"
```
# Triggers
 - When update config or secret
 - When create config or secret

# Watching Namespaces

synator Operator installs with cluster wide permissions, however you can optionally control which namespaces it watches by by setting the WATCH_NAMESPACE environment variable.

`WATCH_NAMESPACE` can be omitted entirely, or a comma separated list of k8s namespaces.

- `WATCH_NAMESPACE=""` will watch for resources across the entire cluster.
- `WATCH_NAMESPACE="foo"` will watch for resources in the foo namespace.
- `WATCH_NAMESPACE="foo,bar"` will watch for resources in the foo and bar namespace.

# Build and deploy
Build docker image

```
docker build -t <usename>/synator:v1 .
```

Edit deploy.yml with your image name

```
kubectl apply -f deploy.yml
```
