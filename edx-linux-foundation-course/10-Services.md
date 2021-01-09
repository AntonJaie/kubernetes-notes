# Services
- used to access the application or pod
  - pods are ephemeral in nature (not static IP)
- used to expose Pods, ReplicaSet, Deployments, DaemonSets, and StatefulSets


## Sample Service Object
```
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
```

### Name of Fields 
*selector._* - the label where the service will be applied
*type* - not mentioned above, but the default type of a service is **ClusterIP**
*targetPort* - port of the Pod where the traffic will be sent
*Port* - port of the Service where the requests will be sent

## kube-proxy
- watches the API server for updates of Services
- implements Service configuration for traffic routing
  - configures iptables rules to capture traffic from the ClusterIP and forwards it to the Pods

## Service Discovery
- the process of discovering of services at runtime

### Two types of Discovery
## Envinroment Variables
- environmental variable will point to the corresponding service

```
REDIS_MASTER_SERVICE_HOST=172.17.0.6
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_PORT=tcp://172.17.0.6:6379
REDIS_MASTER_PORT_6379_TCP=tcp://172.17.0.6:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=172.17.0.6
```

## DNS
- creates a DNS record for each Service. The format is:
```
my-svc.my-namespace.svc.cluster.local.
```
- Services are discoverable within the same namespace by their names

Example:
- If we have a Service called **redis-master** in namespace **my-ns**:
  - all Pods in the **my-ns** namespace will look up the service using **redis-master**
  - Pods on other namespace will look the service using:
    - **redis-master.my-ns**
    - when providing the FQDN: **redis-master.my-ns.svc.cluster.local**

## ServiceType
- used to limit the access scope
  - within a cluster
  - within a cluster and external world
  - either inside or outside the cluster

### ClusterIP
- default service type
- a Service receives a Virtual IP address

### NodePort
- a high-port, from **30000-32767**, mapped to the Service
- this is useful when we want the Service to be accessible from the external world

### LoadBalancer
- nodePort and clusterIP are automatically created
- Service is exposed at a static port on each worker node
- will only work with cloud provider's load balancer feature e.g. AWS and GCP
- if no load balancer feature is configured, the LoadBalancer IP will not be populated, it remains in Pending State, but the Service will work as **NodePort** type service.

### ExternalIP
- Service can be mapped to an **externalIP** if it can route to one or more worker nodes.
- this type of service requires an external cloud provider e.g GCP or AWS and a LoadBalancer configured

### ExternalName
- mapped a Service to a DNS name
- specify this Service with the spec.extenralName parameter

