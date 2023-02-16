
When you want to run the Dev Ops pipelines for Tosca Automation the base script you need is located: `https://github.com/Tricentis/ToscaExecutionClient` this github repository has both windows and linux agent files which can be used to run from any agent and trigger on any other machine.

---
#### Azure DevOps Setup

Create a repository with the following structure

![](./img/Pasted%20image%2020230216151221.png)

| File | Description |
|---|---|
| azure-pipelines.yml | will handle the work necessary to bring down our repo, and perform any file changes needed|
| test.json | contains the test case guid for which to perform the automation test execution on |
| tosca.ps1 | ToscaExecutionClient which will call the appropriate server for automation execution |


---
##### Tosca.ps1
To provide value to this file navigate to the following url and copy paste the code into your .ps1 or .sh file based on your operating system. `https://raw.githubusercontent.com/Tricentis/ToscaExecutionClient/main/tosca_execution_client.ps1`

Once you have the execution file created you will want to create a dummy test file for checking your pipeline connectivity.

---
##### test.json
Create this file and then check in Tosca -> ExecutionList -> Your ExecutionList for the lists. In the properties tab, after clicking on the test event(s) you want to fire, copy the UniqueId and create a new json file similar to the example below. Image of where to find the UniqueId is after the json file example

```json
[
	"3a0969e7-2c45-625c-1811-188b59e051a6"
]
```

![](./img/Pasted%20image%2020230216154022.png)

---
##### azure-pipeline.yml
There is limited items required for the pipeline to run, below you will find an example yml file that will get the test cases run, and results stored back into DevOps.

```yml
trigger:
- main

steps:
- task: PowerShell@2
  inputs:
    filePath: 'Tosca.ps1'
    arguments: '-toscaServerUrl http://localhost -projectName tosca_demo -eventsConfigFilePath test.json'
- task: PublishTestResults@2
  inputs:
    testResultsFormat: 'JUnit'
    testResultsFiles: '**/*_results.xml'
```

![](./img/Pasted%20image%2020230216154545.png)