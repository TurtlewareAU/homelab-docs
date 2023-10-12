
```bash
docker volume create portainer_data
```

```bash
docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data -v ~/.acme.sh/portainer.turtleware.au_ecc/:/certs portainer/portainer-ce:latest --sslcert /certs/portainer.turtleware.au.cer --sslkey /certs/portainer.turtleware.au.key
```