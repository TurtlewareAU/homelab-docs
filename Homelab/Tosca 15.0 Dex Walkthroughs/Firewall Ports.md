#### Firewall Ports To Open

## Tosca Administration Console

| Port Number | Direction |
|---|---|
| 80 | Outbound |
| 80 | Inbound |
| 443 | Outbound |
| 443 | Inbound|
| 5005 | Inbound |
| 5002 | Inbound |
| 5003 | Inbound |
| 5000 | Inbound |

#### Open Ports With Windows

Search For the Windws Defender Firewall and select it from the menu

![](Pasted%20image%2020230216092021.png)

Next you will want to follow these steps for all ports. Start by clicking on the Inbound Rules item. Click on the New Rule item from the Actions tab on the right of the window

![](Pasted%20image%2020230216091939.png)

Select the rule type of Port as we will want to open TCP Ports to this machine for the network

![](Pasted%20image%2020230216092045.png)

In Bound HTTP/HTTPS - Here we will want the specific ports to match, I created two inbound rules, 

> Firstly for Web Access (80, 443) - inbound
> Secondly for Services Access(5000, 5002, 5003, 5005) - inbound

![](Pasted%20image%2020230216092115.png)

Allow the connection

![](Pasted%20image%2020230216092135.png)

![](Pasted%20image%2020230216092148.png)

![](Pasted%20image%2020230216092210.png)
