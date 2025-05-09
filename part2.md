# whoami deployment

https://hub.docker.com/r/traefik/whoami

```
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: whoami
  labels:
    app: whoami
    name: whoami
spec:
  #serviceName: whoami
  replicas: 1 
  selector: 
    matchLabels:
      app: whoami
  template: 
    metadata:
      labels: 
        app: whoami
    spec:
      containers:
        - name: whoamicontainer
          image: traefik/whoami
          imagePullPolicy: Always            
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: whoami-svc
spec:
  ports:
    - name: http    
      port: 80
      protocol: TCP
  selector:
    app: whoami
EOF
```
1. View pods and service
```
kubectl get pods
kubectl get svc whoami-svc
```
2. Curl output 
```
curl $(kubectl get service/whoami-svc -o jsonpath='{.spec.clusterIP}')
```
3. Scale up 
```
kubectl scale deployment whoami --replicas=3
```
4. Get Pods
```
kubectl get pods
```

5. Clean up
```
kubectl delete deployment whoami
kubectl delete service whoami-svc
```
