### Arch Setup

```bash
sudo pacman -S nfs-utils
```

```bash
sudo systemctl start nfs-server.service
```

```bash
sudo systemctl enable nfs-server.service
```

```bash
sudo mkdir -p /var/nfs/docker
sudo mkdir -p /mnt/docker
```