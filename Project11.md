
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





