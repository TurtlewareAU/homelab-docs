install bind on the servers

```bash
ansible-playbook -i inventory.yml setup.yml
```


setup a new `tsig-key.yml`

```bash
ansible-playbook -i inventory.yml tsig.yml
```

create a new file config/tsig.key
```bash
key "tsig-key" {
        algorithm hmac-sha256;
        secret "/w5ByTflpaiOzgU2j9JQtZtkWx8Zenk7k0yP6u2UL4A=";
};
```

create a new file config/named.conf.local

```bash
include "/etc/bind/tsig.key"

zone "turtleware.au" IN {
    type master;
    file "/var/cache/bind/custom/turtleware.au.zone";
    update-policy { grant tsig-key. zonesub any; };
    allow-transfer { 10.0.44.33; };
};

```

create a new file config/named.conf.options
```bash
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
        allow-transfer { none; };
        forwarders {
                10.0.44.104;
                10.0.44.94;
        };
        dnssec-validation auto;
        listen-on-v6 { any; };
};

```

create new folder in var/cache
