Pihole has been running on vm's in my homelab stack for a while. it was my only dns sink hole for a while. I upgraded my router to pfsense and started using ad blocking via pfsense and these were only used for DNS querying for local DNS names. Basically I had 3 machines running to serve simple in home dns which was not much. 

I decided a good way to rectify this was to run PiHole via docker and perform the same tasks via a docker swarm. Below is the swarm stack I have and use in Portainer. 

```
```