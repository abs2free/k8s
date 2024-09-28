## pull image
```
docker pull docker.io/bitnami/elasticsearch:8.15.2-debian-12-r0
```

## install

```
helm upgrade --install my-es oci://registry-1.docker.io/bitnamicharts/elasticsearch -f values.yaml
```
