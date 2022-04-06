
## LEMP STACK IMPLEMENTATION IN AWS
In this project I will implement a similar stack to LAMP STACK, but with an alternative Web Server – <b>NGINX<b>, which is also very popular and 
widely used by many websites in the Internet.

- STEP 1 - CONNECTING TO VIRTUAL SERVER IN AWS.

I created a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image in AWS and connected to my local terminal.
  
![img](https://user-images.githubusercontent.com/93729559/162020347-f58af506-93f3-496a-b7c5-f4b3093c306c.png)
  
  
- STEP 2 – Installing the Nginx Web Server.
  
In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to install this package.

![Img2](https://user-images.githubusercontent.com/93729559/162021852-363fef3a-3ea1-49bd-8471-057a97139b94.png)
  
- Verifying that nginx was successfully installed and is running as a service in Ubuntu.
  
![img 2b](https://user-images.githubusercontent.com/93729559/162022822-23fe8f6c-b525-48ef-838e-4f0aed4eb31b.png)
  
  
- Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is default port that web brousers use to access web pages in the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH,
so we need to add a rule to EC2 configuration to open inbound connection   through port 80:
  
![img 2c](https://user-images.githubusercontent.com/93729559/162023623-ebe975b6-c93b-4126-be62-01fa5abdfeb3.png)
  
- Our server is running and we can access it locally and from the Internet.
  Checking how we can access it locally in our Ubuntu shell.
  
![img 2d](https://user-images.githubusercontent.com/93729559/162024459-630abf50-3322-426b-87d3-c57ea00fd87c.png)
  
  
- Testing how the Nginx server can respond to requests from the Internet on the browser.
  
![2e](https://user-images.githubusercontent.com/93729559/162025338-3418f23e-267d-4f62-8bfa-8770a0ce6ba9.png)
  
- STEP 3 — Installing MySQL
Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in this project.

Using apt to acquire and install the MySQL software.

![3](https://user-images.githubusercontent.com/93729559/162026662-edc5b19d-1c6a-4100-8b61-bf38ddaf084b.png)

- Running a security script that comes pre-installed with MySQL. 
  This script will remove some insecure default settings and lock down access to your database system. Start the interactive script by running:
  
 ![3a](https://user-images.githubusercontent.com/93729559/162027630-0f4bb08c-b319-471f-a1b0-442ef7223335.png)

-  Test if I'm able to log in to the MySQL console.
  
![3b](https://user-images.githubusercontent.com/93729559/162028307-90f20171-e064-4bdf-87f6-ad1493db4ed5.png)
  
  MySQL server is now installed and secured.
  
  
- Step 4 – Installing PHP
  
I have Nginx installed to serve your content and MySQL installed to store and manage your data. 
Now I can install PHP to process code and generate dynamic content for the web server.
While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. I’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, I’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
  
![4](https://user-images.githubusercontent.com/93729559/162030069-3ace7faf-4f20-4605-8bd6-5ae6fe84b0cd.png)


- Step 5 — Configuring Nginx to Use PHP Processor
When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. Here, I will use projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. 
While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. 
Instead of modifying /var/www/html, I’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

Creating the root web directory for the domain as follows:
  
![5](https://user-images.githubusercontent.com/93729559/162034962-40a0d1d1-88e4-49e4-bf82-bee9f87defef.png)

![5a](https://user-images.githubusercontent.com/93729559/162034999-107edbf4-70cb-4fbf-90df-b55e59c5d5f8.png)

![5b](https://user-images.githubusercontent.com/93729559/162035021-45fb9344-b555-46da-b914-3cd150f7a208.png)

![5c](https://user-images.githubusercontent.com/93729559/162035036-edba1910-8df3-4d89-8752-5d1d862ec8fb.png)

   LEMP stack is now fully configured!
  


  
  

  



