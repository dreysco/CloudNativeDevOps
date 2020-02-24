# Part 1
In K8s(kubernetes) all resources such as Deployments or Pods are represented by records in its internal database.  

Usual format for K8s manifest is YAML, but can also understand JSON.

### Example Deployment manifest:  

> Get deployment YAML manifest w/:  
```
kubectl run testdeploy --image=busybox 
```

## Managing Resources
> Best Practice: Always specify resource requests and limits for containers.

### Resoure Requests
Specifies the minimum amount of that resource the Pod needs to run.  
> Create pod w/ resource requests via `kubectl`:
```
kubectl run resourcePod --image=busybox --restart=Never --requests="memory=10Mi,cpu=100m" --dry-run -o yaml
```
> Output:
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: resourcePod
  name: resourcePod
spec:
  containers:
  - image: busybox
    name: resourcePod
    resources:
      requests:
        cpu: 100m
        memory: 10Mi
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
### Resource Limits
Specifies the maximum amount of that resource the Pod is allowed to use.
> Create pod w/ resource limits via `kubectl`:
```
kubectl run resourcePod --image=busybox --restart=Never --limits="memory=10Mi,cpu=100m" --dry-run -o yaml
```
> Output:
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: resourcePod
  name: resourcePod
spec:
  containers:
  - image: busybox
    name: resourcePod
    resources:
      limits:
        cpu: 100m
        memory: 10Mi
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
