### nginx deployment

https://hub.docker.com/_/nginx

```
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
    name: nginx
spec:
  #serviceName: nginx
  replicas: 1 
  selector: 
    matchLabels:
      app: nginx
  template: 
    metadata:
      labels: 
        app: nginx
    spec:
      containers:
        - name: nginxcontainer
          image: nginx
          imagePullPolicy: Always            
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
spec:
  ports:
    - name: http    
      port: 80
      protocol: TCP
  selector:
    app: nginx
EOF
```


1. View service
```
kubectl get svc nginx-svc
```
2. Curl output (replace http://10.104.xx.xx with IP address from above output)
```
curl $(kubectl get service/nginx-svc -o jsonpath='{.spec.clusterIP}')
```

3. Get new index.html file
```
wget https://raw.githubusercontent.com/uzzikie/workshop/refs/heads/master/index.html
```

4. Replace index.html file
```
kubectl cp index.html $(kubectl get pod -l app=nginx -o jsonpath="{.items[0].metadata.name}")
```
5. Curl output again.
```
curl $(kubectl get service/nginx-svc -o jsonpath='{.spec.clusterIP}')
```

6. Clean up
```
kubectl delete deployment nginx
kubectl delete service nginx-svc
```
