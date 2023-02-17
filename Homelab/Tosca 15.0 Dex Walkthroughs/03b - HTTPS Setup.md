--- 
### HTTPS Certificate Installation (Windows)

Have your certificate issued by your certificate issuer, copt the crt file. The best way to get the certificate installed is to double click on the .crt on your computer and install.

Here I have saved the crt file for tosca.turtleware.au to my machine as ToscaServer.crt.

![](./img/Pasted%20image%2020230217122910.png)

Double clicking the .crt file will show the following window.

![](./img/Pasted%20image%2020230217123023.png)

Click on the Install Certificate to start the install wizard

![](./img/Pasted%20image%2020230217123337.png)

Click on Local Machine and then the next button.

![](./img/Pasted%20image%2020230217123421.png)

Select the Automatically select the certificate store based on the type of certificate radio button

![](./img/Pasted%20image%2020230217123511.png)

Click the finish button to install the certificate

![](./img/Pasted%20image%2020230217123547.png)

Seeing the above window says the certificate has been setup and install correctly. 

Now we are ready to start the Tosca Server install for HTTPS connections

--- 
### 
Launch Tricentis Tosca Server 15.2 Patch <#>.exe

![](./img/installer.png)
![](Pasted image 20230215104938.png)

The .exe will load into this splash screen ensure you click on the I agree check box before trying to continue.

![](./img/installer-splash.png)


Next you will see a list of pre-requisites needed for the machine to work with Tosca Server. To accept these and continue click the next button

![](./img/installer-reqs.png)

After the pre-requisites you will see a list of items that will be installed. Here we see Tosca Server, as that is the only item being installed by this instller. Click the next button to proceed

![](./img/installer-type.png)


After the type of installation we will be provided the Tosca Server Destination details, this is where you would like to store the Tosca Server install. Click next to proceed once happy with the destination

![](./img/Installer-destination.png)

Here we will uncheck the Anonymous usage stats. Click Next

![](./img/installer-diagnostics.png)

For the purposes of this install I will be using HTTP as I don't have a valid Certificate Thumprint for the HTTPS instance. Click Next.


![](Pasted%20image%2020230217122728.png)