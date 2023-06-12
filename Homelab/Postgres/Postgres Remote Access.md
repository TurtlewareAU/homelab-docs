### Setup Remote Access

```bash
sudo nano /var/lib/postgres/data/postgresql.conf

Change Listen='localhost' -> listen='0.0.0.0'
```

```bash
sudo nano /var/lib/postgres/data/pg_hba.conf

host    all             all             127.0.0.1/32            trust
to VV
host    all             all             all            trust
```

```bash
sudo systemctl restart postgresql
```