### 2. Ingress Resources. Name Based Virtual Hosting

**What is Ingress Controller?**

Just a reminder from task #1. It’s a cluster-wide web service, which gets http/https requests from customers and routes traffic inside the cluster to corresponding Pods.

![](../img/0-ingress.png)
**What is Ingress (Resource or Rule)?**

It’s a traffic routing configuration - this is how our Ingress Controller knows what request to sent to which pod.

**How to Configure Ingress Resource?**
Name Based Virtual Hosting:

**For k8s prior v1.19:**

```yaml
apiVersion: networking.k8s.io/v1beta
kind: Ingress
metadata:
  name: name-virtual-host-ingress
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - backend:
          serviceName: foo-svc
          servicePort: 80
```          

**For k8s v1.19+:**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: name-virtual-host-ingress
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: foo-svc
            port:
              number: 80
```            

`networking.k8s.io/v1beta1` is deprecated in v1.19+, unavailable in v1.22+

Use `networking.k8s.io/v1` for k8s v1.19+

With this configuration http request to `foo.bar.com` should be routed to `foo-svc` service.

To see ingress resource (e.g. `foo-ingress`), use the command as following:
Here’s an example of Ingress resource

With this configuration http request to foo.bar.com should be routed to foo-svc service.

To see ingress resource (e.g. foo-ingress), use the command as following:

**Describing Ingress Resource**

```yaml
kubectl describe ingress foo-ingress
Name:             foo-ingress
Namespace:        default
Address:          ...
Default backend:  default-http-backend:80 (...)
Rules:
  Host         Path  Backends
  ----         ----  --------
  foo.bar.com  
                  foo-svc:80
...
```

**Where Ingress Resouce Should be Created?**

**At least following ideas can come to mind:**

- Keep Ingress Resource in ingress-nginx namespace - close to Ingress Controller
- Keep all Ingress Resources in separate (dedicated) namespace
- Keep application related Ingress Resource in the same namespace where application workloads are created

**The best choice is option #3 - at least because of the following considerations:**

- It won’t require any additional privileges for end-user to manage other namespaces
- Easier Service Domain Names resolution (Ingress Resource -> Service) within the same namespace
- Eventual Consistency of entire application stack: all resources are running in the same namespace, and ease of deployment approach

#### Task

There’re **aqua, maroon** and **olive** deployments created in default `namespace`.

Please do the following:

- Create services aqua-svc, maroon-svc and olive-svc for respective deployments

- Create (3) **Ingress Resources** which forward incoming traffic to respective pods:

- `aqua-ingress:`
```shel
    domain name: aqua.k8slab.playpit.net
    service name: aqua-svc
    service port: 80
```
- `maroon-ingress`
```shel
    domain name: maroon.k8slab.playpit.net
    service name: maroon-svc
    service port: 80
```
  - `olive-ingress`
```shell
    domain name: olive.k8slab.playpit.net
    service name: olive-svc
    service port: 80
```

**Check your services in browser:**

- aqua.k8slab.playpit.net
- maroon.k8slab.playpit.net
- olive.k8slab.playpit.net

**Checking**
```shell
kubectl get ing
NAME             CLASS    HOSTS                       ADDRESS   PORTS   AGE
aqua-ingress     <none>   aqua.k8slab.playpit.net               80      6s
maroon-ingress   <none>   maroon.k8slab.playpit.net             80      6s
olive-ingress    <none>   olive.k8slab.playpit.net              80      6s
```

**Sollution**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aqua-svc
spec:
  selector:
    app: aqua
  ports:
  - protocol: TCP
    port: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: aqua-ingress
spec:
  rules:
  - host: aqua.k8slab.playpit.net
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: aqua-svc
            port: 
              number: 80
```
**Documentation:**

- https://kubernetes.io/docs/concepts/services-networking/ingress/
- https://cloud.google.com/kubernetes-engine/docs/concepts/ingress
