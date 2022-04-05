## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

What is a Technology stack?
A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. some examples are…

LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
MEAN (MongoDB, ExpressJS, AngularJS, NodeJS 



 <b>STEP 1 - CREATE VIRTUAL SERVER ON AWS<b> <br>
 First I created  a virtual server(EC2) with Linux (Ubuntu) Server OS  in AWS Cloud. Thereafter i connected to the EC2 terminal using my local terminal.
 
![image 1](https://user-images.githubusercontent.com/93729559/161779418-70051fba-02e7-40b7-b9cc-eea06836b559.jpg)
 
 
 
 STEP 2 -INSTALLING APACHE AND UPDATING THE FIREWALL <br>
 Installed Apache using Ubuntu’s package manager ‘apt’:
 
 ![img 2](https://user-images.githubusercontent.com/93729559/161781669-4a182c7a-5fce-4a7d-ba6a-7d74f1f9ba6b.png)
 
 - Verifying that apache2 is running as a Service in the OS

![img2b](https://user-images.githubusercontent.com/93729559/161782007-dd69e3e7-0ca7-4b1e-9df8-d9e3cb90b749.png)
  
  - Opened TCP port 80 which is the default port that web browsers use to access web pages on the Internet
  
  ![img3](https://user-images.githubusercontent.com/93729559/161784459-2fd02e80-be6b-4266-bce4-422a7a6df610.png)
  
  - Checking if we can access the server locally in the Ubuntu shell
  
  ![img4](https://user-images.githubusercontent.com/93729559/161786111-31178971-0ec2-44bc-821b-0ff42e50a80e.png)
  
  - Testing how the Apache HTTP server can respond to requests from the Internet on the web browser by accessing it through the public ip and specifying the port number 80.

  ![img 5](https://user-images.githubusercontent.com/93729559/161788168-04f92b99-d3f8-4a5e-b497-be4cea235418.png)
  
  <b>STEP 3 - INSTALLING MYSQL<b>
   
   - Using ‘apt’ to acquire and install the MYSQL software
   
   ![img 6](https://user-images.githubusercontent.com/93729559/161820343-430c18ce-d87e-4a93-b4a9-ba12f52d1e07.png)
   
   - Running a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database      system.
   
   ![img6b](https://user-images.githubusercontent.com/93729559/161822154-d8c28383-d61c-402b-956a-fe4fdef9a060.png)
   ![img6c](https://user-images.githubusercontent.com/93729559/161822172-00c0c9ec-cd94-483c-bf70-4d0e418e52f1.png)
   
   - Testing if i'm able to log in to the MySQL console.
   
   ![img6d](https://user-images.githubusercontent.com/93729559/161823226-e106d5a1-b9ba-49d5-b9bc-3dc49ce5be9d.png)
   
   
   <b>STEP 4 — INSTALLING PHP<b>
    
 Since have Apache installed to serve your content and MySQL installed to store and manage your data. 
    PHP is the component of our setup that will process code to display dynamic content to the end user. 
    In addition to the php package, I’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. 
    I’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
    
 Installing the three packages at once.
 
 ![img 7](https://user-images.githubusercontent.com/93729559/161825136-92f3f580-7954-450d-8bea-2d63ee37ce82.png)
    
-Confirming PHP version.
 
![img 7b](https://user-images.githubusercontent.com/93729559/161825736-f4706320-d41e-4030-96b3-4aaa070136d4.png)
    
  <b> STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE<b>
   
- To test the setup with a PHP script, it’s best to set up a proper Apache Virtual Host to hold your website’s files and folders. Virtual host allows you to have multiple websites located on a single machine.
   
   In this project, I will set up a domain called projectlamp.
   Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.
We will leave this configuration as is and will add our own directory next next to the default one.

Creating the directory for projectlamp using ‘mkdir’ command as follows:
 Next, assigning ownership of the directory with the current system user:
 Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vi.
 This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
  
   
![img 7c](https://user-images.githubusercontent.com/93729559/161830084-10b4e632-2a07-4379-8b61-15e6f819f1bd.png)
![img 7d](https://user-images.githubusercontent.com/93729559/161830106-a7989cb2-80c4-4966-b07b-22be6db17423.png)
   
With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlampl as its web root directory.
   
- Enabling the new virtual host:
  Disabling the default website that comes installed with Apache.
  Making sure your configuration file doesn’t contain syntax errors.
  And finally, reload Apache so these changes take effect.
   
 ![img 7e](https://user-images.githubusercontent.com/93729559/161833314-4312fd8a-9767-4e39-9096-7d3f6df4386f.png)
   
 The new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location 
 so that we can test that the virtual host works as expected:
 
 ![img 7f](https://user-images.githubusercontent.com/93729559/161834062-3a935858-d530-424b-aff1-8cabe8b64f04.png)
   
 -  The browser displays the ‘echo’ command I wrote to index.html file, meaning the Apache virtual host is working as expected.
   
   ![img 7g](https://user-images.githubusercontent.com/93729559/161835275-5e8520e1-6039-47fc-be8b-d1a43dc8c21d.png)
   
   
 
   


    
   
   

  
  

