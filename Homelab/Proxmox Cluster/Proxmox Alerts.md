```bash
apt update
```

```bash
apt install -y libsasl2-modules mailutils
```

```bash
echo "smtp.ventraip.email homelab@turtlez.au:<password>" > /etc/postfix/sasl_passwd
```

```bash
postmap hash:/etc/postfix/sasl_passwd
```
