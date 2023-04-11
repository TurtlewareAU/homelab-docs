
```bash
sudo apt update
```

```bash
sudo apt install nfs-kernel-server
```

```bash
sudo mkdir /var/nfs/<folder> -p
```

```bash
sudo chown nobody:nogroup /var/nfs/<folder>
```
![](ubuntu-list-folder-items.png)
```bash
ls -al
```

```bash
sudo nano /etc/exports
```
![](k3s-docker-nfs-setup.png)

```bash
sudo systemctl restart nfs-kernel-server
```

```bash
sudo ufw allow ssh
```

```bash
sudo ufw allow from 10.0.44.0/24 to any port nfs
```

```bash
sudo ufw enable
```

```bash
sudo mount -t nfs 10.0.44.187:/var/nfs/docker /mnt/docker
```

```bash
sudo nano /etc/fstab
```

