```bash
sudo curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```

```bash
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
```

```bash
sudo apt-get update && sudo apt-get install boundary
```

```bash
curl -fsSL https://test.docker.com -o test-docker.sh
sudo sh test-docker.sh
```