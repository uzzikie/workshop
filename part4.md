### mariadb deployment

https://hub.docker.com/_/mariadb

```
kubectl apply -f - <<EOF
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mariadb
  labels:
    app: mariadb
    name: mariadb
spec:
  replicas: 1 
  selector: 
    matchLabels:
      app: mariadb
  template: 
    metadata:
      labels: 
        app: mariadb
    spec:
      securityContext:
        runAsUser: 999
        runAsGroup: 999
        fsGroup: 999
      containers:
        - name: mariadbcontainer
          image: mariadb:11
          env:
            - name: TZ
              value: Asia/Singapore
            - name: MARIADB_ROOT_PASSWORD
              value: rootpassword    
            - name: MARIADB_USER
              value: wordpress
            - name: MARIADB_PASSWORD
              value: userpassword    
            - name: MARIADB_DATABASE
              value: wordpress_db                                                     
          ports:
            - name: mariadb-port
              containerPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: mariadb-svc
spec:
  ports:
    - name: mariadb-port    
      port: 3306
      protocol: TCP
  selector:
    app: mariadb    
EOF
```

1. View Pods
```
kubectl get pods
```

2. View Logs (replace mariadb-848c8f4487-nbz5x with pod name from above output)
```
kubectl logs $(kubectl get pod -l app=mariadb -o jsonpath="{.items[0].metadata.name}")
```

3. Enter pod
```
kubectl exec --stdin --tty $(kubectl get pod -l app=mariadb -o jsonpath="{.items[0].metadata.name}") -- /bin/bash
```

4. Login to mariadb
```
mariadb -u root -prootpassword
```

5. View all databases
```
show databases;
```

6. Exit mariadb
```
exit;
```

7. Exit pod
```
exit
```


