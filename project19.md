## AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 4 – TERRAFORM CLOUD


Terraform Cloud is a managed service that provides you with Terraform CLI to provision infrastructure, either on demand or in response to various events.

**Migrate your .tf codes to Terraform Cloud**

Le us explore how we can migrate our codes to Terraform Cloud and manage our AWS infrastructure from there:

- Create a Terraform Cloud account

- Create an organization


- Configure a workspace

Choose version control workflow and you will be promped to connect your GitHub account to your workspace – follow the prompt and add your newly created repository to the workspace.


- Move on to "Configure settings", provide a description for your workspace and leave all the rest settings default, click "Create workspace".

![1](https://user-images.githubusercontent.com/93729559/174335347-8d74bdbf-d7e7-4616-a230-21ead4dc9dcc.png)



- Configure variables
- 
Terraform Cloud supports two types of variables: environment variables and Terraform variables. Either type can be marked as sensitive, which prevents them from being displayed in the Terraform Cloud web UI and makes them write-only.

Set two environment variables: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY. These credentials will be used to prOvision the AWS infrastructure by Terraform Cloud.


![2](https://user-images.githubusercontent.com/93729559/174335350-885172b0-cfe4-4e88-9a99-596546d8c3de.png)


- Now we'll use Packer to build our images, and Ansible to configure the infrastructure.

- Running packer build for AMI

![3](https://user-images.githubusercontent.com/93729559/174338031-479c5ca5-6ee1-4c5f-bd98-529a7c160b0a.png)


Now the AMI'S are created in AWS

![4](https://user-images.githubusercontent.com/93729559/174338039-6e429ee6-af0f-49ed-8507-8b187d7fe227.png)


- Update terraform script with new AMI ID's generated from the packer build

![5](https://user-images.githubusercontent.com/93729559/174339743-f96cc1eb-e26b-4c15-86f1-c5a33e5686e2.png)


- Include the Ansible files

![1](https://user-images.githubusercontent.com/93729559/174595414-b8ee85fa-29f9-43d2-9b0e-b8b65f347881.png)

![2](https://user-images.githubusercontent.com/93729559/174595428-bff50bcc-4b3f-4927-8da8-24a9e9abcd8f.png)


- Run terraform plan and terraform apply from web console.

Switch to "Runs" tab and click on "Queue plan manualy" button. If planning has been successfull, you can proceed and confirm Apply – press "Confirm and apply", provide a comment and "Confirm plan"

![3](https://user-images.githubusercontent.com/93729559/174603309-2b6f1994-da4f-403a-92d7-872667f436bd.png)

![3a](https://user-images.githubusercontent.com/93729559/174603317-e3fecdcd-433b-4e59-8864-5c02b73d3fc0.png)

![3b](https://user-images.githubusercontent.com/93729559/174603318-2a2d597f-45fb-40a1-8142-d73ba5934191.png)

<br>

- Confirming if the resources were created in AWS

![4](https://user-images.githubusercontent.com/93729559/174606446-945c477d-b7e8-44e5-bd8c-00b4d226266f.png)

![5](https://user-images.githubusercontent.com/93729559/174606461-76facc34-29fe-42ae-bd39-c746cc6c16b9.png)

![6](https://user-images.githubusercontent.com/93729559/174606468-661c29c6-fc49-4e61-84c2-e2eb845e921a.png)

![7](https://user-images.githubusercontent.com/93729559/174606475-271fd472-2eda-47b2-8147-0c7aa31592a6.png)

![8](https://user-images.githubusercontent.com/93729559/174609147-f3535b79-7c91-499f-aa94-65aca34f842e.png)



<br>

- Update Ansible script with values from terraform output.
    - RDS endpoints for wordpress and tooling
    - Database password and username for wordpress and tooling
    - Access point ID for wordpress and tooling
    - Internal load balancer DNS for nginx reverse proxy









