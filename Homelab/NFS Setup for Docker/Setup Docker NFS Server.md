Docker swarm could run a specific container on any node, to make sure we have available docker mounts for the docker containers, we will setup NFS on a server to host our mounted volumes. On each Node we will connect via NFS to this machine to allow any container on any node to read configurations in a centralised location.

Docker NFS Server - Ubuntu 20.04.4 Server Version
Docker Swarm Node 01 - Ubuntu 20.04.4 Server Version
Docker Swarm Node 02 - Ubuntu 20.04.4 Server Version
Docker Swarm Node 03 - Ubuntu 20.04.4 Server Version

Below were the steps necessary to get NFS Setup for my Docker Swarm Nodes

Will start with the Server Commands First:

1. Update the current apt repository. Then install the nfs-kernel-server

```bash
sudo apt update
sudo apt install nfs-kernel-server
```

1. Next We will want to create a useable directory for our NFS Mount
```bash
sudo mkdir /var/nfs/general -p
```

1. NFS will remap all root operations to the following user and group nobody:nogroup. We will need to add this to our recently made folder.
```bash
sudo chown nobody:nogroup /var/nfs/general
```

1. Next we will add values to the machine exports file to allow other clients to connect to our NFS machine.
```bash
sudo nano /etc/exports
```

```bash
/var/nfs/general    192.1.68.1/24(rw,sync,no_subtree_check)
```

1. Next we will restart the server to take on the new configuration
```bash
sudo systemctl restart nfs-kernel-server
```

1. We want to allow external machines access to the NFS server by opening firewalls
```bash
sudo ufw allow from 192.1.68.1/24 to any port nfs
```

```bash
sudo ufw status
```

---
Client Commands Here

1. Update client apt repository, then install the nfs-common toolset to allow the client to connect to the server

```bash
sudo apt update
sudo apt install nfs-common
```

1. Add a new folder for the share
```bash
sudo mkdir -p /nfs/general
```

1. Mounting the NFS Drive is the next step to do so we use the following
```bash
sudo mount -t nfs 192.1.68.10:/var/nfs/general /nfs/general -vvv
```

this will print out verbose information.

Now check everything is connected via
```bash
df -h
```