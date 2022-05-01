
### LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

So far we have seen what Load Balancing is used for and have configured an LB solution using Apache, but a DevOps engineer must be a versatile professional 
and know different alternative solutions for the same problem. That is why, in this project I will configure an Nginx Load Balancer solution.

It is also extremely important to ensure that connections to your Web solutions are secure and information is encrypted in transit – we will also cover 
connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.

When data is moving between a client (browser) and a Web Server over the Internet – it passes through multiple network devices and, if the data is not encrypted, 
it can be relatively easy intercepted by someone who has access to the intermediate equipment. 
This kind of information security threat is called Man-In-The-Middle (MIMT) attack.

This threat is real – users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, 
risk their data to be compromised and used by cybercriminals.

SSL and its newer version, TSL – is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. 
Here we will refer this family of cryptographic protocols as SSL/TLS – even though SSL was replaced by TLS, the term is still being widely used.

SSL/TLS uses digital certificates to identify and validate a Website. A browser reads the certificate issued by a Certificate Authority (CA) to make sure that the 
website is registered in the CA so it can be trusted to establish a secured connection.


In this project I will register the website with LetsEnrcypt Certificate Authority, to automate certificate issuance I will use a shell client recommended 
by LetsEncrypt – cetrbot.

#### Task

This project consists of two parts:

- Configure Nginx as a Load Balancer

- Register a new domain name and configure secured connection using SSL/TLS certificates


- Your target architecture will look like this:


![nginx_lb](https://user-images.githubusercontent.com/93729559/166106787-da4fa8c2-9a9b-464d-b1eb-c6a3b396b279.png)


<br>

#### Step 1 - CONFIGURE NGINX AS A LOAD BALANCER

- Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)


- Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses


- Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

- Update the instance and Install Nginx

![1](https://user-images.githubusercontent.com/93729559/166130439-8c606740-c406-452b-9998-a13da13d2885.png)



- Start Enable and Check status of Nginx
 
![2](https://user-images.githubusercontent.com/93729559/166130504-dd5029f7-9f80-41c2-b09f-49be1c4a28e4.png)


- Configure Nginx LB using Web Servers’ names defined in /etc/hosts

![3](https://user-images.githubusercontent.com/93729559/166130729-9e066ae5-f3cf-4fcc-b01b-afc10616ee97.png)

![4](https://user-images.githubusercontent.com/93729559/166130730-617d302c-aee0-48d5-82cc-4d6b61238d2c.png)

![5](https://user-images.githubusercontent.com/93729559/166130731-6d32bf38-6eed-448f-a044-49f0263fd88a.png)



-Restart Nginx and make sure the service is up and running

![6](https://user-images.githubusercontent.com/93729559/166130733-612430eb-d116-46ab-ac37-a9a0cafc4944.png)

































