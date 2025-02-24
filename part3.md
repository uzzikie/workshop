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
curl http://10.104.xx.xx
```
3. View pod
```
kubectl get pods
```
4. Enter pod (replace nginx-5b5757c7b7-s6q2x with pod name from above output)
```
kubectl exec --stdin --tty nginx-5b5757c7b7-s6q2x -- /bin/bash
```  
5. Change directory
```
cd /usr/share/nginx/html
```

6. View index.html contents
```
cat index.html
```

7. Exit pod
```
exit
```

8. Get new index.html file
```
wget https://raw.githubusercontent.com/uzzikie/workshop/refs/heads/master/index.html
```

9. Replace index.html file
```
kubectl cp index.html default/nginx-5b5757c7b7-vkc2z:/usr/share/nginx/html
```
10. Curl output again.
```
curl http://10.104.xx.xx
```

11. Clean up
```
kubectl delete deployment nginx
kubectl delete service nginx-svc
```
