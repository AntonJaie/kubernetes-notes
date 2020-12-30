# Namespaces

- sometimes called *virtual sub-clusters*
- objects created inside a namespace are unique
- can be used to isolate users, developer teams, applications, etc

To get the available namespaces:
```
> kubectl get namespaces
```

## Four Initial Namespaces of Kubernetes
- *kube-system* - for the K8s system, mostly control plane objects

- *kube-public* - for exposed objects about the cluster

- *default* - for objects created by admin or developers if no namespace is provided in the configuration

- *kube-node-lease* - for node lease objects used for heartbeat data

### Resource Quotas
- limit overall resources consumed within a *namespace*

### LimitRanges
- limit resources consumed by *Pods* or *Containers*