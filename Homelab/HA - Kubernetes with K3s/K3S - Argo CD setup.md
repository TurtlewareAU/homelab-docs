
### Setup
---

To perform these steps you will need to have a working kubernetes cluster. I am running this on K3S, in HA mode with three nodes. I am running these on the first node, the node where I issued the first K3S download. This node is the Master. 


#### Kubernetes Namespace
---

Firstly start by creating the argocd namespace inside of the container

```bash
kubectl create namespace argocd
```

Verify by : `kubectl get ns`
![](k3s-get-namespace.png)

You will see the newly created namespace is set.


#### Apply argo manifest (Install)
---
Here we will issue and apply command on the argo install.yml. This file will change, check the following link for the most up to date scripts. https://argo-cd.readthedocs.io/en/release-1.8/getting_started/

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

After this has been applied. We can check what is running. `kubectl get all -n argocd` lets list all the items we have in the argocd namespace.

![](k3s-get-all-namespace-argocd.png)


#### Setup External Access
---

Here we want to patch our server so we can get to it via the NodePorts assigned. This will allow us to get access to the dashboard/application from any ip on our network.

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```


##### Grab the initial password
---

The last step when everything is up and running is to get the admin default password. This can be done by issuing the following command. This will print to the console the username and password.

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

With all the above steps. Head to your Master machines ip:port and you will see the Argocd login page.

`Note - IP is the ip address of the node you were running the above commands on.`
`Note - PORT can be found in the kubectl get all -n argocd - service/argo-server` will show two ports, 80:30826, and 443:30872 the 30872 is the port you want to use.  

![](k3s-argocd-login.png)