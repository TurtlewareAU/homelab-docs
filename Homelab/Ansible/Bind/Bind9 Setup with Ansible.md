Using Ansible I wanted to build a primary and secondary DNS structure for my local network. I wanted two machines which will live on a ups which can keep my local network online. I currently have a pfsense router running off a ups so I wanted to keep my network online as long as possible in the event of a power outage. Here are the steps I followed to get two Bind9 machine setup and running on my network.

## Section 1 - Setup New Virtual Machines

My network stack is currently running 4 machines in a proxmox cluster. I wanted to look for something semi/fully automated when creating new virtual machines. I landed on using templates and Terraform to build new cloned vm's.

Please check this page <> for the Terraform setup of my two DNS servers. If you don't have Terraform setup, and are doing things manually. you will need to have.

- 2 Virtual/Physical machines - `I am running ubuntu 22.04 on these machines`
- 