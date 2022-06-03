## AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 3 – REFACTORING


In two previous projects I have developed AWS Infrastructure code using Terraform and tried to run it from local workstation.
Now it is time to introduce some more advanced concepts and enhance our code.


Introducing Backend on S3
Each Terraform configuration can specify a backend, which defines where and how operations are performed, where state snapshots are stored, etc.
Take a peek into what the states file looks like. It is basically where terraform stores all the state of the infrastructure in json format.

So far, we have been using the default backend, which is the local backend – it requires no configuration, and the states file is stored locally. This mode can be suitable for learning purposes, but it is not a robust solution, so it is better to store it in some more reliable and durable storage.

The second problem with storing this file locally is that, in a team of multiple DevOps engineers, other engineers will not have access to a state file stored locally on your computer.

To solve this, we will need to configure a backend where the state file can be accessed remotely other DevOps team members. There are plenty of different standard backends supported by Terraform that you can choose from. Since we are already using AWS – we can choose an S3 bucket as a backend.

Another useful option that is supported by S3 backend is State Locking – it is used to lock your state for all operations that could write state. This prevents others from acquiring the lock and potentially corrupting your state. State Locking feature for S3 backend is optional and requires another AWS service – DynamoDB.

Let us configure it!

- Here is our plan to Re-initialize Terraform to use S3 backend:

- Add S3 and DynamoDB resource blocks before deleting the local state file
- Update terraform block to introduce backend and locking
- Re-initialize terraform
- Delete the local tfstate file and check the one in S3 bucket
- Add outputs
- terraform apply



- Create a file and name it backend.tf 


- Next, we will create a DynamoDB table to handle locks and perform consistency checks. In previous projects, locks were handled with a local file as shown in terraform.tfstate.lock.info. Since we now have a team mindset, causing us to configure S3 as our backend to store state file, we will do the same to handle locking. Therefore, with a cloud storage database like DynamoDB, anyone running Terraform against the same infrastructure can use a central location to control a situation where Terraform is running at the same time from multiple different people.
Dynamo DB resource for locking and consistency checking:


Terraform expects that both S3 bucket and DynamoDB resources are already created before we configure the backend. So, let us run terraform apply to provision resources.

- Configure S3 Backend

![1](https://user-images.githubusercontent.com/93729559/171876353-a46e3ba8-b8ec-4fcc-8fbc-2b7528c4978d.png)



- DynamoDB table which we create has an entry which includes state file status


I'll navigate to the DynamoDB table inside AWS and leave the page open in the browser, run terraform plan and while that is running, I'll refresh the browser and see how the lock is being handled:


![2 (1)](https://user-images.githubusercontent.com/93729559/171876365-0c1326a0-b68b-4920-8b11-5ae36efa8b34.png)

![2 (2)](https://user-images.githubusercontent.com/93729559/171876370-4aa977fe-b9aa-40ab-a1f1-46d0f4a24507.png)


### Refactoring Terraform code

**Now I'll break down the terraform codes to have all resources in their respective modules and combine resources of a similar type into directories within a ‘modules’ directory**
































