
## AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 1


In the previous project(project 15)  I built AWS infrastructure for 2 websites manually, it is time to automate the process using Terraform.

Let us start building the same set up with the power of Infrastructure as Code (IaC)


![tooling](https://user-images.githubusercontent.com/93729559/171012628-17a28327-1476-440b-8081-502be18396ec.png)


<br>

Before writing the terraform code, Lets do the following:

- Create an IAM user, name it terraform (ensure that the user has only programatic access to your AWS account) and grant this user AdministratorAccess permissions.


- Copy the secret access key and access key ID. Save them in a notepad temporarily.


- Configure programmatic access from your workstation to connect to AWS using the access keys copied above and a Python SDK (boto3). You must have Python 3.6 or higher on your workstation.

- Create an S3 bucket to store Terraform state file. You can name it something like <yourname>-dev-terraform-bucket
  
  ![1a](https://user-images.githubusercontent.com/93729559/171137603-f431621c-547c-41fc-a8d5-060274e979bb.png)

  ![1](https://user-images.githubusercontent.com/93729559/171137609-3cd869d9-dce2-436a-8971-0af101cbb4ea.png)
  
  ![2](https://user-images.githubusercontent.com/93729559/171137606-0ef8a678-3a99-4541-bf52-4836dcd59288.png)

  

### VPC | Subnets | Security Groups.
  
Let us create a directory structure. Open your Visual Studio Code and:

- Create a folder called PBL
- Create a file in the folder, name it main.tf
  
  
![1](https://user-images.githubusercontent.com/93729559/171162048-e50e5a27-6b27-4c63-8156-61aa31a751eb.png)

  
### Setup Provider and VPC resource section

- Add AWS as a provider, and a resource to create a VPC in the main.tf file.
- Provider block informs Terraform that we intend to build infrastructure within AWS.
- Resource block will create a VPC.
  
![2](https://user-images.githubusercontent.com/93729559/171163546-3d06c8f1-29e0-4d27-b425-306aa461a722.png)
  
  
The next thing we need to do, is to download necessary plugins for Terraform to work. These plugins are used by providers and provisioners. At this stage, we only have provider in our main.tf file. So, Terraform will just download plugin for AWS provider.
Lets accomplish this with terraform init command as seen in the below demonstration.
  
  **Observations**:

Notice that a new directory has been created: .terraform\.... This is where Terraform keeps plugins. Generally, it is safe to delete this folder. It just means that you must execute terraform init again, to download them.
Moving on, let us create the only resource we just defined. aws_vpc. But before we do that, we should check to see what terraform intends to create before we tell it to go ahead and create it.

Run terraform plan, 

![3](https://user-images.githubusercontent.com/93729559/171360829-7a5641f7-5cf4-4511-9842-6b435bc43718.png)
  
- Subnets resource section
According to our architectural design, we require 6 subnets:

- 2 public
- 2 private for webservers
- 2 private for data layer

  Let us create the first 2 public subnets.

Add below configuration to the main.tf file:
  
![4](https://user-images.githubusercontent.com/93729559/171362861-71a6c33a-1c78-49c3-8673-36d0faf13667.png)
  
  
- We are creating 2 subnets, therefore declaring 2 resource blocks – one for each of the subnets.
- We are using the vpc_id argument to interpolate the value of the VPC id by setting it to aws_vpc.main.id. This way, Terraform knows inside which VPC to create the subnet.  
  
- Run terraform plan and terraform apply
  
 ![5](https://user-images.githubusercontent.com/93729559/171366615-bbc559ea-81df-471b-9596-64b49f3fb035.png)
  
  - The VPC and subnets created!
  
![6](https://user-images.githubusercontent.com/93729559/171366616-2054ca41-360e-44da-946e-0d421ed45f44.png)
  
![6a](https://user-images.githubusercontent.com/93729559/171366618-3c0f6401-fc37-486c-b150-0fab76f54c99.png)

  
  
 Observations:

Hard coded values: Remember our best practice hint from the beginning? Both the availability_zone and cidr_block arguments are hard coded. We should always endeavour to make our work dynamic.
Multiple Resource Blocks: Notice that we have declared multiple resource blocks for each subnet in the code. This is bad coding practice. We need to create a single resource block that can dynamically create resources without specifying multiple blocks. Imagine if we wanted to create 10 subnets, our code would look very clumsy. So, we need to optimize this by introducing a count argument.
Now let us improve our code by refactoring it.

First, destroy the current infrastructure. Since we are still in development, this is totally fine. Otherwise, DO NOT DESTROY an infrastructure that has been deployed to production.

To destroy whatever has been created run terraform destroy command, and type yes after evaluating the plan.
  
![7](https://user-images.githubusercontent.com/93729559/171367453-bffd54cf-4ec7-4d3e-a1fe-e16511814f88.png)

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  


  
  
  
  
  
  
  
  
  
  
  

