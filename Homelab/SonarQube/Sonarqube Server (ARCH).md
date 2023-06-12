
## Setup Server

### pre-requisites
Installing JDK

```bash
sudo pacman -S jdk17-openjdk
```

```bash
sysctl vm.max_map_count=524288
sysctl fs.file-max=13
ulimit -n 131072
ulimit -u 8192
```