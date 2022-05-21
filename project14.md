## EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP


SIMULATING A TYPICAL CI/CD PIPELINE FOR A PHP BASED APPLICATION.
  
As part of the ongoing infrastructure development with Ansible started from Project 11,  I'll be creating a pipeline that simulates continuous integration and delivery. 
 Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both Tooling and TODO Web Applications are based on an interpreted (scripting) language (PHP). It means, it can be deployed directly onto a server and will work without compiling the code to a machine language.

The problem with that approach is, it would be difficult to package and version the software for different releases. 
  And so, in this project, I will be using a different approach for releases, rather than downloading directly from git, I will be using Ansible uri module.
    

![CI_CD-Pipeline-For-PHP-ToDo-Application](https://user-images.githubusercontent.com/93729559/169549849-dc03f465-a8d1-4b77-9681-6625ed6f0f4c.png)
<br>   
  
  To get started, we will focus on these environments initially - Ci, Dev Pentest.
																							 
Both SIT – For System Integration Testing and UAT – User Acceptance Testing do not require a lot of extra installation or configuration. They are basically the webservers holding our applications. But Pentest – For Penetration testing is where we will conduct security related tests, so some other tools and specific configurations will be needed. In some cases, it will also be used for Performance and Load testing. Otherwise, that can also be a separate environment on its own. It all depends on decisions made by the company and the team running the show.

What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table.
																							 
<img width="832" alt="Environment-setup" src="https://user-images.githubusercontent.com/93729559/169549663-c439c774-96d3-4e28-a8e0-ab18a4fa268d.png">

<br>


- ANSIBLE ROLES FOR CI ENVIRONMENT.

Now I'll go ahead and Add two more roles to ansible:

- SonarQube 

- Artifactory


Why do we need SonarQube?

SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.

Why do we need Artifactory?
Artifactory is a product by JFrog that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be used for certain other automation, but we will it strictly to manage our build artifacts.


- Configuring Ansible For Jenkins Deployment.
- 
In previous projects, I have been launching Ansible commands manually from a CLI. Now, with Jenkins, I will start running Ansible from Jenkins UI.

To do this,

- Navigate to Jenkins URL

- Install & Open Blue Ocean Jenkins Plugin

- Create a new pipeline


- Select GitHub


- Connect Jenkins with GitHub


- Login to GitHub & Generate an Access Token

- Copy and Paste the token to the pipeline and connect



![1](https://user-images.githubusercontent.com/93729559/169647781-9c5e3e9a-014d-4ded-8db6-234946b2d657.png)

![2](https://user-images.githubusercontent.com/93729559/169647785-ec83ff49-6ed9-490a-83b4-592be027726c.png)


<br>

- create a new directory deploy and start a new file Jenkinsfile inside the directory.

- Now go back into the Ansible pipeline in Jenkins, and select configure.

- Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile.


![3](https://user-images.githubusercontent.com/93729559/169647786-e23dfe82-dcdf-4e54-ba87-931ca0d069c2.png)



Back to the pipeline again, this time click "Build now"

![4](https://user-images.githubusercontent.com/93729559/169648990-37c748f2-e304-4778-a973-e8bc2916cd1f.png)




### RUNNING ANSIBLE PLAYBOOK FROM JENKINS


 Let us get the actual Ansible deployment to work by: Installing Ansible on Jenkins, Installing Ansible plugin in Jenkins UI and Creating Jenkinsfile from scratch. 

- Parameterizing Jenkinsfile For Ansible Deployment.

- To deploy to other environments, we will need to use parameters.


- Update sit inventory with new servers.

































																							 
																							 
																							 
