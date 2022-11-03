Pihole has been running on vm's in my homelab stack for a while. it was my only dns sink hole for a while. I upgraded my router to pfsense and started using ad blocking via pfsense and these were only used for DNS querying for local DNS names. Basically I had 3 machines running to serve simple in home dns which was not much. 

I decided a good way to rectify this was to run PiHole via docker and perform the same tasks via a docker swarm. Below is the swarm stack I have and use in Portainer. 

The default part of the compose - defined by the pihole team.
```yml
version: "3"
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
    environment:
      TZ: 'Australia/Sydney'
```

The above is the bare minimum to make the containers run and do their work.

I made changes to the compose and included some NFS mounted folders for storing the pihole configurations. these are set to the machine, I want these to pick up from an NFS Server so I can manage one copy.
```yml
   volumes:
      - '/mnt/docker/pihole:/etc/pihole'
      - '/mnt/docker/pihole/etc-dnsmasq.d:/etc/dnsmasq.d'
```

The final piece is the deploy section. I have a management VM which is the manager of the docker swarm. I back this up nightly, and I like to run all my containers on the worker machines and not the manager. this allows me to lose the manager, backup from a previous nights backup and unlock my swarm.
```yml
    deploy:
      replicas: 3
      placement:
        constraints:
          - "node.role==worker"
```

Below is the entire compose file I have defined and used for my stack

```yml
version: "3"
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "8000:80/tcp"
    environment:
      TZ: 'Australia/Sydney'
    volumes:
      - '/mnt/docker/pihole:/etc/pihole'
      - '/mnt/docker/pihole/etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped
    deploy:
      replicas: 3
      placement:
        constraints:
          - "node.role==worker"
```