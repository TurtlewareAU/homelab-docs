
### Description

Here is a walk through on how to setup all of the necessary components to have dns validation working for a given website application. Development will be either in React, Svelte, or Next Js. This will be a basic overview of the manifest files needed for deploying a new nginx based website.

### Folder Structure.

I have 2 local clusters which are currently setup as a 3 node k3s cluster setup instructions can be found here [K3S Initial Setup](K3S%20Initial%20Setup.md). These clusters have had argo cd [K3S - Argo CD setup](K3S%20-%20Argo%20CD%20setup.md) deployed to them. These manifest files will contain the entire application configuration for argo to deploy. 

The files are all the items you would need to make sure kubernetes could start, and connect your application to any internal containers, and external access via ingress / nodeports.

Below is an example of how I handle these manifest files within a given project. I create an argo folder which then has a folder for the environment each manifest will work with. Inside the environment folder is a single manifest file.

![](Pasted%20image%2020230315112504.png)


### Deployment setup

default deployment setup. Information about the name and namespace and which labels to apply.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stack-api
  namespace: default
  labels:
    app: stack-api
```

Deployment spec how many replicas to deploy to the cluster. This example will create one pod for the stack-api application.
```yaml
spec:
  replicas: 1
  selector:
    matchLabels:
      app: stack-api
  template:
    metadata:
      labels:
        app: stack-api
 ```

Pod Deployment spec. Here we define the container name, and image to select. We have also setup some readiness and liveness probes, these will make sure the container is up and running before saying everything is healthy.  These probe checks will be useful in argo to show a deployment was successfully or in error. Resources 
``` yaml
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
```

#### Manifest YAML File Complete Example


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