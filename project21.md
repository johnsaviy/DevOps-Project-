## Orchestrating containers across multiple Virtual Servers with Kubernetes. Part 1


- Tools to be used and expected result of the Project 20

- VM: AWS EC2
- OS: Ubuntu 20.04 lts+
- Docker Engine
- kubectl console utility
- cfssl and cfssljson utilities
- Kubernetes cluster

- We will create 3 EC2 Instances, and in the end, we will have the following parts of the cluster properly configured:

- One Kubernetes Master
- Two Kubernetes Worker Nodes
- Configured SSL/TLS certificates for Kubernetes components to communicate securely
- Configured Node Network
- Configured Pod Network



#### STEP 0-INSTALL CLIENT TOOLS BEFORE BOOTSTRAPPING THE CLUSTER

#### Install kubectl
 
Kubernetes cluster has a Web API that can receive HTTP/HTTPS requests, but it is quite cumbersome to curl an API each and every time you need to send some command, so kubectl command tool was developed to ease a K8s administrator’s life.

With this tool you can easily interact with Kubernetes to deploy applications, inspect and manage cluster resources, view logs and perform many more administrative operations.

![1](https://user-images.githubusercontent.com/93729559/176220018-775a0c86-29cc-45bf-87bf-d29c701832c9.png)



#### Install CFSSL and CFSSLJSON

cfssl is an open source tool by Cloudflare used to setup a Public Key Infrastructure (PKI Infrastructure) for generating, signing and bundling TLS certificates. In previous projects you have experienced the use of Letsencrypt for the similar use case. Here, cfssl will be configured as a Certificate Authority which will issue the certificates required to spin up a Kubernetes cluster.


![2](https://user-images.githubusercontent.com/93729559/176220026-16fd0445-429e-4112-be22-c7b2bb6571a3.png)


#### AWS CLOUD RESOURCES FOR KUBERNETES CLUSTER

#### Step 1 – Configure Network Infrastructure- Virtual Private Cloud – VPC

- Create a directory named k8s-cluster-from-ground-up

- Create a VPC and store the ID as a variable:

- Tag the VPC so that it is named: k8s-cluster-from-ground-up

- Enable DNS support for your VPC

- Enable DNS support for hostnames



![3](https://user-images.githubusercontent.com/93729559/176235142-f8bc1810-f5a9-47bb-8a33-1f500cf37fdf.png)

![4](https://user-images.githubusercontent.com/93729559/176235368-e0eaf243-0a63-4378-b4f3-ad4434698723.png)

<br>

#### Dynamic Host Configuration Protocol – DHCP

- Configure DHCP Options Set:

![5](https://user-images.githubusercontent.com/93729559/176240559-8844b3b0-cae3-4f51-95f8-4e883d577aa2.png)

![5b](https://user-images.githubusercontent.com/93729559/176240881-ada57cbc-8a63-4f6d-b3df-ffe6b6b05358.png)

![5b](https://user-images.githubusercontent.com/93729559/176241745-b07475db-fb0d-4a23-a71d-9012e7c2cc8f.png)



#### Create the Subnet

![6](https://user-images.githubusercontent.com/93729559/176242982-b33249d2-d496-46c7-a913-f7f4d1205fe0.png)


#### Create the Internet Gateway and attach it to the VPC:

![7](https://user-images.githubusercontent.com/93729559/176243061-e7354d17-c6f7-48d0-a432-29063f08714c.png)

#### Create route tables, associate the route table to subnet, and create a route to allow external traffic to the Internet through the Internet Gateway:

![8](https://user-images.githubusercontent.com/93729559/176243637-0e6b2175-f5d7-4069-a0fa-2fe54f8150f3.png)


#### Configure security groups

![9](https://user-images.githubusercontent.com/93729559/176244187-94af7542-a26d-4ab6-adfe-e66eb781b874.png)


![10](https://user-images.githubusercontent.com/93729559/176245116-ad459484-c5cb-4c98-81f0-4b12b17a8431.png)

![11](https://user-images.githubusercontent.com/93729559/176245138-1fa57815-009b-4230-9865-11b6b9f75bf4.png)

![12](https://user-images.githubusercontent.com/93729559/176245157-ca704898-bcca-4960-b1eb-d5678d0df18c.png)

![15](https://user-images.githubusercontent.com/93729559/176246731-3c61a859-a5e0-48df-a5d6-1083bc544d73.png)



<br>

#### Create a network Load balancer

![13](https://user-images.githubusercontent.com/93729559/176245848-e6cc2cff-4ff2-4cc0-9ddc-c1057079c11d.png)

![16](https://user-images.githubusercontent.com/93729559/176247864-1fc0871e-9ee8-4be9-9664-6203be379653.png)




#### Create a target group:

![14](https://user-images.githubusercontent.com/93729559/176245889-dd4eff2e-5893-463c-ad6c-3ac4530442be.png)

![17](https://user-images.githubusercontent.com/93729559/176247880-e8daa2bc-b3d1-4ab5-aacc-2a4fd77235c0.png)



#### Register targets:

![18](https://user-images.githubusercontent.com/93729559/176248398-189dd774-cf43-48eb-90bb-33eea21097b7.png)


![22](https://user-images.githubusercontent.com/93729559/176249496-87bee669-17bb-4d4c-bb6b-7dadc5c20f38.png)



#### Create a listener to listen for requests and forward to the target nodes on TCP port 6443


![19](https://user-images.githubusercontent.com/93729559/176248672-f3a5dbb9-faf3-4672-b5be-9281c3fdb2a4.png)

![21](https://user-images.githubusercontent.com/93729559/176249475-7c582f72-ea09-46c3-84ba-f90a7d137a2c.png)


#### Get the Kubernetes Public address


![20](https://user-images.githubusercontent.com/93729559/176249080-ffcd6236-4d8e-4227-a029-74eab6079366.png)










