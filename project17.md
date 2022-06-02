## AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 2


Let us continue from where we have stopped in Project 16.

Based on the knowledge from the previous project lets keep on creating AWS resources!

**Networking**

- Private subnets & best practices.

**Create 4 private subnets keeping in mind following principles:**

- Make sure you use variables or length() function to determine the number of AZs
- Use variables and cidrsubnet() function to allocate vpc_cidr for subnets
- Keep variables and resources in separate files for better code structure and readability
- Tags all the resources you have created so far. Explore how to use format() and count functions to automatically tag subnets with its respective number.


![1](https://user-images.githubusercontent.com/93729559/171413852-5054adf6-c4e9-4022-8024-6a8f3c24e241.png)

![2](https://user-images.githubusercontent.com/93729559/171413861-0d5e7101-bd7d-40d3-ab72-5aed44d38696.png)

![3](https://user-images.githubusercontent.com/93729559/171413867-e194941d-c6cd-412d-b406-af57cb6d8ea5.png)


**Internet Gateways & format() function**

Create an Internet Gateway in a separate Terraform file internet_gateway.tf

![4](https://user-images.githubusercontent.com/93729559/171414356-f6077884-ccf7-4b5b-9463-915c4073b68c.png)


**NAT Gateways**
Create 1 NAT Gateways and 1 Elastic IP (EIP) addresses

Now use similar approach to create the NAT Gateways in a new file called natgateway.tf.

Note: We need to create an Elastic IP for the NAT Gateway, and you can see the use of depends_on to indicate that the Internet Gateway resource must be available before this should be created. Although Terraform does a good job to manage dependencies, but in some cases, it is good to be explicit.


![5](https://user-images.githubusercontent.com/93729559/171417930-7b8d5711-d115-4c1b-a5b1-4ba6c465c2a5.png)



**AWS ROUTES**

Create a file called route_tables.tf and use it to create routes for both public and private subnets, create the below resources. Ensure they are properly tagged.

- aws_route_table
- aws_route
- aws_route_table_association

![6](https://user-images.githubusercontent.com/93729559/171442848-cd5962d5-f11f-436d-bd83-d83d0eee87d8.png)

- Run terraform plan and apply
- 

![8](https://user-images.githubusercontent.com/93729559/171442854-837bc4b1-0831-43e0-84f7-c54648d84f75.png)

![8a](https://user-images.githubusercontent.com/93729559/171442857-66160e1c-5396-489f-ab57-becd464eac98.png)

![8b](https://user-images.githubusercontent.com/93729559/171442861-5198fe44-948c-4be6-b5cf-877602fd8bc5.png)

![8c](https://user-images.githubusercontent.com/93729559/171442863-c49c4d32-809c-446b-b348-80fdcae13ff0.png)

![8d](https://user-images.githubusercontent.com/93729559/171442865-32fb294f-94ae-4f76-b510-40f9cff2d613.png)

![8e](https://user-images.githubusercontent.com/93729559/171442869-651ccc6c-0e8f-4949-ba31-ebbc7ab09c1d.png)


Now, we are done with Networking part of AWS set up, let us move on to Compute and Access Control configuration automation using Terraform!

**AWS Identity and Access Management** 

- IaM and Roles
- 
We want to pass an IAM role our EC2 instances to give them access to some specific resources, so we need to do the following:

- Create AssumeRole
Assume Role uses Security Token Service (STS) API that returns a set of temporary security credentials that you can use to access AWS resources that you might not normally have access to. These temporary credentials consist of an access key ID, a secret access key, and a security token. Typically, you use AssumeRole within your account or for cross-account access.

Add the following code to a new file named roles.tf

In this code we are creating AssumeRole with AssumeRole policy. It grants to an entity, in our case it is an EC2, permissions to assume the role.


- Create IAM policy for this role
This is where we need to define a required policy (i.e., permissions) according to our requirements. For example, allowing an IAM role to perform action describe applied to EC2 instances:

- Attach the Policy to the IAM Role
This is where, we will be attaching the policy which we created above, to the role we created in the first step.

- Create an Instance Profile and interpolate the IAM Role

![9](https://user-images.githubusercontent.com/93729559/171622202-2f6281fe-4d5c-4fb8-9efd-73630f57a2b5.png)


**CREATE SECURITY GROUPS**

We are going to create all the security groups in a single file, then we are going to refrence this security group within each resources that needs it.


![10](https://user-images.githubusercontent.com/93729559/171623306-6b6f477a-20bd-44ce-baa3-c259d25d2339.png)


**CREATE CERTIFICATE FROM AMAZON CERIFICATE MANAGER**

Creating cert.tf file and adding the coding


![11](https://user-images.githubusercontent.com/93729559/171623843-08eb222f-f8a6-41f3-a8f3-e9dede6ec008.png)



**Create an external (Internet facing) Application Load Balancer (ALB)**

Create a file called alb.tf

First of all we will create the ALB, then we create the target group and lastly we will create the lsitener rule.

![12](https://user-images.githubusercontent.com/93729559/171624432-5771ad1b-1fd8-409b-a397-38c19a298464.png)


**CREATING AUSTOALING GROUPS**

Now we need to configure our ASG to be able to scale the EC2s out and in depending on the application traffic.

Before we start configuring an ASG, we need to create the launch template and the the AMI needed. For now we are going to use a random AMI from AWS.

- Create asg-bastion-nginx.tf and add the codes
- create notification for all the auto scaling groups
- launch template for bastion

![13](https://user-images.githubusercontent.com/93729559/171626339-93ccfe76-bbbd-405c-bc71-3965c133d5e3.png)


**Autoscaling for wordpress and tooling will be created in a seperate file**

- Create asg-wordpress-tooling.tf and the codes

![14](https://user-images.githubusercontent.com/93729559/171626851-7220318c-2cb3-4778-8d01-9f64a9297081.png)


**STORAGE AND DATABASE**

- Create Elastic File System (EFS)
- 
In order to create an EFS you need to create a KMS key.

AWS Key Management Service (KMS) makes it easy for you to create and manage cryptographic keys and control their use across a wide range of AWS services and in your applications.

Add the code to efs.tf

![15](https://user-images.githubusercontent.com/93729559/171651620-00a2c82d-8bee-4beb-8663-607673ac7704.png)









































