# Kubernetes Object Model

## Kubernetes Objects Overview
Example of Kubernetes objects are as follows:
- Pods
- ReplicaSets
- Deployments 
- Namespaces

### Sample Configuration File of Deployment object
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.11
        ports:
        - containerPort: 80
```

*apiVersion* - first required field, specified the API endpoint on the API server

*kind* - the specific object type (e.g Deployment, Pod, ReplicaSet, Namespace)

*metadata* - object's basic information
- *name* - name of the object
- *labels* - specific label of the object

*spec* - defining the desired state of the Deployment object
 - *replicas* - number of desired replicas
 - *template* - Pod template
 - *template.spec* - This is the desired state of the Pod

 To create the specific object, invoke the following command:
 ```
> kubectl apply -f <configuration-file>.yml
 ```