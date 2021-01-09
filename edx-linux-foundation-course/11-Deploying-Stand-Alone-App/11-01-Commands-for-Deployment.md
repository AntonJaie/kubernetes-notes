# Command in the Chapter 11 exercise

### List of Deployments
```
kubectl get deployments
```

### List of ReplicaSets
```
kubectl get replicasets
```

### List of Pods
```
kubectl get pods
```

### Describe specific pod
```
kubectl describe pod web-dash-7797d85794-mcfkw
```

### Getting pods with specific Label
```
kubectl get pods -L k8s-app, label2
```

### Getting pods with a given Label
```
kubectl get pods -l k8s-app=web-dash
```

### Delete deployments
```
kubectl delete deployments web-dash
```

### Creating Deployments
```
kubectl create -f <.yaml> file
```

### Expose a Deployment
```
kubectl expose deployment webserver --name=web-service --type=NodePort
```