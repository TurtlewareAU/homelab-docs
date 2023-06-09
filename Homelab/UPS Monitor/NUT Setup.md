```bash
sudo apt update
```

```bash
sudo apt install nut nut-client nut-server
```

```bash
sudo nut-scanner -U
```

```bash
sudo nano /etc/nut/ups.conf
sudo mv /etc/nut/ups.conf /etc/nut/ups.conf.back
```

```bash  
pollinterval = 1
maxretry = 3

[Server]
        driver = usbhid-ups
        port = auto
        desc = Cyber Power Main UPS
        vendorid = 0764
        productid = 0501

[Network]
        driver = "usbhid-ups"
        port = "auto"
        desc = Network Router and Synology
        vendorid = 0764
        productid = 0601
        serial = GBXXXXXXX

```

```bash
sudo mv /etc/nut/upsmon.conf /etc/nut/upsmon.conf.back
sudo nano /etc/nut/upsmon.conf
```

```bash
RUN_AS_USER root

MONITOR server@localhost 1 admin <secret> master
MONITOR network@localhost 1 admin <secret> master
```

```bash
sudo mv /etc/nut/upsd.conf /etc/nut/upsd.conf.back
sudo nano /etc/nut/upsd.conf
```

```bash
LISTEN 0.0.0.0 3493
```

```bash
sudo mv /etc/nut/nut.conf /etc/nut/nut.conf.back
sudo nano /etc/nut/nut.conf
```

```bash
MODE=netserver
```

```bash
sudo mv /etc/nut/upsd.users /etc/nut/upsd.users.back
sudo nano /etc/nut/upsd.users
```

```bash
[monuser]
  password = <secret>
  admin master
```

```bash
sudo service nut-server restart
sudo systemctl restart nut-monitor
sudo upsdrvctl stop
sudo upsdrvctl start
```

```bash
sudo apt install apache2 nut-cgi
```

```bash
sudo mv /etc/nut/hosts.conf /etc/nut/hosts.conf.back
sudo nano /etc/nut/hosts.conf
```