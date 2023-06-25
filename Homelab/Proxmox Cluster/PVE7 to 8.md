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

```bash
sed -i -e 's/bullseye/bookworm/g' /etc/apt/sources.list.d/pve-install-repo.list

deb http://ftp.au.debian.org/debian bookworm main contrib
deb http://ftp.au.debian.org/debian bookworm-updates main contrib
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
deb http://security.debian.org bookworm-security main contrib

```

```bash
apt update
```

```bash
apt dist-upgrade
```

```bash
nano /etc/apt/sources.list.d/pve-enterprise.list.dpkg-dist
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

