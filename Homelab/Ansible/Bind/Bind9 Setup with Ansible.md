Using Ansible I wanted to build a primary and secondary DNS structure for my local network. I wanted two machines which will live on a ups which can keep my local network online. I currently have a pfsense router running off a ups so I wanted to keep my network online as long as possible in the event of a power outage. Here are the steps I followed to get two Bind9 machine setup and running on my network.

## Section 1 - Setup New Virtual Machines

My network stack is currently running 4 machines in a proxmox cluster. I wanted to look for something semi/fully automated when creating new virtual machines. I landed on using templates and Terraform to build new cloned vm's.

Please check this page [Bind9 Setup - Terraform VM Clone](Bind9%20Setup%20-%20Terraform%20VM%20Clone.md) for the Terraform setup of my two DNS servers. If you don't have Terraform setup, and are doing things manually. you will need to have.

- 2 Virtual/Physical machines - `I am running ubuntu 22.04 on these machines`

## Section 2 - Ansible Setup 

Here you should have 2 Virtual/physical machines setup with the same version of Ubuntu 22.04. The best thing to do is identify their IP addresses. I have mine set statically via the Terraform steps. If you did not statically set them then the best option is to:

`ip add` on a command line to grab their network address.

With our DNS machines online and running, we can start by setting up the main projects inventory.yml file. Here I use the Terraform plugin for linting my inventory files. so there are simpler ways to make this inventory file. 

## Inventory Files 

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



## Machine Setup

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


## File breakdown

- Here we will perform an `apt update` on both machines. To Make sure we have the most up to date package lists.
- The next step is to change the timezone to match closer to where I live, so logs get a better timezone for timestamps
- The final step is to perform the install of `sudo apt install bind9 bind9utils`

## TSIG generation

