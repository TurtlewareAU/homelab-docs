
## Sonarqube Database Setup

```bash
sudo pacman -Syy
```

```bash
sudo pacman -S postgres
```

```bash
sudo pacmans -S postgresql
```

```bash
sudo -u postgres initdb --locale en_US.UTF-8 -D /var/lib/postgres/data
```

```bash
systemctl start postgresql
systemctl enable postgresql
systemctl status postgresql
```

```bash
su - postgres
```