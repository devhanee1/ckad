# For Docker
```
$ docker run ubuntu
```

# For K8s
```
$ k run nginx --image nginx
$ k run redis --image=redis --dry-run=client -o yaml
$ k scale --replicas=6 -f replicaset-definition.yaml
$ k scale --replicas=5 replicaset myapp-replicaset
$ k get replicaset
$ k delete replicaset myapp-replicaset
$ k config set-context $(k config current-context) --namespace=dev
```

# Pod
```
$ k run nginx --image=nginx
$ k run nginx --image=nginx --dry-run=client -o yaml
```

# Deployment
```
$ k create deployment --image=nginx nginx
$ k create deployment --image=nginx nginx --dry-run=client -o yaml
$ k create deployment --image=nginx --replicas=4
$ k create deployment nginx--image=nginx replicas=4
$ k create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
$ k scale deployment nginx --replicas=4
```


# Service
### create a service named redis-service of type ClusterIP to expose pod redis on port 6379
```
$ k expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
$ k create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```
=> assume that selector as app=redis
 
### create a service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes
```
$ k expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
```
=> assume the selector as app=nginx
   