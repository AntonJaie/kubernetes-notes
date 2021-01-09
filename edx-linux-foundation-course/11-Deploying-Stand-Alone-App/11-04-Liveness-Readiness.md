# Liveness and Readiness

## Liveness
- to know when the kubelet needs to restart a container
- due to deadlock or memory pressure
- liveness command is checking the existence of a file */tmp/healthy*
Example:
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 3
      failureThreshold: 1
      periodSeconds: 5
```

**periodSeconds** - dictates when the file should be checked (Liveness)
**initialDelaySeconds** - delay for start up before checking for first probe
**failureThreshold** - threshold to declare the container unhealthy

### Types of Liveness Probes
#### HTTP Liveness Probe
- Probe which used HTTP request to check the health of a container

#### TCP Liveness Probe
- kubelet will attempt to open a socket to your container on a specified port. If it can establish a connection, the container is healthy

##
## Readiness
- to know when a container is ready to start accepting traffic. 
- A pod is ready once all of its containers are ready
- should allow enough time for the **Readiness** Probe before checking the **Liveness** probe. - An overlap can create an infinite loop

### Types of Readiness Probes
- Same as the Liveness Probes


## Fields for Readiness and Liveness

- *initialDelaySeconds* - number of seconds after the container has started before liveness or readiness probe are initiated. Default: 0 sec. Min: 0 sec.

- *periodSeconds* - how often to perform probe. Default: 10 sec. Min: 1 sec

- *timeoutSeconds* - number of seconds after the probe times out. Default: 1 sec. Min: 1 sec

- *successThreshold* - minimum consecutive successes for the probe to be considered successful after failure. Default: 1. Min: 1.

- *failureThreshold* - minimum consecutive failures of the container before giving up for readiness and liveness probes. Default: 1. Min: 1
