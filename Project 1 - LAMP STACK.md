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
  
  

