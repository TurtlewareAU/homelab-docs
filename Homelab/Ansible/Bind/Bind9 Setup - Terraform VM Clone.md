Here I will show you have to build two new virtual machines with Proxmox as your hypervisor and an already built Ubuntu 22.04 template. If you need to build a new Proxmox clone I followed Learn Linux TV's video : [Ubuntu 22.04 Template](https://www.youtube.com/watch?v=MJgIm03Jxdo&t=1180s) The only change I made was the cloud image. For my use the minimal image did not contain modules for Docker Networking (Swarm did not work well). I instead selected the current release image.

---
### Defining Provider File

The first step of my Terraform projects is to create a new folder for the system I am looking to spin up a new machine for. Here I will create a new folder `bind`.

In the folder I will create a new provider file `provider.tf` and here I will store the provider necessary to build and clone a new virtual machine. The provider file looks like


```hcl
terraform {
  required_version = ">=1.1.0"
  required_providers {
    proxmox = {
      source = "telmate/proxmox"
      version = "2.9.11"
    }
  }
} 

provider "proxmox" {
pm_api_url = "https://XX.XXX.XXX.XX:8006/api2/json"
pm_api_token_id = "xxx@xxx!xxxxxxx"
pm_api_token_secret = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
pm_tls_insecure = true

}
```

After this I like to create the necessary machine `.tf` files for this I will create a primary and a secondary machine.

- First machine file is `dns01.tf`
- Second machine file is `dns02.tf`

Basic machine structure for both files:
```hcl
resource "proxmox_vm_qemu" "<resourcename>" {
  target_node = "<ProxmoxHostName>"
  name        = "<ProxmoxGuestName>"
  desc        = "<ProxmoxGuestDescription>"
  onboot      = true
  full_clone  = true
  boot        = "order=ide2;scsi0;net0;ide0"
  clone       = "<TemplateName>"
  agent       = 0
  cores       = 1
  sockets     = 1
  cpu         = "host"
  memory      = 2048
  ipconfig0   = "gw=10.0.44.1,ip=10.0.44.33/24"
  
  vga {
    type = "serial0"
  }
  
  network {
    bridge = "vmbr0"
    model = "virtio"
  }
  
  os_type = "ubuntu"
}
```


#### Information about the above resource structure.
`resource "proxmox_vm_qemu" "<resourcename>"` defines our resource type and name.
`target_node = "<ProxmoxHostName>"` defines the Proxmox node we want our virtual machine to be cloned to.
`name        = "<ProxmoxGuestName>"` defines the Proxmox guest name, Shown in the GUI of proxmox next to the vm id
`desc        = "<ProxmoxGuestDescription>"` defines the description text, can be viewed in the GUI when looking at the specific machine.
`onboot      = true` sets the start on boot to yes 
`full_clone  = true` sets the machine to be a full clone
`boot        = "order=ide2;scsi0;net0;ide0"` sets the virtual machine boot order.
`clone       = "<TemplateName>"` This is the template on the node we want to use.
`agent       = 0` sets the agent allowed flag to 0, my template does not have it installed by default, and so need is set to off for Terraform to succeed, and not destroy the vm.
`cores       = 1` sets the cpu cores for the machine
`sockets     = 1` sets how many of the underlying sockets can be used
`cpu         = "host"` sets the cpu generation type
`memory      = 2048` sets the vm memory (2 - GiB)
`ipconfig0   = "gw=10.0.44.1,ip=10.0.44.33/24"` sets the gateway and static IP address for the virtual machine.

#### Information about the VGA section. 
This section sets the display output for Proxmox, due to the cloud image output not matching Proxmox implementation.

`type = "serial0"` sets the vga display to the serial0 output.