Transactional Signature [Wikipedia - TSIG](https://en.wikipedia.org/wiki/TSIG) will be used to validate that the Terraform script has approval to add, delete and update DNS records within the 2 virtual machines above. Below is the script we can run to get a `tsig-key` which we can assign to our zone files to allow Terraform/DNS to perform its infrastructure changes.

```yml
- name: Generate New TSIG-KEY
  hosts: leader
  become: true
  tasks:
  - name: Issue New tsig - key
    ansible.builtin.command: tsig-keygen -a hmac-sha256
    register: tsigkeygen
    changed_when: tsigkeygen != 0
  - name: Print tsig-key
    ansible.builtin.debug:
      msg: "{{ tsigkeygen.stdout }}"
```

The output will contain the following. This is a tsig-key we can register within our bind9 servers.

```bash
key "tsig-key" {
	algorithm hmac-sha256;
	secret "RvGY8nW8nVSaZe9IpFhSAh2WBdVmgWvPTpOuljJyS/c=";
};

```

## Create Options File

The named.conf.options file allows you to change the named configuration based on your need. Here I will show my file.

```bash
acl "trusted" {
        10.0.44.33;
        10.0.44.34;
        10.0.44.1;
        10.0.44.0/24;
};

options {
        directory "/var/cache/bind";
        recursion yes;
        allow-recursion { trusted; };
        listen-on { 10.0.44.33; };
        allow-transfer { 10.0.44.34; };
        forwarders {
                10.0.44.104;
                10.0.44.94;
        };
        dnssec-validation auto;
        listen-on-v6 { any; };
};
```

In my options file I set a trusted network of ip's which can query the DNS Server. Then there is the options set which I have set based on guides.

Listen on will tell the DNS server which host ip to listen to.

```bash
listen-on { 10.0.44.33; };
```

Allow transfer will allow this DNS server to transfer its configuration to the next "Slave" server.

```bash
allow-transfer { 10.0.44.34; };
```

Forwarders is a list of the next DNS Server in the trip. Here I have upstream local DNS. I do this as I want Bind9 to handle the turtleware.au domain locally only. I then upstream any other request to my pi-hole servers at 104, 94 (ad blocking). In my pi-hole machines I have upstream DNS to Cloudflare DNS for DNSSEC. 

```bash
forwarders {
  10.0.44.104;
  10.0.44.94;
};
```

## named.conf.local

The named.conf.local is where you want your name server to be the authoritative answer to DNS queries. This allows us to define our zone files. I am only going for a forward resolution.

```bash
key "tsig-key" {
	algorithm hmac-sha256;
	secret "RvGY8nW8nVSaZe9IpFhSAh2WBdVmgWvPTpOuljJyS/c=";
};

zone "turtleware.au" IN {
    type master;
    file "/var/cache/bind/custom/turtleware.au.zone";
    update-policy { grant tsig-key. zonesub any; };
    allow-transfer { 10.0.44.34; };
};
```

## Create Bind Config Leader(Ansible)

```yml
- name: Setup BIND Configurations
  hosts: leader
  become: true
  tasks:
    - name: Backup named.conf.options
      ansible.builtin.copy:
        remote_src: true
        src: "/etc/bind/named.conf.options"
        dest: "/etc/bind/named.conf.options.back"
        mode: "644"
    - name: Copy named.conf to "/etc/bind/named.conf.options"
      ansible.builtin.copy:
        src: "config/named.conf.options"
        dest: "/etc/bind/named.conf.options"
        mode: "644"
    - name: Backup named.conf.local
      ansible.builtin.copy:
        remote_src: true
        src: "/etc/bind/named.conf.local"
        dest: "/etc/bind/named.conf.local.back"
        mode: "644"
    - name: Copy named.conf to "/etc/bind/named.conf.local"
      ansible.builtin.copy:
        src: "config/named.conf.local"
        dest: "/etc/bind/named.conf.local"
        mode: "644"
    - name: Create New Zone Folder
    ansible.builtin.command: sudo mkdir /var/cache/bind/custom
        register: createfolder
        changed_when: createfolder != 0
    - name: Copy turtleware.au.zone to "/etc/bind/turtleware.au.zone"
      ansible.builtin.copy:
        src: "config/turtleware.au.zone"
        dest: "/var/cache/bind/custom/turtleware.au.zone"
        mode: "644"
    - name: Refresh bind to use new Configurations
      ansible.builtin.systemd:
        name: bind9
        state: restarted
```

## Create Bind Config Follower (Ansible)

```yml
- name: Setup BIND Configurations
  hosts: secondary
  become: true
  tasks:
    - name: Backup named.conf.options
      ansible.builtin.copy:
        remote_src: true
        src: "/etc/bind/named.conf.options"
        dest: "/etc/bind/named.conf.options.back"
        mode: "644"
    - name: Copy named.conf to "/etc/bind/named.conf.options"
      ansible.builtin.copy:
        src: "config/named.conf.options.slave"
        dest: "/etc/bind/named.conf.options"
        mode: "644"
    - name: Backup named.conf.local
      ansible.builtin.copy:
        remote_src: true
        src: "/etc/bind/named.conf.local"
        dest: "/etc/bind/named.conf.local.back"
        mode: "644"
    - name: Copy named.conf to "/etc/bind/named.conf.local"
      ansible.builtin.copy:
        src: "config/named.conf.local.slave"
        dest: "/etc/bind/named.conf.local"
        mode: "644"
    - name: Create New Zone Folder
    ansible.builtin.command: sudo mkdir /var/cache/bind/custom
        register: createfolder
        changed_when: createfolder != 0
    - name: Copy turtleware.au.zone to "/etc/bind/turtleware.au.zone"
      ansible.builtin.copy:
        src: "config/turtleware.au.zone"
        dest: "/var/cache/bind/custom/turtleware.au.zone"
        mode: "644"
    - name: Refresh bind to use new Configurations
      ansible.builtin.systemd:
        name: bind9
        state: restarted
```

Creating the `named.conf.local.slave` file with the below information. will trigger the bind server on restart to download the zone file from ns1 and use this zone file for local domain queries

```yml
zone "turtleware.au" IN {
    type slave;
    file "/var/cache/bind/custom/turtleware.au.zone";
    masters { 10.0.44.33; };
};
```

Creating the `named.conf.options.slave` file with the below structure will allow for setup of the DNS server, similar to ns1

```yml
acl "trusted" {
        10.0.44.34;
        10.0.44.33;
        10.0.44.1;
        10.0.44.0/24;
};

options {
        directory "/var/cache/bind";
        recursion yes;
        allow-recursion { trusted; };
        listen-on { 10.0.44.34; };
        forwarders {
                10.0.44.104;
                10.0.44.94;
        };
        dnssec-validation auto;
        listen-on-v6 { any; };
};
```

## How To Run

We have now come to the part where we will run our scripts and perform the work needed. Its been a lot of setup, but the good thing is when these machines are torn down and new machines spun up we can run these commands and get our environment up an running quickly.

- `ansible-playbook -i inventory.yml setup.yml` - installing bind9 and utils
- `ansible-playbook -i inventiry.yml tsig.yml` - Grab this an place it into the zone file.
- `ansible-playbook -i inventory.yml bindconfigs.yml` - setup leader configs on NS1
- `ansible-playbook -i inventory.yml bindconfigs-slave.yml` - setup the secondary NS2 machine.

From here we should be able to get results back from nslookup for our internal host names. 

```bash
turtlez in ~ λ nslookup firewall.turtleware.au
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	firewall.turtleware.au
Address: 10.0.44.1
```

```bash
turtlez in ~ λ nslookup firewall.turtleware.au 10.0.44.33
Server:		10.0.44.33
Address:	10.0.44.33#53

Name:	firewall.turtleware.au
Address: 10.0.44.1
```

```bash
turtlez in ~ λ nslookup firewall.turtleware.au 10.0.44.34
Server:		10.0.44.34
Address:	10.0.44.34#53

Name:	firewall.turtleware.au
Address: 10.0.44.1
```