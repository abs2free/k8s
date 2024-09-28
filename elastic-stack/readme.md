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
helm install elasticsearch elastic/elasticsearch -f elasticsearch/values.yml -n monitoring
helm install filebeat elastic/filebeat -f filebeat/values.yml -n monitoring
helm install logstash elastic/logstash -f logstash/values.yml -n monitoring
helm install kibana elastic/kibana -f kibana/values.yml -n monitoring
```


## login credentials 
```sh
kubectl get secret elasticsearch-master-credentials -o jsonpath="{.data.username}" | base64 --decode

kubectl get secret elasticsearch-master-credentials -o jsonpath="{.data.password}" | base64 --decode
```
