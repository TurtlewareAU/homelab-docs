
### Machine Provision

I am using Ubuntu Maas to provission new virtual machines. To do so I run a new machine from terraform to network boot and retrieve information from my pfsense router, on the next server to inquire.


### Install

```bash
curl -sfL https://get.k3s.io | sh -s - server \
--token="" \
--tls-san https://dev.kube.turtleware.au --tls-san 10.0.44.144 \
--cluster-init
```
```



### Uninstall
```
/usr/local/bin/k3s-uninstall.sh
```