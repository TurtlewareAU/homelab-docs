install bind on the servers

```bash
ansible-playbook -i inventory.yml setup.yml
```




setup a new `tsig-key.yml`

```bash
ansible-playbook -i inventory.yml tsig.yml
```


```bash
key "tsig-key" {
        algorithm hmac-sha256;
        secret "/w5ByTflpaiOzgU2j9JQtZtkWx8Zenk7k0yP6u2UL4A=";
};

acl internal {
    192.168.1.0/24;
    192.168/16;
};

options {
    forwarders {
        192.168.1.104;
        192.168.1.94;
    };
    allow-query { internal; };
};

zone "turtleware.au" IN {
    type master;
    file "/etc/bind/turtleware.au.zone";
    update-policy { grant tsig-key zonesub any; };
};
```

Create the necessary zone files.

```bash

```