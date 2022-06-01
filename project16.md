
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

  

  **Fixing The Problems By Code Refactoring**
  
  Fixing Hard Coded Values: We will introduce variables, and remove hard coding.

- Starting with the provider block, declare a variable named region, give it a default value, and update the provider section by referring to the declared variable.
  
- Do the same to cidr value in the vpc block, and all the other arguments.
  
  
 ![8](https://user-images.githubusercontent.com/93729559/171370362-ba894111-f695-4b45-8394-8a59436f6e08.png)

  

  - Fixing multiple resource blocks: This is where things become a little tricky. It’s not complex, we are just going to introduce some interesting concepts. Loops & Data sources

Terraform has a functionality that allows us to pull data which exposes information to us. For example, every region has Availability Zones (AZ). Different regions have from 2 to 4 Availability Zones. With over 20 geographic regions and over 70 AZs served by AWS, it is impossible to keep up with the latest information by hard coding the names of AZs. Hence, we will explore the use of Terraform’s Data Sources to fetch information outside of Terraform. In this case, from AWS

Let us fetch Availability zones from AWS, and replace the hard coded value in the subnet’s availability_zone section.
  
![9](https://user-images.githubusercontent.com/93729559/171372319-0fd5cffb-aa75-4649-a7ad-47145639df14.png)
  
  
 - To make use of this new data resource, we will need to introduce a count argument in the subnet block: Something like this. 
  
![10](https://user-images.githubusercontent.com/93729559/171372328-e55727fe-9ec5-42d9-9d98-bbfb3255b4e9.png)

  
  
Let us quickly understand what is going on here.

The count tells us that we need 2 subnets. Therefore, Terraform will invoke a loop to create 2 subnets.
The data resource will return a list object that contains a list of AZs. Internally, Terraform will receive the data like this
  ["eu-central-1a", "eu-central-1b"]
Each of them is an index, the first one is index 0, while the other is index 1. If the data returned had more than 2 records, then the index numbers would continue to increment.

Therefore, each time Terraform goes into a loop to create a subnet, it must be created in the retrieved AZ from the list. Each loop will need the index number to determine what AZ the subnet will be created. That is why we have data.aws_availability_zones.available.names[count.index] as the value for availability_zone. When the first loop runs, the first index will be 0, therefore the AZ will be eu-central-1a. The pattern will repeat for the second loop.

But we still have a problem. If we run Terraform with this configuration, it may succeed for the first time, but by the time it goes into the second loop, it will fail because we still have cidr_block hard coded. The same cidr_block cannot be created twice within the same VPC. So, we have a little more work to do.
  

  - Let’s make cidr_block dynamic.
  
 We will introduce a function cidrsubnet() to make this happen. It accepts 3 parameters. Let us use it first by updating the configuration.
  
 ![11](https://user-images.githubusercontent.com/93729559/171375030-f95ba2ac-64c1-4077-a2ba-63fae446b710.png)

  
- The final problem to solve is removing hard coded count value.
  
If we cannot hard code a value we want, then we will need a way to dynamically provide the value based on some input. Since the data resource returns all the AZs within a region, it makes sense to count the number of AZs returned and pass that number to the count argument.

To do this, we can introuduce length() function, which basically determines the length of a given list, map, or string.

Since data.aws_availability_zones.available.names returns a list like ["eu-central-1a", "eu-central-1b", "eu-central-1c"] we can pass it into a lenght function and get number of the AZs.

length(["eu-central-1a", "eu-central-1b", "eu-central-1c"])  
  
![12](https://user-images.githubusercontent.com/93729559/171375463-b888c7d2-052d-4fd9-93f3-0fd05bed66c4.png)
  
  
**Observations:** 

What we have now, is sufficient to create the subnet resource required. But if you observe, it is not satisfying our business requirement of just 2 subnets. The length function will return number 3 to the count argument, but what we actually need is 2.
Now, let us fix this.

- Declare a variable to store the desired number of public subnets, and set the default value.
  
 ![13](https://user-images.githubusercontent.com/93729559/171376296-dcace173-f581-479e-8d03-3f55a4e0b131.png)
  
- Next, update the count argument with a condition. Terraform needs to check first if there is a desired number of subnets. Otherwise, use the data returned by the length function. See how that is presented below.
  

![14](https://user-images.githubusercontent.com/93729559/171376300-1c7fc981-d4d1-4195-a846-61ad07f840ad.png)
  
  
**Introducing variables.tf & terraform.tfvars**
  
Instead of havng a long lisf of variables in main.tf file, we can actually make our code a lot more readable and better structured by moving out some parts of the configuration content to other files.

- We will put all variable declarations in a separate file And provide non default values to each of them
- Create a new file and name it variables.tf
- Copy all the variable declarations into the new file.
- Create another file, name it terraform.tfvars
- Set values for each of the variables.

![15](https://user-images.githubusercontent.com/93729559/171380105-d2908e1c-7649-40ab-811a-628565a6b10b.png)

  


  
  
  
  
  
  
  
  
  
  
  

