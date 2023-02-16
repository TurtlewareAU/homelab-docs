
#### Configuration Settings

Locate the ToscaDistributionAgent

Navigate to ```C:\Program Files (x86)\TRICENTIS\Tosca Testsuite\DistributedExecution```

![](./img/Pasted%20image%2020230216113750.png)

Right click the ToscaDistributionAgent.exe and select run as administrator. The Agent will then start and become active
![](./img/Pasted%20image%2020230216113920.png)

Right click on the agent and select configure agent

![](./img/Pasted%20image%2020230216114003.png)

Enter the Machine details, these will determine how Tosca will filter agents for test events. As you can select agents for a test event based on the information stored in this screen. These are not mandatory or require changing, however will only match when these values are selected in tosca. will be covered later in the test event setup.

![](./img/Pasted%20image%2020230216114034.png)

Setup the workspace details. Click the ... button and navigate the the `*.tws` file which Tosca created for the repository back in the 04 - Database Repository Setup

![](./img/Pasted%20image%2020230216114119.png)

Next Check that the Connect To Server is returning a green tick this means everything is sorted for the communication service.

![](./img/Pasted%20image%2020230216114135.png)

Next if there is a need to setup tokens and client secrets they can be done under the authenticate agent section

![](./img/Pasted%20image%2020230216114151.png)

Next Unattended execution is where you put in remote desktop information. this will include the user name and password, and the desktop settings to use for the remote connection.

![](./img/Pasted%20image%2020230216114213.png)

Setup logging is the last section and this will allow you to see the log location, as well as changing the logging message type.

![](./img/Pasted%20image%2020230216114226.png)

Clicking on the save button will save the configuration to the machine and allow the agent to be set to active status.

left cl