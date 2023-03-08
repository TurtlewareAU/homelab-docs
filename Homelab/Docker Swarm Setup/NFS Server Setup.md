
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
![](Pasted%20image%2020230309094835.png)
```bash
ls -al
```

```bash
sudo nano /etc/exports
```
![](Pasted%20image%2020230309095030.png)

```bash
sudo systemctl restart nfs-kernel-server
```

