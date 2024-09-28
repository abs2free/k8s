## install repo
```sh
helm repo add elastic https://helm.elastic.co
helm search repo elastic
```

## create namespace

```sh
kubectl create namespace monitoring
```

## install

```sh
helm upgrade --install elasticsearch elastic/elasticsearch -f elasticsearch/values.yml -n monitoring
helm upgrade --install filebeat elastic/filebeat -f filebeat/values.yml -n monitoring
helm upgrade --install logstash elastic/logstash -f logstash/values.yml -n monitoring
helm upgrade --install kibana elastic/kibana -f kibana/values.yml -n monitoring
```


## login credentials 
```sh
kubectl get secret elasticsearch-master-credentials -o jsonpath="{.data.username}" | base64 --decode

kubectl get secret elasticsearch-master-credentials -o jsonpath="{.data.password}" | base64 --decode
```

## storage class

1. check storage

```sh
kubectl get storageclass
```

2. download storageclass

```sh
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

3. make storageclass as default

```sh
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```
