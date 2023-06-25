```bash
nano /etc/apt/sources.list
```

```bash
nano /etc/apt/sources.list.d/pve-enterprise.list
```

```bash
apt update
```

```bash
apt upgrade
```

Refresh Should be 7.4.15 Version

## PVE7to8
this will tell you when you are ready to move to version 8

Proxmox node
```bash
apt install proxmox-ve
```

Qdevice
```bash
sudo apt install corosync-qdevice corosync-qnetd
```

```bash
sudo nano /etc/ssh/sshd_config
allow root ssh login
sudo systemctl restart sshd
```

Proxmox Nodes
```bash
apt install corosync-qdevice
```

On One Node
```bash
pvecm qdevice setup 10.0.44.250 -f
```

```bash

```