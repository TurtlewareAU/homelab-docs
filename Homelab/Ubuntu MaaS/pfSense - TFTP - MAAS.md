
Ubuntu MaaS is software developed by Canonical as a way to provission ubuntu machines without the need to hold an ISO image and perform all of the installation steps. When you have created your server, and its up and running you will already have version of ubuntu that can be deployed to a new machine. 

I have my server already setup check out backlink to install for installation steps. The main dashboard looks like the image below. 

![](maas-dashboard.png)

Once this is setup correctly we will want to allow this server for network booting via pfSense gui. Login to your pfSense GUI. Click on the Services > DHCP Server menu item. You should land on this page.

![](Pasted%20image%2020230306135111.png)

From this page scroll all the way down to the other options section of the page and locate

![](Pasted%20image%2020230306135214.png)

Set the TFTP Server to your Ubuntu MaaS server ip address.

Next set Enables network booting check box to see the fields below.

> `Next Server` is the location of your MaaS server same as above
> `default BIOS file name` needs to be set to pxelinux.0
> `uefi 32 bit file name` was part of a walkthrough I saw when you are doing uefi booting.
> `uefi 64 bit fil name` same as above

If you are doing uefi network booting then you will need to check these are correct. I have not properly tested uefi booting for my purposes.

![](Pasted%20image%2020230306135228.png)

With all these changes / modifications made you can save the DHCP Server details. 
![](Pasted%20image%2020230306135608.png)

No you can follow ![Terraform - Proxmox - VM Create Network Boot]()
