## AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY



In this project I will build a secure infrastructure inside AWS VPC (Virtual Private Cloud) network for a  company that uses WordPress CMS for its main business website, and a Tooling Website for their DevOps team. As part of the company’s desire for improved security and performance, a decision has been made to use a reverse proxy technology from NGINX to achieve this.

Cost, Security, and Scalability are the major requirements for this project. Hence, implementing the architecture designed below, ensure that infrastructure for both websites, WordPress and Tooling, is resilient to Web Server’s failures, can accomodate to increased traffic and, at the same time, has reasonable cost.

![tooling_project_15](https://user-images.githubusercontent.com/93729559/170043248-ea4dc348-7605-4108-bb5d-3a2080719353.png)


<br>

There are few requirements that must be met:

- Properly configure your AWS account and Organization Unit .



### Set Up a Virtual Private Network (VPC)

Always make reference to the architectural diagram and ensure that your configuration is aligned with it.

- Create a VPC
- Create subnets as shown in the architecture
- Create a route table and associate it with public subnets
- Create a route table and associate it with private subnets
- Create an Internet Gateway
- Edit a route in public route table, and associate it with the Internet Gateway. (This is what allows a public subnet to be accisble from the Internet)
- Create 3 Elastic IPs
- Create a Nat Gateway and assign one of the Elastic IPs (*The other 2 will be used by Bastion hosts)


![1](https://user-images.githubusercontent.com/93729559/170094425-2214c99b-bd6d-42f7-8833-2100172a6592.png)

![2](https://user-images.githubusercontent.com/93729559/170094427-d5b5162e-4578-4e9e-9d5f-8e496cb6a1d9.png)

![3](https://user-images.githubusercontent.com/93729559/170094434-84650511-7cd6-4bc1-a41e-479c230e611e.png)

![4](https://user-images.githubusercontent.com/93729559/170094439-8d93d03a-9d0d-439e-abe2-d5109936780b.png)

![5](https://user-images.githubusercontent.com/93729559/170094456-12350c4a-4791-48d4-9fee-cba14cb07ed5.png)

![6](https://user-images.githubusercontent.com/93729559/170094479-7908ac59-d0ad-4d3c-98a4-090508847b06.png)

![7](https://user-images.githubusercontent.com/93729559/170094486-ca8cbaba-dacc-4b8f-b466-6504ecc4dc34.png)

![8](https://user-images.githubusercontent.com/93729559/170094490-73342daa-0209-4c55-9688-ff07c0343c46.png)

![9](https://user-images.githubusercontent.com/93729559/170094492-7346921d-bb23-4b11-9f93-d1bb9a9f746d.png)

![10](https://user-images.githubusercontent.com/93729559/170094497-c92b9416-f3d6-4732-892a-899cf96f7207.png)

![11](https://user-images.githubusercontent.com/93729559/170094499-5e90f1de-e633-44e4-bc85-699dd367c029.png)

 
 ### Create a Security Group for:

- Nginx Servers: Access to Nginx should only be allowed from a Application Load balancer (ALB). At this point, we have not created a load balancer, therefore we will update the rules later. For now, just create it and put some dummy records as a place holder.
- Bastion Servers: Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, you can use your workstation public IP address. To get this information, simply go to your terminal and type curl www.canhazip.com
Application Load Balancer: ALB will be available from the Internet
- Webservers: Access to Webservers should only be allowed from the Nginx servers. Since we do not have the servers created yet, just put some dummy records as a place holder, we will update it later.
- Data Layer: Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully designed – only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.
ers (ALB)



![12](https://user-images.githubusercontent.com/93729559/170094514-3e0509d0-2b89-4ec1-9d50-ca61b78ad9f5.png)

![13](https://user-images.githubusercontent.com/93729559/170094520-010b2ecb-d377-46a3-87ce-a34951ea2aa5.png)

![14](https://user-images.githubusercontent.com/93729559/170094523-85c9dc93-fc65-45ab-be62-503d50f303db.png)

![15](https://user-images.githubusercontent.com/93729559/170094529-c73cb56c-8be0-434d-8659-42a9340944ee.png)



<br>


### Setting up domain name

![43](https://user-images.githubusercontent.com/93729559/170218883-2e430bee-4eb1-4b1d-8b68-656bd480fca8.png)



### Setting up Elastic filesystem

Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic Network File System (NFS) for use with AWS Cloud services and on-premises resources. In this project, I will utulize EFS service and mount filesystems on both Nginx and Webservers to store data.

Create an EFS filesystem
Create an EFS mount target per AZ in the VPC, associate it with both subnets dedicated for data layer
Associate the Security groups created earlier for data layer.
Create an EFS access point. (Give it a name and leave all other settings as default)


![16](https://user-images.githubusercontent.com/93729559/170099416-06768bad-14e9-416d-abd1-2183c8886dab.png)

![17](https://user-images.githubusercontent.com/93729559/170101747-029fa993-d18d-40c1-8f10-d2849a842808.png)

<br>

### Setup RDS

Pre-requisite: Create a KMS key from Key Management Service (KMS) to be used to encrypt the database instance.

![18](https://user-images.githubusercontent.com/93729559/170103025-1868f4b9-c193-4962-8d2f-27f52c177cf9.png)


![19](https://user-images.githubusercontent.com/93729559/170104055-9e9d02af-b0fd-4964-bc7e-fc3a091ce375.png)


![20](https://user-images.githubusercontent.com/93729559/170105422-3fd580ac-88b3-4e93-87df-2b4dd9cd599c.png)


![41](https://user-images.githubusercontent.com/93729559/170212592-8c016efd-9caa-4dfc-a831-cedd4c5563ee.png)




### Set Up Compute Resources for Nginx

- Provision EC2 Instances for Nginx
- Create an EC2 Instance based on CentOS Amazon Machine Image (AMI) in any 2 Availability Zones (AZ) in any AWS Region (it is recommended to use the Region that is closest to your customers). Use EC2 instance of T2 family (e.g. t2.micro or similar)

- Ensure that it has the following software installed: python, ntp, net-tools, vim, wget, telnet, epel-release, htop

![24](https://user-images.githubusercontent.com/93729559/170137924-14ebd555-de95-404d-91aa-86ded23d61f1.png)

![25](https://user-images.githubusercontent.com/93729559/170137921-257c7743-b926-4a93-a7bd-ae3dce824acd.png)

<br>
- Install efs utils for mounting target on the EFS and set up self-signed certificate for nginx webserver instance.

![26](https://user-images.githubusercontent.com/93729559/170139864-d67e8013-5853-42bb-bd94-3a17efd6e5d4.png)

![27](https://user-images.githubusercontent.com/93729559/170139867-1f751982-cad7-4904-b49c-e36a8235ca8f.png)

![28](https://user-images.githubusercontent.com/93729559/170139868-ea8d6771-17fd-4c92-ac2d-3a1745797365.png)

![30](https://user-images.githubusercontent.com/93729559/170140907-3dd4f1d6-ee38-4175-8ab2-568e4788e6c0.png)

<br>


### Create an AMI out of the EC2 instance

![33](https://user-images.githubusercontent.com/93729559/170141939-753478db-2fff-4b01-831d-d0b830668944.png)


- Prepare Launch Template For Nginx (One Per Subnet)
- Make use of the AMI to set up a launch template
- Ensure the Instances are launched into a public subnet
- Assign appropriate security group
- Configure Userdata to update yum package repository and install nginx


![37](https://user-images.githubusercontent.com/93729559/170206594-47269e3d-4529-4d01-ad9e-d4333dee2354.png)



### Configure Target Groups
- Select Instances as the target type
- Ensure the protocol HTTPS on secure TLS port 443
- Ensure that the health check path is /healthstatus
- Register Nginx Instances as targets
- Ensure that health check passes for the target group

![34](https://user-images.githubusercontent.com/93729559/170201593-0878f785-0623-47f1-8405-dfd078b70c2f.png)



 
### Configure Autoscaling For Nginx
- Select the right launch template
- Select the VPC
- Select both public subnets
- Enable Application Load Balancer for the AutoScalingGroup (ASG)
- Select the target group you created before
- Ensure that you have health checks for both EC2 and ALB
- The desired capacity is 2
- Minimum capacity is 2
- Maximum capacity is 4
- Set scale out if CPU utilization reaches 90%
- Ensure there is an SNS topic to send scaling notifications


![40](https://user-images.githubusercontent.com/93729559/170210369-48e3ff30-a772-4d36-bafa-0b19a75a3ad3.png)


<br>

### Set Up Compute Resources for Bastion

- Provision the EC2 Instances for Bastion
- Create an EC2 Instance based on CentOS Amazon Machine Image (AMI) per each Availability Zone in the same Region and same AZ where you created Nginx server
- Ensure that it has the following software installed

python, ntp, net-tools, vim, wget, telnet, epel-release, htop


![21](https://user-images.githubusercontent.com/93729559/170136036-0addf721-eca9-46b1-9771-92f04c7c1085.png)

![22](https://user-images.githubusercontent.com/93729559/170136098-97ef7334-fd37-4f8d-9ab5-279afbf30b2a.png)

![23a](https://user-images.githubusercontent.com/93729559/170136106-99fdaf9b-370a-4e31-90ad-3caff475babb.png)

<br>

- Create an AMI out of the EC2 instance

![32](https://user-images.githubusercontent.com/93729559/170141633-4dc0716a-11dc-48cd-88f3-aeaf93397f45.png)


### Prepare Launch Template For Bastion (One per subnet)
- Make use of the AMI to set up a launch template
- Ensure the Instances are launched into a public subnet
- Assign appropriate security group
- Configure Userdata to update yum package repository and install Ansible and git


![36](https://user-images.githubusercontent.com/93729559/170204576-b5c1e614-53a6-434d-9378-e8ae879f49ba.png)



### Configure Target Groups
- Select Instances as the target type
- Ensure the protocol is TCP on port 22
- Register Bastion Instances as targets
- Ensure that health check passes for the target group

### Configure Autoscaling For Bastion
- Select the right launch template
- Select the VPC
- Select both public subnets
- Enable Application Load Balancer for the AutoScalingGroup (ASG)
- Select the target group you created before
- Ensure that you have health checks for both EC2 and ALB
- The desired capacity is 2
- Minimum capacity is 2
- Maximum capacity is 4
- Set scale out if CPU utilization reaches 90%
- Ensure there is an SNS topic to send scaling notifications
- 

![39](https://user-images.githubusercontent.com/93729559/170209964-b4797ad3-5b3c-413b-9a75-f7c6a1e85ca5.png)



### Set Up Compute Resources for Webservers
- Provision the EC2 Instances for Webservers


- Now, you will need to create 2 separate launch templates for both the WordPress and Tooling websites

- Create an EC2 Instance (Centos) each for WordPress and Tooling websites per Availability Zone (in the same Region).

Ensure that it has the following software installed: python, ntp, net-tools, vim, wget,


![24](https://user-images.githubusercontent.com/93729559/170137924-14ebd555-de95-404d-91aa-86ded23d61f1.png)

![25](https://user-images.githubusercontent.com/93729559/170137921-257c7743-b926-4a93-a7bd-ae3dce824acd.png)

<br>
- Install efs utils for mounting target on the EFS and set up self-signed certificate for apache webserver instance.

![26](https://user-images.githubusercontent.com/93729559/170139864-d67e8013-5853-42bb-bd94-3a17efd6e5d4.png)

![27](https://user-images.githubusercontent.com/93729559/170139867-1f751982-cad7-4904-b49c-e36a8235ca8f.png)

![28](https://user-images.githubusercontent.com/93729559/170139868-ea8d6771-17fd-4c92-ac2d-3a1745797365.png)

![29](https://user-images.githubusercontent.com/93729559/170140661-40a13a69-15cd-4f3c-ac8b-f78812877cfa.png)

![30](https://user-images.githubusercontent.com/93729559/170140907-3dd4f1d6-ee38-4175-8ab2-568e4788e6c0.png)


<br>

- Create an AMI out of the EC2 instance

![31](https://user-images.githubusercontent.com/93729559/170141475-ce026b6b-6dc2-435a-8e6a-85c6cc722135.png)


### Prepare Launch Template For Webservers (One per subnet)

- Make use of the AMI to set up a launch template
- Ensure the Instances are launched into a public subnet
- Assign appropriate security group
- Configure Userdata to update yum package repository and install wordpress (Only required on the WordPress launch template)



![38](https://user-images.githubusercontent.com/93729559/170208868-25a417af-a20c-4489-af52-b6db28c9f725.png)


### Configure Autoscaling For Tooling and wordpress

![42](https://user-images.githubusercontent.com/93729559/170213888-200bd067-4756-41fe-ab4d-1d7194ff52fb.png)





TLS Certificates From Amazon Certificate Manager (ACM)
You will need TLS certificates to handle secured connectivity to your Application Load Balancers (ALB).

Navigate to AWS ACM
Request a public wildcard certificate for the domain name you registered in Freenom
Use DNS to validate the domain name
Tag the resource


