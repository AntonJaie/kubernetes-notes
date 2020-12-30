# Replication Controllers

- **no longer recommended controller**
- ensures the specified number of replicas of a Pod is running at any given time
  - if there are more pods than the desired count, the controller randomly terminates the number of pods
  - if there are fewer pods than the desired count, the controller requests additional pods to be created to meet the count

> The default recommended controller is the **Deployment** which configures a **ReplicaSet** to manage Pods' lifecycle