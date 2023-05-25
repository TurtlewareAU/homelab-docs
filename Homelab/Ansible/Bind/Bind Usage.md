install bind on the servers

```bash
ansible-playbook -i inventory.yml setup.yml
```

setup a new tsig-key.yml

```bash
ansible-playbook -i inventory.yml tsig.yml
```