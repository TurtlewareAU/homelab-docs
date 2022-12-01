
Login to your sonar qube instance http://sonarqube:9000

under projects click on create Project

![[Pasted image 20221201141600.png]]

Next selec the CI tool of your choice. Here are home I run a jenkins server for all of my at home CI pipeline needs. Also its so versatile for CI, many plugins are compatible with the tool.

![[Pasted image 20221201141512.png]]

You will now need to follow all of the steps necessary to complete setup of Sonarqube with Jenkins.

![[Pasted image 20221201141857.png]]

The first step is to follow this set of steps: [[Setup Jenkins For SonarQube]] complete these steps and then return to this section.

### With SonarQube Scanner Installed

The next step is to move onto step 2 of the sonarqube setup. This means setting up a new jenkins pipeline that will act as our CI build for our scanner to run and analyse our code. As per the below image states.

![[Pasted image 20221201143806.png]]

For this we need to head over to Jenkins once again and create a new pipeline

![[Pasted image 20221201144517.png]]

Here I am going to create a new pipeline for my current production personal/business website. This website is built with react, using docker as the hosting platform, all running from a linode cloud server. Click ok to be taken to the pipeline configuration page.

Scroll down to the Build Triggers Section and click on the Trigger builds remotely checkbox. Make sure you enter an authentication token. I have been using lastpass to generate an 18 character password and then using that for my authentication token.

![[Pasted image 20221201145204.png]]

After setting these values scroll down to the Pipeline section. Here you want to setup the definition: Pipeline script from SCM (file stored in your repository)
SCM: Git (as I use git to pull down from github)
Repository: https://github.com/username/repository.git
Credentials: select your git credentials for use with the repository
Branch: make sure this matches your repositry settings (github uses main)
Script Path: Jenkinsfile

If all setup you should have something similar to the image

![[Pasted image 20221201150038.png]]

Click Save, your pipeline has been setup however we have a few more steps to complete to make it all work well together.


