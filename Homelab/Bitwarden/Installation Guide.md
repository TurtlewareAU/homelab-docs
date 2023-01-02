1.  Create a bitwarden user:
```bash
sudo adduser bitwarden
```
 
2.  Set password for bitwarden user (strong password):
   
```bash
sudo passwd bitwarden
```

3.  Create a docker group (if it doesn’t already exist):
   
```bash
sudo groupadd docker
```
   
4.  Add the bitwarden user to the docker group:
   
```bash
sudo usermod -aG docker bitwarden
```
   
5.  Create a bitwarden directory:
   
```bash
sudo mkdir /opt/bitwarden
```
   
6.  Set permissions for the `/opt/bitwarden` directory:
   
```bash
sudo chmod -R 700 /opt/bitwarden
```
   
7.  Set the bitwarden user as owner of the `/opt/bitwarden` directory:
   
```bash
sudo chown -R bitwarden:bitwarden /opt/bitwarden
```

------
```bash
curl -Lso bitwarden.sh https://go.btwrdn.co/bw-sh && chmod 700 bitwarden.sh
```

```bash
./bitwarden.sh install
```