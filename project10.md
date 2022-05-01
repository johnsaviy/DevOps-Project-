
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



-Reloading Nginx

![6](https://user-images.githubusercontent.com/93729559/166130733-612430eb-d116-46ab-ac37-a9a0cafc4944.png)


<br>


#### Step 2 - REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES


- Register a new domain name , Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
- Update A record in your registrar to point to Nginx LB using Elastic IP address

![7](https://user-images.githubusercontent.com/93729559/166131087-00ca34ec-9e4a-4f24-969f-49af78ea8816.png)

![8](https://user-images.githubusercontent.com/93729559/166131088-a469c1b3-3f79-424c-b67f-ad63552bb48e.png)

![9](https://user-images.githubusercontent.com/93729559/166131090-593e1096-fde6-4cdd-960a-34126c9fb5c1.png)

![10](https://user-images.githubusercontent.com/93729559/166131091-2551d846-e7c2-4708-a21c-a02b12698a79.png)

![11](https://user-images.githubusercontent.com/93729559/166131092-ff462ff8-93cb-4c3a-a610-595bc64014c0.png)



- Updated the nginx.conf with my domain name

![3](https://user-images.githubusercontent.com/93729559/166131189-e4369a58-14d4-45a3-9f2f-6b9c9a34f855.png)



- Checking that the Web Servers can be reached from the browser using new domain name using HTTP protocol – http://<your-domain-name.com>


![12](https://user-images.githubusercontent.com/93729559/166131247-bdda09b1-3224-45ad-bb4a-fa7300af60f2.png)



- Install certbot and request for an SSL/TLS certificate

![13](https://user-images.githubusercontent.com/93729559/166131412-0c871817-f88f-4001-a417-771515b209ca.png)

![14](https://user-images.githubusercontent.com/93729559/166131413-db29b7a0-e243-44a3-ae12-72bf06738e29.png)

![15](https://user-images.githubusercontent.com/93729559/166131416-be18e6c2-3009-4ea4-a65b-f6d68c6cb4a3.png)


<br>


- Request your certificate

![16](https://user-images.githubusercontent.com/93729559/166131634-f26374bb-7c14-4a07-84f5-8087ca5a960e.png)
![17](https://user-images.githubusercontent.com/93729559/166131636-f8f073e9-242a-47af-a6ab-9557e0d6b52f.png)
![18](https://user-images.githubusercontent.com/93729559/166131637-3d48dff3-a21e-4222-98de-198fa797bec3.png)
![19](https://user-images.githubusercontent.com/93729559/166131639-9d831deb-3d9c-4f2d-80c7-1177048a1479.png)

<br>

- Test secured access to the Web Solution by reaching the domain name on the browser.

![20](https://user-images.githubusercontent.com/93729559/166131640-9d218307-ac84-4aa7-ba09-4e175eb8d43e.png)


<br>

- Best pracice is to have a scheduled job that renew command periodically. Let us configure a cronjob.

![21](https://user-images.githubusercontent.com/93729559/166131872-c7f69739-b6a0-4ca9-ba41-9e636e1d3ec7.png)
![22](https://user-images.githubusercontent.com/93729559/166131874-b31d7415-c81b-4670-950b-86f5b6038a27.png)

![23](https://user-images.githubusercontent.com/93729559/166131877-24eea1bd-6562-44c8-ae0c-c6a12be75737.png)




#### I have just implemented an Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.






































