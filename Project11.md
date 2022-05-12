
## ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10


In Projects 7 to 10 I performed a lot of manual operations to set up virtual servers, install and configure required software, and deploy the web application.

In this Project I'll make you use of appreciable DevOps tools to make most of the routine tasks automated with Ansible Configuration Management, at the same time I'll be using declarative language such as YAML.

### Ansible Client as a Jump Server (Bastion Host)
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

<br>

![bastion](https://user-images.githubusercontent.com/93729559/167856067-7f77dc3e-74f4-44b4-ad93-2b626e1f3342.png)

#### Task

- Install and configure Ansible client to act as a Jump Server/Bastion Host
- Create a simple Ansible playbook to automate servers configuration

<br>


### Step 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE.

- Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.


![1](https://user-images.githubusercontent.com/93729559/167857180-ca5f9506-5839-4c91-9cc7-e2b70de191da.png)
<br>


- create a new repository in github and name it ansible-config-mgt.
<br>

- Instal Ansible

![2](https://user-images.githubusercontent.com/93729559/167858279-14bd224a-e986-424d-a6d7-5210f77149cb.png)

![2a](https://user-images.githubusercontent.com/93729559/167858577-bce7adc7-9b35-4645-b0b3-e0e9fc2554bb.png)


<br>


- Configure Jenkins build job to save your repository content every time you change it.

- Create a new Freestyle project ansible in Jenkins and point it to the ‘ansible-config-mgt’ repository.
- 
- Configure Webhook in GitHub and set webhook to trigger ansible build.

- Configure a Post-build job to save all (**) files.

- Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder


![3](https://user-images.githubusercontent.com/93729559/167868404-844d607d-6a84-4987-a4c5-6bb41d577bc2.png)

![3a](https://user-images.githubusercontent.com/93729559/167864376-c69eecdd-3336-47cc-b41a-b5a862a244c1.png)



###  Step 2 – Prepare your development environment using Visual Studio Code

- Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance


![4](https://user-images.githubusercontent.com/93729559/167869902-10edddbe-b785-46ab-b858-04b0a1f73102.png)


<br>

 ### Step 3 - BEGIN ANSIBLE DEVELOPMENT.
 
- In the ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.


![5](https://user-images.githubusercontent.com/93729559/167873253-757ebbe1-3fe7-4192-b7fa-e0e22dff3f97.png)


- Checkout the newly created feature branch to your local machine and start building your code and directory structure.


- Create a directory and name it playbooks – it will be used to store all your playbook files.
 
- Create a directory and name it inventory – it will be used to keep your hosts organised.

- Within the playbooks folder, create your first playbook, and name it common.yml

- Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.
- 

![6](https://user-images.githubusercontent.com/93729559/167875537-a0acbe6f-59f0-47f4-b4ba-f1609af8754b.png)


<br>


### Step 4 – Set up an Ansible Inventory


An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your development servers. 


![7](https://user-images.githubusercontent.com/93729559/168103821-1977a0c7-f21d-4669-a116-922ffc926baf.png)

<br>


### Step 5 – Create a Common Playbook

It is time to start giving Ansible the instructions on what it needs to perform on all servers listed in inventory/dev.

In common.yml playbook I'll write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

- Updating the playbooks/common.yml file with following code:

![8](https://user-images.githubusercontent.com/93729559/168106721-2c3282d9-ee22-4362-896d-1c8c8da4a882.png)


<br>


### Step 6 – Update GIT with the latest code

![9](https://user-images.githubusercontent.com/93729559/168113467-6fff48a0-03b2-44fc-9c9e-7a0bbf30e871.png)


![10](https://user-images.githubusercontent.com/93729559/168118038-14ae9290-b648-4aa3-8b11-159083e4bc66.png)



<br>

### Step 7 – Run first Ansible test

Now, it is time to execute ansible-playbook command and verify if the playbook actually works:

























