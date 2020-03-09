# Objectives
- [ ] **Pods**
- [ ] **Deployments**
- [ ] **Services**
- [ ] **Resources**
- [ ] **Jobs/Cronjobs**
- [ ] **Volumes/Storage**
- [ ] **Secrets/ConfigMaps**

> :bulb: Bash Autocompletion 
```bash
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```
## Pods 
#### Create busybox pod via terminal  
```bash
kubectl run busybox --restart=Never --image=busybox 
```
#### Fake the creation of pod via terminal  
```bash
kubectl run busybox --restart=Never --image=busybox --dry-run 
```
#### Get yaml for creating pod
```bash
kubectl run <pod-name> --restart=Never --image=busybox --dry-run -o yaml
```
#### Output:
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-name
  name: pod-name
spec:
  containers:
  - image: busybox
    name: pod-name
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
#### Change a pods image:  
```bash
# kubectl set image POD/POD_NAME CONTAINER_NAME=IMAGE_NAME:TAG  
kubectl set image pod/nginx nginx=nginx:1.7.1
```
#### Create a pod that accepts traffic on port 80
```bash
kubectl run pod --image=nginx --restart=Never --port=80
```
#### Create a pod and expose a port on 80
> :bulb: --expose=false: If true, a public, external service is created for the container(s) which are run 
```bash
kubectl run pod --image=nginx --restart=Never --port=80 --expose
```
This will create not only a pod, but also an external service.  
#### Yaml of objects created below:
```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  name: enginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    run: enginx
status:
  loadBalancer: {}
---
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: enginx
  name: enginx
spec:
  containers:
  - image: nginx
    name: enginx
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
### Example Deployment manifest:  

#### Get deployment YAML manifest w/:  
```
kubectl run testdeploy --image=busybox -o yaml --dry-run
```
#### Output:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    run: testdeploy
  name: testdeploy
spec:
  replicas: 1
  selector:
    matchLabels:
      run: testdeploy
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: testdeploy
    spec:
      containers:
      - image: busybox
        name: testdeploy
        resources: {}
status: {}
```
#### Create a service that exposes a deployment on port 6262
```
kubectl expose <object-type> <object-name> --port 6262 --target-port=8080 # target port is exposed container port
```


## Managing Resources
#### Best Practice: Always specify resource requests and limits for containers.

### Resoure Requests
Specifies the minimum amount of that resource the Pod needs to run.  
#### Create pod w/ resource requests via `kubectl`:
```
kubectl run resourcePod --image=busybox --restart=Never --requests="memory=10Mi,cpu=100m" --dry-run -o yaml
```
#### Output:
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
#### Create pod w/ resource limits via `kubectl`:
```
kubectl run resourcePod --image=busybox --restart=Never --limits="memory=10Mi,cpu=100m" --dry-run -o yaml
```
#### Output:
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
