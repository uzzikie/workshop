# SMU .Hack X CYC Workshop

https://killercoda.com/playgrounds/scenario/kubernetes

---
### kubctl commands

1. View nodes
```
kubectl get nodes
```
2. View nodes with more information
```
kubectl get nodes -o wide
```
3. View pods
```
kubectl get pods
```
4. View pods in all namespaces
```
kubectl get pods --all-namespaces
```
5. View services
```
kubectl get svc
```

---
### whoami deployment

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
1. View service
```
kubectl get svc whoami-svc
```
2. Curl output 
```
curl http://10.104.xx.xx
```
3. Scale up 
```
kubectl scale deployment whoami --replicas=3
```

4. Clean up
```
kubectl delete deployment whoami
kubectl delete service whoami-svc
```


---
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
2. Curl output 
```
curl http://10.104.xx.xx
```
3. View pod
```
kubectl get pods
```
4. Enter pod
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


10. Clean up
```
kubectl delete deployment nginx
kubectl delete service nginx-svc
```
