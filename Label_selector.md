# Label & Selector

## Pod with labels
```
apiVersion: v1
kind: Pod
metadata:
  name: sample-webapp
  labels:
    app: App1
    function: front-end
```
## ReplicatSet with labels
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webap
spec:
  replicas: 3
  selector: 
    matchLabels:
      app: App1
  template:
    metadata:
      labels:
        app: App1
        function: front-end
    spec:
      ...
```

## Select pod using label
```
k get pods --selector app=App1
```

# Rolling update & rollback

## Commands related with rolling
```
$ k create -f deploy.yaml
$ k get deployments
$ k apply -f deploy.yaml 
$ k set image deployment/myapp-deployment nginx=nginx:1.9.1
$ k rollout status deployment/myapp-deployment
$ k rollout history deployment/myapp-deployment
$ k rollout undo deployment/myapp-deployment
```
