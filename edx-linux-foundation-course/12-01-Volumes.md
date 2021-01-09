# Volumes
- storage resouces in order to provide data to be consumed by containers or to store data produced by containers
- all data stored inside a container will be deleted if the container restarts
- volumes are used to allow storage abstractions
- a mount point on the container's file system backed by a storage medium

- linked to a Pod and shared among the containers in a Pod

## VolumeType
- decides the properties of the directory, size, content, default access modes

### Examples of Volume Types
- *emptyDir*
  - created for the Pod
  - tightly coupled with the Pod
    - when a Pod is deleted the emptyDir is deleted forever

- *hostPath*
  - shared directory between the host and the pod
    - when the pod is terminated, the Volume is still available on the host

- *gcePersistentDisk*
  - can be mounted to GCE persistent disk

- *awsElasticBlockStore*
  - can be mounted to an AWS EBS Volume

- *azureDisk*
  - can be mounted to microsoft azure data disk

- *azureFile*
  - can be mounted to microsoft azure file volume

- cephfs
  - an existing CephFS volume can be mounted
  - once a Pod terminates, the volume is unmounted and the contents are preseved

- nfs
  - can mount a NFS share

- secret
  - can pass sensitive info e.g. passwords to Pods

- configMap
  - can provide configuration data, or shell cmds and arguments to Pods

- persistentVolumeClaim
  - a PersistentVolume to a Pod

## Persistent Volumes
- a storage abstraction to manage and consume persistent storage given the various examples of Volume Types above
- managed by Cluster Administrators

Sample PV Configuration
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: foo-pv
spec:
  storageClassName: ""
  claimRef:
    name: foo-pvc
    namespace: foo
  ...
```

> NOTE: *claimRef* bounds the PV to the specified PVC so that no other PVC can bind to the PV

## Persistent Volume Claims
- accepts request for dynamic PV creation based on type, access mode, and size
- used to create Persistent Volumes

Sample PVC Configuration
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: foo-pvc
  namespace: foo
spec:
  storageClassName: "" # Empty string must be explicitly set otherwise default StorageClass will be set
  volumeName: foo-pv
  ...
```

### Mode of Access
- ReadWriteOnce (read-write by a single node)
- ReadOnlyMany (read-only by many nodes)
- ReadWriteMany (read-write by many nodes)

## Steps for Persistent Volumes and Persistent Volume Claim
1. Administrator provides the Persistent Volumes
2. User Request a claim
3. Claim Request Honoured
4. The PVC resource can be used by the containers of the POd
5. After usage, the PVC can be released.

### After Usage of PVC
- Based on *persistentVolumeReclaimPolicy*
- *reclaimed* - for admin to verify and/or aggregate data
- *deleted* - data and volume deleted
- *recycled* - only data is deleted


## Container Storage Interface (CSI)
- standard to work with various storage vendors


## Using Shared hostPath Volume type
Create a directory inside the minikube (host machine)
```
$ minikube ssh
$ mkdir pod-volume
```

