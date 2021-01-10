## ConfigMaps
- configuration data as key-value pairs to be consumed by objects (form of env variables)

## Sample Creation of ConfigMap through Command Line
```
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
```

## Sample Creation of ConfigMap through Configuration file (yaml)
Filename: 13-02-configmap-conf.yaml
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: customer1
data:
  TEXT1: Customer1_Company
  TEXT2: Welcomes You
  COMPANY: Customer1 Company Technology Pct. Ltd.
```

Command to create:
```
kubectl create -f 13-02-configmap-conf.yaml
```

## Sample Creation of ConfigMap through File (.properties)
Filename: 13-03-permission-reset.properties
```
permission=read-only
allowed="true"
resetCount=3
```

Command to create:
```
kubectl create configmap permission-config --from-file=13-03-permission-reset.properties
```

## Use ConfigMaps Inside Pods
### as Environment Variables
- use *env* tag inside the container definition

Sample
```
...
  containers:
  - name: myapp-specific-container
    image: myapp
    env:
    - name: SPECIFIC_ENV_VAR1
      valueFrom:
        configMapKeyRef:
          name: config-map-1
          key: SPECIFIC_DATA
    - name: SPECIFIC_ENV_VAR2
      valueFrom:
        configMapKeyRef:
          name: config-map-2
          key: SPECIFIC_INFO
...
```

### as Volumes
- use *volumes* to map the configmap as volume in a pod
```
...
  containers:
  - name: myapp-vol-container
    image: myapp
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: vol-config-map
...
```

## Secrets
- encode sensitive information e.g. passwords, token
- secrets are stored in *etcd* but it is in plain text
  - therefore, needs to limit access to the API server
  - should enable encryption of data in *etcd* by the administrator

## Sample creation of Secret through cmd line
```
kubectl create secret generic my-password --from-literal=password=mysqlpassword
```

## Sample creation of Secret manually (using *data*)
1. encode the password in *base64*
```
echo mysqlpassword | base64
```

2. create the configuration below:
```
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
data:
  password: bXlzcWxwYXNzd29yZAo=
```

But the password can be decoded by others so it is not **really** secure!

## Sample creation of Secret manually (using *stringData*)
1. create the configuration below:
```
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
stringData:
  password: mysqlpassword
```

## Sample creation of Secret using File
1. encode the password and save it in a txt file
```
echo mysqlpassword | base64
echo -n 'bXlzcWxwYXNzd29yZAo=' > password.txt
```

2. create the secret using kubectl command
```
kubectl create secret generic my-file-password --from-file=password.txt
```

## Use Secrets Inside Pods
### Using Secrets Env variables

```
....
spec:
  containers:
  - image: wordpress:4.7.3-apache
    name: wordpress
    env:
    - name: WORDPRESS_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-password
          key: password
....
```

### Using Secrets as Files from Pod
```
....
spec:
  containers:
  - image: wordpress:4.7.3-apache
    name: wordpress
    volumeMounts:
    - name: secret-volume
      mountPath: "/etc/secret-data"
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: my-password
....
```