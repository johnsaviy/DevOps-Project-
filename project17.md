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



















