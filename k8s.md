# components
## node and pod

### pod

- Smallest unit of k8s 
- Abstraction over container
- Usually 1 application per pod
- Each pod gets its own ip address
- New IP address on re-creation
    

## service
- Permanent IP address

## Configmap and Secret
### Configmap
- External configuration of your application

### secret
- used to store secret data
- base64 encoded

## volumes

## Deployment and Statefulset
### Deployment
- blueprint for my-app pods
- abstraction of pods

> Db cant be replicated via Deployment!

### Statefulset
- statefulset for stateful apps or databases

# K8s Architecture

## Node
- each Node has multiple Pods on it
- 3 processes must be installed on every Node
    - Kubelet
    - Kube Proxy
    - Container runtime
- Workder Nodes do the actual work


Kubelet interacts with both -the container and node
Kubelet starts eht pod with a container inside
Kube Proxy forwards the requests

## Master Process
- 4 process run on every master node!
    - Api Server
        - cluster gateway
        - acts as a gatekeeper for authentication!
    - Scheduler
        - manage pods
    - Controller Manager
        - detects cluster state changes 
    - etcd
        - etcd is the cluster brain!
        - CLuster changes get Stored in the key value store



# command
```sh
kubectl get nodes
kubectl get services
kubectl get deployment
kubectl get replicatset

kubectl create deployment NAME --image=image [--dry-run] [options]
kubectl create deployment nginx-delp --image=nginx

kubectl edit deployment nginx-depl 

kubectl logs [pod name]
kubectl describe pod [pod name]


kubectl exec -it [pod name] -- bin/bash


kubectl delete deployment [deployment name]

kubectl apply -f [filename]

```

# volumes
- Persistent Volume
- Persistent Volume Claim
- Storage Class


## storage requirements
1. Storage that doesn't depend on the pod lifecycle
2. Storage must be avaliable on all nodes
3. Storage needs to survive even cluster crashes

## Persistent Volume
- not namespaced
- pv outside of the namespaces
- accessble to whole cluster

















