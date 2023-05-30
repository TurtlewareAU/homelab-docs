Using Ansible I wanted to build a primary and secondary DNS structure for my local network. I wanted two machines which will live on a ups which can keep my local network online. I currently have a pfsense router running off a ups so I wanted to keep my network online as long as possible in the event of a power outage. Here are the steps I followed to get two Bind9 machine setup and running on my network.

## Section 1 - Setup New Virtual Machines

My network stack is currently running 4 machines in a proxmox cluster. I wanted to look for something semi/fully automated when creating new virtual machines. I landed on using templates and Terraform to build new cloned vm's.

Please check this page [Bind9 Setup - Terraform VM Clone](Bind9%20Setup%20-%20Terraform%20VM%20Clone.md) for the Terraform setup of my two DNS servers. If you don't have Terraform setup, and are doing things manually. you will need to have.

- 2 Virtual/Physical machines - `I am running ubuntu 22.04 on these machines`

## Section 2 - Ansible Setup 

Here you should have 2 Virtual/physical machines setup with the same version of Ubuntu 22.04. The best thing to do is identify their IP addresses. I have mine set statically via the Terraform steps. If you did not statically set them then the best option is to:

`ip add` on a command line to grab their network address.

With our DNS machines online and running, we can start by setting up the main projects inventory.yml file. Here I use the Terraform plugin for linting my inventory files. so there are simpler ways to make this inventory file. 

---
#### Inventory Files 
My Inventory file valid selections for ansible hosts are [all, leader, secondary] this will allow a specific script to target, one or all of the hosts.

```yml
all:
  hosts:
    children:
      leader:
        hosts:
          dns01:
            ansible_host: 10.0.44.33
            ansible_user: twadmin
      secondary:
        hosts:
          dns02:
            ansible_host: 10.0.44.34
            ansible_user: twadmin
```

Example of a simple inventory file. 
```yml
[leader]
x.x.x.x

[secondary]
x.x.x.x
```


---
#### Machine Setup

For the machine setup I have only added to the script the bare minimum to get the machine operation as a bind9 server. I will eventually add a few things extra such as the qemu guest agent.

The script:
```yml
- name: Bind Server Install
  hosts: all
  become: true
  tasks:
    - name: Update Packages
      ansible.builtin.apt:
        update-cache: true
    - name: Set Date Time
      community.general.timezone:
        name: Australia/Sydney
- name: Install Default Server APT Packages
ansible.builtin.apt:
package:
- bind9
- bind9utils
```