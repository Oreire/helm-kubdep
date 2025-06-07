---
apiVersion: v1
kind: Namespace
metadata:
  name: coweb-ns
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: coweb
  namespace: coweb-ns
spec:
  replicas: 4
  selector:
    matchLabels:
      app: coweb
  template:
    metadata:
      labels:
        app: coweb
    spec:
      containers:
        - name: coweb
          image: coweb-app:latest
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "256Mi"
