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


![3](https://user-images.githubusercontent.com/93729559/171879739-f9a32819-3f57-4846-8b42-ca341874eed0.png)

![4](https://user-images.githubusercontent.com/93729559/171879829-56e16e47-22d0-4d0a-a045-9567d8f878e6.png)

![5](https://user-images.githubusercontent.com/93729559/171879834-932c43a0-7958-4750-a083-f0a81c574f75.png)

![6](https://user-images.githubusercontent.com/93729559/171879859-465849fb-2aa1-48c7-a8b1-8d29e1c2e1fa.png)

![7](https://user-images.githubusercontent.com/93729559/171879876-985be30d-98f0-44e0-b4c5-5b285011a427.png)

![8](https://user-images.githubusercontent.com/93729559/171879884-e646c3d2-a9fb-46af-a95d-73d43e1e2a2b.png)

![9](https://user-images.githubusercontent.com/93729559/171879892-8be2eef2-6936-461f-b543-6729740abd4a.png)

![10](https://user-images.githubusercontent.com/93729559/171879899-a1d80b83-8362-4014-b94a-57df6ba1d3ac.png)

![11](https://user-images.githubusercontent.com/93729559/171879921-dd6ba3bd-7c26-451b-a096-bcafc3c39f71.png)

![12](https://user-images.githubusercontent.com/93729559/171879928-4708017f-edb9-4c85-b77b-c2fa11950091.png)

![13](https://user-images.githubusercontent.com/93729559/171879944-644dbd67-05d5-461b-96f4-8ea8b2cfce9f.png)



**After running terrraform plan and apply, we have the infrastructure set up in Aws as it was in previous project 17**


![17](https://user-images.githubusercontent.com/93729559/171880644-fa29c59e-1fd5-4b82-925a-a8d631ec7a09.png)

![17a](https://user-images.githubusercontent.com/93729559/171880648-060a725d-e0ac-41a5-a7c6-6aa4b9a32ff1.png)

![17b](https://user-images.githubusercontent.com/93729559/171880651-eaf26051-30b3-4cfe-aa32-9d531e9d07ca.png)

![17c](https://user-images.githubusercontent.com/93729559/171880653-513caf3f-c1ef-4b2f-9127-1aa21fefdd02.png)

![17d](https://user-images.githubusercontent.com/93729559/171880657-bedfe671-4fe8-48db-92a6-aaf922fb4998.png)

![17e](https://user-images.githubusercontent.com/93729559/171880674-11cae6b5-1767-449b-90b9-13a7e5cc500f.png)

![17f](https://user-images.githubusercontent.com/93729559/171880675-b7f52150-18ff-474e-a49d-25ec522e942c.png)

![17g](https://user-images.githubusercontent.com/93729559/171880676-92a41754-0919-444b-9bcc-d644470992e6.png)

![18](https://user-images.githubusercontent.com/93729559/171880681-f8cbc5b4-da96-41c2-8b38-0d0b2dc6c23b.png)

![19](https://user-images.githubusercontent.com/93729559/171880682-d5c2f8c4-bece-463d-b710-c8bc37fe5b36.png)

![20](https://user-images.githubusercontent.com/93729559/171880690-9e2b3ea5-2892-4f34-ba6e-09ec0a98b22d.png)






























