If you have not already setup the Sonarqube for jenkins plugin you need to do that now. Add the following plugins to jekins to work with SonarQube. https://plugins.jenkins.io/sonar/

Once you have completed the install you will need to do a little setup in Jenkins.

Navigate to `manage jenkins` > `Global Tool Configuration`

Scroll down till you find the SonarQube Scanner settings. 

I clicked on the Add SonarQube Scanner option as I develop mostly in Typescript or Javascript. 

![[Pasted image 20221201142127.png]]

SonarQube Scanner Settings
Name: `anything you want it to be`
Click install automatically check box

Installation is now setup for Jenkins. Continue with this section if you came here from my other documents.

[[Sonarqube Setup]]
