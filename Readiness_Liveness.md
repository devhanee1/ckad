# Readiness and Liveness Probes
- Readiness: Checking the status of Pod before start
- Liveness: Checking during the execution of Pod

## Readiness 
```
spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
      - containerPort: 8080
    readinessProbe:
      httpGet:
        path: /api/ready
        port: 8080
      tcpSocket:
        port: 3306
      exec:
        command:
          - cat
          - /app/is_ready 

      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 8
```

## Liveness 
```
spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
      - containerPort: 8080
    livenessProbe:
      httpGet:
        path: /api/ready
        port: 8080
      tcpSocket:
        port: 3306
      exec:
        command:
          - cat
          - /app/is_ready 

      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 8
```