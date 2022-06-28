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











