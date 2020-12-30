# Deployment

- provides updates for Pods and ReplicaSets
- part of the master node controller manager
- **rollouts** update of revision version
- **rollbacks** downgrades of revision version

## Revision Version
- state of a current deployment
- whenever an update is applied to a deployment another deployment is created but with a different *Revision version*

## Rolling Update
- update of specific properties of deployment
- e.g. from *Revision 1* to *Revision 2* is called _rolling update_
> Take note: Scaling or Labeling do not trigger a rolling update

## Rollback
- Deployment keeps its prior configuration for seamless downgrade of revision versions
