# Networking

- In the networking, setting 2 things
  - Select pod
  - Giveing detail for policy

## Network Policy 
### Example 1
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db

  policyTypes:
  - Ingress
  
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
      namespaceSelector:
        matchLabels:
          name: prod
    - ipBLock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 3306

  egress:
  - to:
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 80

```
1. Set network to pod having label <role:db>
2. Two type of network (can use both)
  - Ingress -> incoming traffic
  - egress -> outgoing traffic 
3. Specify each network policy
  - Ingress
    - Pod (with namespace) or ipBlock with Port
  - Egress

### Example 2
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```