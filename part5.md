### wordpress deployment

https://hub.docker.com/_/wordpress

```
kubectl apply -f - <<EOF
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  labels:
    app: wordpress
    name: wordpress
spec:
  replicas: 1 
  selector: 
    matchLabels:
      app: wordpress
  template: 
    metadata:
      labels: 
        app: wordpress
    spec:    
      containers:
        - name: wordpresscontainer
          image: wordpress:php8.3
          env:
            - name: WORDPRESS_DB_HOST
              value: mariadb-svc.default.svc.cluster.local
            - name: WORDPRESS_DB_USER
              value: wordpress    
            - name: WORDPRESS_DB_PASSWORD
              value: userpassword  
            - name: WORDPRESS_DB_NAME
              value: wordpress_db             
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: wordpress-svc
spec:
  ports:
    - name: http    
      port: 80
      protocol: TCP
  selector:
    app: wordpress    
EOF
```

1. View Service
```
kubectl get svc
```

2. Curl output (replace http://10.104.xx.xx with IP address from above output)
```
curl -v http://10.104.xx.xx
```

3. Curl wordpress install page (replace http://10.104.xx.xx with IP address from above output)
```
curl http://10.104.xx.xx/wp-admin/install.php
```

