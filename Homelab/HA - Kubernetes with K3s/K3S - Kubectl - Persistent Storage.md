
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: test-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 500Mi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/mnt/kubedata/test/"
```

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 500Mi
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pv-pod
spec:
  volumes:
    - name: test-pv-storage
      persistentVolumeClaim:
        claimName: test-pv-claim
  containers:
    - name: pv-test
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: test-pv-storage
```