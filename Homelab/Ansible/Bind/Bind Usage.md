install bind on the servers

```bash
ansible-playbook -i inventory.yml setup.yml
```

setup a new tsig-key.yml

```bash
ansible-playbook -i inventory.yml tsig.yml
```

take the tsig-key, and append to the config/named.conf file under the ansible project folder.

```bash
key "tsig-key" {
        algorithm hmac-sha256;
        secret "/w5ByTflpaiOzgU2j9JQtZtkWx8Zenk7k0yP6u2UL4A=";
};

acl internal {
    192.0.44.0/24;
    192.0/16;
};

options {
    forwarders {
        10.0.44.104;
        10.0.44.94;
    };
    allow-query { internal; };
};

zone "turtleware.au" IN {
    type master;
    file "/etc/bind/turtleware.au.zone";
    update-policy { grant tsig-key zonesub any; };
};

```