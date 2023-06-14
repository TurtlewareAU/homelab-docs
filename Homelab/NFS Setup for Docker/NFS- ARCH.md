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

```bash
sudo mount --bind /mnt/docker /var/nfs/docker
```

```bash
sudo nano /etc/exports
```

```bash
/mnt/docker   10.0.44.0/24(sync,rw)
```

```bash
sudo exportfs -arv
```