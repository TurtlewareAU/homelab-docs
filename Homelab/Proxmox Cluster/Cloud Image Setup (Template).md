```bash
wget https://geo.mirror.pkgbuild.com/images/latest/Arch-Linux-x86_64-cloudimg.qcow2
```

```bash
mv Arch-Linux-x86_64-cloudimg.qcow2 arch.qcow2
```

```bash
qm set 118 --serial0 socket --vga serial0
```

```bash
qemu-img resize arch.qcow2 32G
```