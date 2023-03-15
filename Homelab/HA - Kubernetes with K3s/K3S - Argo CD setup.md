
### Setup

To perform these steps you will need to have a working kubernetes cluster. I am running this on K3S, in HA mode with three nodes. I am running these on the first node, the node where I issued the first K3S download. This node is the Master. 

Firstly start by creating the argocd namespace inside of the container

```bash
kubectl create namespace argocd
```

Verify by : `ku`

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

