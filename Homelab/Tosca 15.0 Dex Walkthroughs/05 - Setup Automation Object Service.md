#### Enabling Dex Server

Navigate to ```here```

![](Pasted%20image%2020230216121419.png)

You will want to edit the Tricentis.Distribution.ServerService.exe.config with notepad

When loaded find the element you are after.

`ctrl + f ` and then enter `tricentis.distributionserver.properties.settings`

![](Pasted%20image%2020230216121615.png)

make sure the EnableWorkspacelessExecution is true

#### Setup AOS

Navigate to the following url : http://localhost/#/configuration/ao

Here is the landing page for the Automation Object Service.
![](./img/aos-landing.png)

Add your workspace details which you saved in step 04 Database Repository Setup. Make sure all inputs have a tick so you ensure you have correctly setup the service.

![](./img/aos-workspace.png)

Next make sure the http settings for the Distributed Execution are set accordingly.

![](./img/aos-distributed-exec.png)

Make any changes as per the following settings.

![](./img/aos-http-settings.png)

Lastly click on the save button. when completed you will see this image when all successfully applied and restarted.

![](./img/aos-outcome.png)