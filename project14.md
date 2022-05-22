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


- Jenkins needs to export the ANSIBLE_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set.


![5](https://user-images.githubusercontent.com/93729559/169650069-6ddeb5d9-2b47-4539-abc9-5c549d569929.png)


- in Global tool configuration, configure Ansible as below:


![6](https://user-images.githubusercontent.com/93729559/169650816-1c7dfe7a-c3e4-4c72-8e38-783634870415.png)


- Configure pipeline syntax and generate pipeline script.

![7](https://user-images.githubusercontent.com/93729559/169650930-f818b984-28ca-4124-bb84-954e75f8fc33.png)


- update pipeline script in jenkins file for running ansible playbook.

![8](https://user-images.githubusercontent.com/93729559/169651056-59126278-089a-4d60-8f94-9b826cbd7765.png)









- Parameterizing Jenkinsfile For Ansible Deployment.

- To deploy to other environments, we will need to use parameters.


- Update sit inventory with new servers.

- Update Jenkinsfile to introduce parameterization. Below is just one parameter. It has a default value in case if no value is specified at execution. It also has a description so that everyone is aware of its purpose


![9](https://user-images.githubusercontent.com/93729559/169651444-6a5175ee-8af9-482c-aed2-28ea1ae4b792.png)



- In the Ansible execution section, remove the hardcoded inventory/dev and replace with `${inventory}
From now on, each time you hit on execute, it will expect an input.

![10](https://user-images.githubusercontent.com/93729559/169651654-b17eb1b8-aff9-4fc5-b5eb-bafa15572f7c.png)



 ### CI/CD PIPELINE FOR TODO APPLICATION.

We already have tooling website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application.

Our goal here is to deploy the application onto servers directly from Artifactory rather than from git.

- Phase 1 – Prepare Jenkins

- Fork the todo -app repository from https://github.com/darey-devops/php-todo.git
 

![11](https://user-images.githubusercontent.com/93729559/169652688-0a9820b6-769c-4146-be33-cf7fc7c9951b.png)

<br>

- On you Jenkins server, install PHP, its dependencies and Composer tool 

![12](https://user-images.githubusercontent.com/93729559/169652881-1565a638-290d-462f-b00b-fdaf2089615b.png)


![13](https://user-images.githubusercontent.com/93729559/169652983-1fbf2fd0-00d8-4f5b-b636-832f815c98ff.png)





- Install Jenkins plugins

- Plot plugin

![14](https://user-images.githubusercontent.com/93729559/169653114-8cf0c627-f49c-4dc1-8fe2-99b9716a1a4a.png)


<br>

- Artifactory plugin

![15](https://user-images.githubusercontent.com/93729559/169653266-91c59b78-12b5-44c7-8e8e-f5acf1630347.png)

<br>



- We will use plot plugin to display tests reports, and code coverage information.
- The Artifactory plugin will be used to easily upload code artifacts into an Artifactory server.



- In Jenkins UI configure Artifactory


![16](https://user-images.githubusercontent.com/93729559/169708746-c4eef7ef-d2c6-4804-902a-2974602b897f.png)

<br>

![17](https://user-images.githubusercontent.com/93729559/169708958-f0063773-9b7a-440a-94b3-3b96666be1ad.png)































																							 
																							 
																							 
