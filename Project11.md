
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

