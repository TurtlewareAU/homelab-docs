
### Description

Here is a walk through on how to setup all of the necessary components to have dns validation working for a given website application. Development will be either in React, Svelte, or Next Js. This will be a basic overview of the manifest files needed for deploying a new nginx based website.

### Folder Structure.

I have 2 local clusters which are currently setup as a 3 node k3s cluster [K3S Initial Setup](K3S%20Initial%20Setup.md)


#### Manifest YAML Files


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stack-api
  namespace: default
  labels:
    app: stack-api
spec:
  replicas: 1
  selector:
    matchLabels:
      app: stack-api
  template:
    metadata:
      labels:
        app: stack-api
    spec:
      containers:
      - name: stack-api
        image: turtlez/stackapi:1.0.0
        ports:
        - containerPort: 4400
        readinessProbe:
          initialDelaySeconds: 5
          periodSeconds: 20
          httpGet:
            path: /ping
            port: 4400
        livenessProbe:
          initialDelaySeconds: 5
          periodSeconds: 20
          httpGet:
            path: /ping
            port: 4400
        securityContext:
          readOnlyRootFilesystem: true
          allowPrivilegeEscalation: false
        resources:
          limits:
            memory: "128Mi"
          requests:
            memory: "128Mi"
        env:
        - name: MONGO_DB
          valueFrom:
            configMapKeyRef:
              name: mongo-connection-configmap
              key: database_url
      imagePullSecrets:
      - name: regcred
---
apiVersion: v1
kind: Service
metadata:
  name: stackapi-service
  namespace: default
spec:
  selector:
    app: stack-api
  ports:
    - protocol: TCP
      port: 4400
      targetPort: 4400
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: stack-ingress
  namespace: default
spec:
  rules:
  - host: api.stack.dev.kube
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: stackapi-service
            port:
              number: 4400
```