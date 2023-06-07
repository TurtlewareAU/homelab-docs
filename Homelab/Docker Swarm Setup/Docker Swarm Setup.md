```bash
docker swarm init --advertise-addr 10.0.44.0
```

```bash
ansible-playbook -i inventory.yml setup.yml
```

```bash
ansible-playbook -i inventory.yml nfs.yml
```

```bash
ansible-playbook -i inventory.yml docker.yml
```

```bash
ansible-playbook -i inventory.yml swarm.yml
```

```bash
ansible-playbook -i inventory.yml manager-join.yml
```