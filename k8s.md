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

#### key-value pair
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
  namespace: my-namespace
data:
  database_url: mongodb-service 

```

#### config-file
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mosquitto-config-file
data:
  mosquitto.conf: |
    log_dest stdout
    log_type all
    log_timestamp true
    listener 9001
```

### secret
- used to store secret data
- base64 encoded

#### key-value pair
```yaml
apiVersion: v1
kind: Secret
metadata: 
  name: mongodb-secret
  namespace: my-namespace
type: Opaque
data:
  mongo-root-username: dXNlcm5hbWU=
  mongo-root-password: cGFzc3dvcmQ=

```
#### secret-file
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mosquitto-secret-file
type: Opaque
data:
  secret.file: |
    c29tZXN1cGVyc2VjcmV0IGZpbGUgY29udGVudHMgbm9ib2R5IHNob3VsZCBzZWU=
```

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





# services
- ClusterIP Services
- Headless Services
- NodePort Services
- LoadBalancer Services


what is Service and when we need it?
- Each Pod has its own IP Address
    - pods are ephemeral - are destroyed frequently!
- Service:
   - stable Ip address
   - loadbalancing
   - loose coupling
   - within & outside cluster


## ClusterIP Services
- default type
- IP address from node's range

## Headless Services
- Client wants to communicate with 1 specific pod directly
- Pods want to talk directly with specific pod
- so , not randomly selected
- use case: stateful applications , like databases
- Client needs to figure out ip sddresses of each pod? DNS Lookup - return single ip address (ClusterIp)
- Set ClusterIp to "None" - returns pod IP addres instead.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service-headless
spec:
  clusterIP: None
  selector:
    app: mongodb
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

## NodePort Services
- range: 30000 - 32767

## LoadBalancer Services
- LoadBalancer Service is an extension of NodePort Sercice
- NodePort Service is an extension of ClusterIP Sercice

# operater
- managing complete lifecycle of stateless apps
- kus can't automate the process natively for stateful apps
- operatorhub.io

## who creates these operators?

for example: mysql operator
- How to create the mysql cluster?
- How to run it?
- How to synachronize the data ?
- How to update?


# prometheus
## install Prometheus-operator
### add repos
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add stable https://charts.helm.sh/stable
    helm repo update

### install chart
    helm install prometheus prometheus-community/kube-prometheus-stack
    helm upgrade --install prometheus prometheus-community/kube-prometheus-stack


#### port-forwardings
###### Prometheus-UI
    kubectl port-forward service/prometheus-kube-prometheus-prometheus 9090

###### Alert Manager UI
    kubectl port-forward svc/prometheus-kube-prometheus-alertmanager 9093

###### Grafana
    kubectl port-forward deployment/prometheus-grafana 3000
    - username: admin
    - password: prom-operator

###### Grafana Dashboard credentials
    user: admin
    pwd: prom-operator (from values.yaml file set as default)


