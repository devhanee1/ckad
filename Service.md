# Service

```
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
    type: ClusterIP
    ports:
    - targetPort: 80
      port: 80
    selector:
      app: myapp
      type: back-end

```


## Type of Service : ClusterIP, NodePort, LoadBalancer
### ClusterIP
- default type
- have port, targetPort

### NodePort
- Can be exposed to outside, at the edge of Node
- have nodePort, port, targetPort
  - nodePort: for accessing the node from outside
  - port: service port
  - targetPort: the port of Pod

### loadBalancer
- General Scenario for product: On-prem
  - Set DNS to point the ip addr of node 
  - User can access with dns and port number(high number above 30,000)
  - using Proxy server (to avoid high port number)
  => Need to set DNS whenever new service is added (Just work , not fancy way)

- Cloud Service 
  - Create service with LoadBalancer type (almost same as NodePort)
  - After that k8s send a request to GCP

  - automatically deploy a load balancer (real load balancer)
  => Same as above, but happend automatically by GCP
  => BOTH cost a lot