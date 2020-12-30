# ReplicaSets

- implements the replication and self-healing aspects of *ReplicationController*
- supports equality- and set-based selectors

## ReplicaSets in comparison to Deployments
- ReplicaSets offer limited set of features. Thus, the recommended Pod Controller is the Deployment which automatically creates a ReplicaSet, which then creates a Pod. 

