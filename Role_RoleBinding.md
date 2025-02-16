# Role
## Role 
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"']
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

## Role with specific permission

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"']
  resourceNames: ["blue", "orange"]
```

# Role Binding
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subejcts:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```


## Check Access
```
$ k auth can-i create deployments
$ k auth can-i delete nodes

$ k auth can-i create deployments --as dev-user
$ k auth can-i create pods --as dev-user
$ k auth can-i create pods --as dev-user --namespace test
```