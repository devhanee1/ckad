# Taint & Toleration

## Set node with taint
```
k taint nodes node-name key=value:taint-effect
```

### taint-effect
- NoSchedule
- PreferNoSchedule
- NoExecute

## Pod with toleration
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
  
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"

```

# Node Selector & Node Affinity

## Node Selector
```
$ k label nodes <node-name> <label-key>=<label-value>
$ k label nodes node-1 size=Large

```

## Pod with nodeSelector
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx

  nodeSelector:
    size: Large 
```
node selector 는 단순히 label 이 매칭되는 경우만 적용 가능함
그런데 만약 즉 예를 들어 Large 혹은 Medium 일 때 해당 노드에 할당하라 와 같은 요구사항은 어떻게?
그래서 node affinity 가 필요함 

## Node Affinity
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx

  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values:
            - Large
            - Medium
```

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx

  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: NotIn
            values:
            - Small

```

- requiredDuringSchedulingIgnoreDuringExecution
- requiredDuringSchedulingIgnoredDuringExecution