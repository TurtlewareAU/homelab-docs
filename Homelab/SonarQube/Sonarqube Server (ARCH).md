
## Setup Server

### pre-requisites
Installing JDK

```bash
sudo pacman -S jdk17-openjdk
```

```bash
sudo sysctl vm.max_map_count=524288
sudo sysctl fs.file-max=131072
ulimit -n 131072
```

```bash 
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.0.0.68432.zip
```

```bash
sudo unzip sonarqube.zip
```

```bash
sudo mkdir /sonarqube
```

```bash
sudo cp sonarqube-10.0.0.68432 /so
```