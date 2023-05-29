Here I will show you have to build two new virtual machines with Proxmox as your hypervisor and an already built ubuntu 22.04 template. If you need to build a new proxmox clone I followed Learn Linux TV's video : [Ubuntu 22.04 Template](https://www.youtube.com/watch?v=MJgIm03Jxdo&t=1180s) The only change I made was the cloud image. For my use the minimal image did not contain modules for Docker Networking (Swarm did not work well). I instead selected the current release image.

---
### Defining Provider File

The first step of my Terraform projects is to create a new folder for the system I am looking to spin up a new machine for. Here I will create a new folder `bind`.

In the folder I will create a new provider file `provider.tf` and here I will store the provider necessary to build and clone a new virtual machine. The provider file 
