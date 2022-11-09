

```yml
resources:
- repo: self 

variables:
  imageRepository: xamplifyautomation/cleanenergy
  containerRegistry: 'Xamplify Automation Docker'
  dockerhub: 'docker.io'
  tag: 'latest'
  vmImageName: 'ubuntu-latest'

stages:
- stage: DockerLogin
  displayName: Docker Hub Login
  jobs:
  - job: Login
    pool:
      vmImage: ubuntu-latest
    displayName: Login To Docker Hub
    steps:
    - task: Docker@2
      inputs:
        containerRegistry: $(containerRegistry)
        command: 'login'
    - task: CmdLine@2
      displayName: Run Docker
      inputs:
        script: |
          docker run -v $(Build.SourcesDirectory)/:/results/ 
          $(dockerhub)/$(imageRepository):latest

    - task: PublishTestResults@2
      displayName: Publish Test Results
      inputs:
        testResultsFormat: 'JUnit'
        testResultsFiles: '**/result.xml'
```
