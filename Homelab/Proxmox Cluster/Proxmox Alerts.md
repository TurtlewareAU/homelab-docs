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

```bash
chmod 600 /etc/postfix/sasl_passwd
```

```bash
postfix reload
```

```bash
apt install postfix-pcre
```

```bash
nano /etc/postfix/smtp_header_checks
```

```bash
postmap hash:/etc/postfix/smtp_header_checks
```

```bash
postfix reload
```