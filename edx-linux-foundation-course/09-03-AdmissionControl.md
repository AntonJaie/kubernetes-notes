# Admission Control
- used to specify granular access control policies
  - allowing privileged containers
  - checking on resource quotas
  - etc
- each policy will use a different admission controller e.g. ResourceQuota, DefaultStorageClass, AlwaysPullImages, etc

- to enable admission controls, the API server should be started with **--enable-admission-plugins=...** parameter

Example:
```
--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurityPolicy,DefaultStorageClass
```

- K8s has some admission controllers by default
- custom plugins for admission control can be used through webhooks
