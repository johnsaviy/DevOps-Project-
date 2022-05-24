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

<br>

- In Jenkins UI configure Artifactory


![16](https://user-images.githubusercontent.com/93729559/169708746-c4eef7ef-d2c6-4804-902a-2974602b897f.png)

<br>


![17](https://user-images.githubusercontent.com/93729559/169708958-f0063773-9b7a-440a-94b3-3b96666be1ad.png)

<br>

### Phase 2 – Integrate Artifactory repository with Jenkins.

- Create a dummy Jenkinsfile in the repository
- Using Blue Ocean, create a multibranch Jenkins pipeline
- On the database server, create database and user
- Update the database connectivity requirements in the file .env.sample
- Update Jenkinsfile with proper pipeline configuration



![18](https://user-images.githubusercontent.com/93729559/169710727-43661e43-9ba4-4402-b790-e1be37c39663.png)

![21](https://user-images.githubusercontent.com/93729559/169710737-2301a7b0-544d-449e-8a7b-4225acf95bf8.png)

![19](https://user-images.githubusercontent.com/93729559/169710730-6cae5c9e-311c-48a0-b274-b77214d9ae37.png)

![20](https://user-images.githubusercontent.com/93729559/169710735-7edfff72-1077-4962-82fe-14bf659bccac.png)



<br>

- Update the Jenkinsfile to include Unit tests step

![22](https://user-images.githubusercontent.com/93729559/169710913-e080686b-e26f-4310-a72a-8b8cb519ce5c.png)


<br>

### Phase 3 – Code Quality Analysis

This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc.


- Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.

![23](https://user-images.githubusercontent.com/93729559/169711545-f69d3145-d7cf-4cda-ade3-23561725dcca.png)

<br>


- Plot the data using plot Jenkins plugin.

This plugin provides generic plotting (or graphing) capabilities in Jenkins. It will plot one or more single values variations across builds in one or more plots. Plots for a particular job (or project) are configured in the job configuration screen, where each field has additional help information. Each plot can have one or more lines (called data series). After each build completes the plots’ data series latest values are pulled from the CSV file generated by phploc.


![24](https://user-images.githubusercontent.com/93729559/169711852-e7680dfb-e489-49d0-888f-bc3ac6ca5355.png)


<br>
<br>

- Bundle the application code for into an artifact (archived package) upload to Artifactory and Publish the resulted artifact into Artifactory.

![25](https://user-images.githubusercontent.com/93729559/169712239-b1b42729-8976-4801-b981-e6dcf0af7465.png)

![26](https://user-images.githubusercontent.com/93729559/169712563-d1d64f59-2616-42f7-83d6-0b43ca157687.png)

![27](https://user-images.githubusercontent.com/93729559/169712564-b5e1816e-0cc2-4957-ad90-6f801f9ee37b.png)





<br>
- Deploy the application to the dev environment by launching Ansible pipeline
- <br>

![28](https://user-images.githubusercontent.com/93729559/169712568-6e69ec62-3511-41c2-9720-41c7972104e1.png)


![29](https://user-images.githubusercontent.com/93729559/169980241-cb202e54-52a6-4d18-882d-c2150dba8e0d.png)



<br>




The build job used in this step tells Jenkins to start another job. In this case it is the ansible-project job, and we are targeting the main branch. Hence, we have ansible-project/main. Since the Ansible project requires parameters to be passed in, we have included this by specifying the parameters section. The name of the parameter is env and its value is dev. Meaning, deploy to the Development environment.

But how are we certain that the code being deployed has the quality that meets corporate and customer requirements? Even though we have implemented Unit Tests and Code Coverage Analysis with phpunit and phploc, we still need to implement Quality Gate to ensure that ONLY code with the required code coverage, and other quality standards make it through to the environments.

To achieve this, we need to configure SonarQube – An open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.


<br>

### SONARQUBE INSTALLATION


SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as The Sonar Way). Software testers and developers would normally work with project leads and architects to create custom quality gates.



- Install SonarQube on Ubuntu 20.04 With PostgreSQL as Backend Database.


![30](https://user-images.githubusercontent.com/93729559/169988872-bf97621f-2b93-4452-a323-d8194325c6cf.png)

![31](https://user-images.githubusercontent.com/93729559/169988878-9d3c23d7-85a0-4c14-b374-336084b0bdb0.png)

<br>

- Configure  SonarQube


![32](https://user-images.githubusercontent.com/93729559/169988882-e05facc7-ab57-48d3-98fc-3534c1d42efe.png)

![33](https://user-images.githubusercontent.com/93729559/169992846-ffa946a1-f9b8-44ca-af57-8dc30fee9bc1.png)

![34](https://user-images.githubusercontent.com/93729559/169992860-c333e0b5-3dce-4e91-9764-15c0658ae954.png)

![35](https://user-images.githubusercontent.com/93729559/169992862-7c9c58c7-da08-4d2d-a65d-3f36b0eb836a.png)

<br>

































																							 
																							 
																							 
