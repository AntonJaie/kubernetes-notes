# Kubernetes Object Model
## Pods
- The smallest and simpletes K8s object
- often represents a single instance of the application
- can contain one or more containers
  - Multi-container pods: the convention is to group related containers (e.g. 1 master container, 1 helper container)

- pods share the same network namespace
- ephemeral in nature
  - e.g. can't self heal, can't replicate, fault tolerance
  - that is why they are deployed with controllers. Example of controllers are:
    - Deployments
    - ReplicaSets
    - ReplicationControllers

### Sample Configuration Manifest for Pods

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.15.11
    ports:
    - containerPort: 80
```

*apiVersion* - should be v1 for Pod object definition

*kind* - should be *_Pod_*

*metadata* - should contain the name, labels, namespace, etc

*spec* - should contain the intent of the Pod (PodSpec). e.g. containers, image, ports