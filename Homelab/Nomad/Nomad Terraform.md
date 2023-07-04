```yml
resource "proxmox_vm_qemu" "nomad_carpo" {
target_node = "uranus"
name = "NOMAD-SERVER"
desc = "NOMAD-SERVER For Turtleware"
onboot = true
full_clone = true
boot = "order=ide2;scsi0;net0;ide0"
clone = "deb-template"
agent = 0
cores = 2
sockets = 1
cpu = "host"
memory = 4096
ipconfig0 = "gw=10.0.44.1,ip=10.0.44.90/24"
vga {
type = "serial0"
}
network {
bridge = "vmbr0"
model = "virtio"
}

os_type = "ubuntu"

}

  

resource "dns_a_record_set" "nomad_carpo" {

zone = "turtleware.au."

name = "carpo"

addresses = ["10.0.44.90"]

ttl = 300

}
```