
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


