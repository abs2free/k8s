# Set up Ingress on Minikube with the NGINX Ingress Controller
## Enable the Ingress controller
```sh
minikube addons enable ingress
```

## Verify that the NGINX Ingress controller is running

```sh
kubectl get pods -n ingress-nginx
```

The output is similar to:

```
NAME                                        READY   STATUS      RESTARTS    AGE
ingress-nginx-admission-create-g9g49        0/1     Completed   0          11m
ingress-nginx-admission-patch-rqp78         0/1     Completed   1          11m
ingress-nginx-controller-59b45fb494-26npt   1/1     Running     0          11m
```

## Example
### Deploy a hello, world app
1. Create a Deployment using the following command
```sh
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0
```


2. Expose the Deployment
```sh
kubectl expose deployment web --type=NodePort --port=8080
```

3. Visit the Service via NodePort, using the minikube service command
```sh
minikube service web --url
```

### Create an Ingress
1. Create example-ingress.yaml
```sh
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  ingressClassName: nginx
  rules:
    - host: hello-world.example
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 8080
```

2. Create the Ingress object by running the following command
```sh
kubectl apply -f https://k8s.io/examples/service/networking/example-ingress.yaml
```

3. Verify the IP address is set
```sh
kubectl get ingress
```

4. Verify that the Ingress controller is directing traffic
```sh
minikube tunnel


curl --resolve "hello-world.example:80:127.0.0.1" -i http://hello-world.example
```

5. Optionally, you can also visit hello-world.example from your browser.

Add a line to the bottom of the /etc/hosts file on your computer (you will need administrator access):
```sh
127.0.0.1 hello-world.example
```
