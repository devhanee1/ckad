# Ingress
- Noramlly nodePort or using LoadBalancer is not easy and complicated and not scalable
- So, ingress is came out. It's not native resource of k8s
- It's another Controller


## Order to use
1. Deploy (reverse proxy, like Nginx, HAProxy, Traefik)
-> Ingress controller

2. Configure(route traffic)
- Resource
- ingress resource
- not want user type ip address manually

## Ingress controller sample 
#### Controller (Pod)
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
    name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: query.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
      args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration
```

#### ConfigMap
```
kind: ConfigMap
apiVersion: v1
metadata:
  name: nginx-controller
```

#### Service
- nginx-controller is also kind of Pod. So service is needed to expose the Pod
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https

  selector:
    name: nginx-ingress (참고로 service의 selector는 deployment 가 아닌 pod 의 label 을 명시함. )
```
- Permission should be configured.

## Ingress Configure

- Domain is same, request traffic is routed by path 
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - http:
      paths:
      - path: /wear
        pathType: Prefix
        backend:
          service:
            name: wear-service
            port:
              number: 80
      - path: /watch
        pathType: Prefix
        backend:
          service:
            name: watch-service
            port:
              number: 80
```