
When you want to run the Dev Ops pipelines for Tosca Automation the base script you need is located: `https://github.com/Tricentis/ToscaExecutionClient` this github repository has both windows and linux agent files which can be used to run from any agent and trigger on any other machine.

#### Azure DevOps Setup

Create a repository with the following structure

![](./img/Pasted%20image%2020230216151221.png)

| File | Description |
|---|---|
| azure-pipelines.yml | will handle the work necessary to bring down our repo, and perform any file changes needed|
| test.json | contains the test case guid for which to perform the automation test execution on |
| tosca.ps1 | ToscaExecutionClient which will call the appropriate server for automation execution |


##### Tosca.ps1
To provide value to this file navigate to the following url and copy paste the code into your .ps1 or .sh file based on your operating system. `https://raw.githubusercontent.com/Tricentis/ToscaExecutionClient/main/tosca_execution_client.ps1`

Once you have the execution file created you will want to create a dummy test file for checking your pipeline connectivity.
