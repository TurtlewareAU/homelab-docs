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
sudo nano /et
```