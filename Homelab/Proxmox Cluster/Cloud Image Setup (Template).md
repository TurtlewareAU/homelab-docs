Create a new empty VM

Setup Cloud Init Settings

Perform the following to download and import
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

```bash
qm importdisk 118 arch.qcow2 vm
```

Go to hardward,

Click edit on unused disk
check ssd emulation and discard

Edit Boot Order to enable disk, and move to second in the list