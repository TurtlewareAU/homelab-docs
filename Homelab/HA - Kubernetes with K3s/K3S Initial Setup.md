
### Machine Provision

I am using Ubuntu Maas to provission new virtual machines. To do so I run a new machine from terraform to network boot and retrieve information from my pfsense router, on the next server to inquire.


### Install

```bash
curl -sfL https://get.k3s.io | sh -s - server \
--token="" \
--tls-san https://dev.kube.turtleware.au --tls-san 0.0.0.0 \
--cluster-init
```

```bash
curl -sfL https://get.k3s.io | sh -s - server \
--token="" \
--tls-san https://dev.kube.turtleware.au --tls-san 0.0.0.0 \
--server https://0.0.0.0:6443
```

### Uninstall
```
/usr/local/bin/k3s-uninstall.sh
```