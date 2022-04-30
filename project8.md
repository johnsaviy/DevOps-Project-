
#### LOAD BALANCER SOLUTION WITH APACHE.

After completing Project-7 you might wonder how a user will be accessing each of the webservers using 3 different IP addreses or 3 different DNS names.
You might also wonder what is the point of having 3 different servers doing exactly the same thing.

When we access a website in the Internet we use an URL and we do not really know how many servers are out there serving our requests, this complexity 
is hidden from a regular user, but in case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to
serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs).

Each URL contains a domain name part, which is translated (resolved) to IP address of a target server that will serve requests when open a website in the Internet.
Translation (resolution) of domain names is perormed by DNS servers, the most commonly used one has a public IP address 8.8.8.8 and belongs to Google.
You can try to query it with nslookup command.


When you have just one Web server and load increases – you want to serve more and more customers, you can add more CPU and RAM or completely replace the server with a 
more powerful one – this is called "vertical scaling". This approach has limitations – at some point you reach the maximum capacity of CPU and RAM that can be 
installed into your server.

Another approach used to cater for increased traffic is "horizontal scaling" – distributing load across multiple Web servers. 
This approach is much more common and can be applied almost seamlessly and almost infinitely (you can imagine how many server Google has
to serve billions of search requests).

Horizontal scaling allows to adapt to current load by adding (scale out) or removing (scale in) Web servers. 
Adjustment of number of servers can be done manually or automatically (for example, based on some monitored metrics like CPU and Memory load).

Property of a system (in our case it is Web tier) to be able to handle growing load by adding resources, is called "Scalability".

In our set up in Project-7 we had 3 Web Servers and each of them had its own public IP address and public DNS name. 
A client has to access them by using different URLs, which is not a nice user experience to remember addresses/names of even 3 server, 
let alone millions of Google servers.

In order to hide all this complexity and to have a single point of access with a single public IP address/name, a Load Balancer can be used. 
A Load Balancer (LB) distributes clients’ requests among underlying Web Servers and makes sure that the load is distributed in an optimal way.


Let us take a look at the updated solution architecture with an LB added on top of Web Servers 
(for simplicity let us assume it is a software L7 Application LB, for example – Apache, NGINX or HAProxy).


![Tooling-Website-Infrastructure-wLB](https://user-images.githubusercontent.com/93729559/166094353-0c581534-ecf0-40a5-92cb-6535a3d0f2b3.png)



In this project we will enhance our Tooling Website solution by adding a Load Balancer to disctribute traffic between Web Servers 
and allow users to access our website using a single URL.

#### Task

- Deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 intance. 
- Make sure that users can be served by Web servers through the Load Balancer.

 To simplify, let us implement this solution with 2 Web Servers, the approach will be the same for 3 and more Web Servers.
 
 
 - Prerequisites.
 - 
    Make sure that you have following servers installed and configured within Project-7:

  -  Two RHEL8 Web Servers
  -  One MySQL DB Server (based on Ubuntu 20.04)
  -  One RHEL8 NFS server


![prerequisites-project8](https://user-images.githubusercontent.com/93729559/166094516-e841c0bd-250b-45f6-9ec4-ece588f235f3.png)



#### Step 1 - Configure Apache As A Load Balancer


- Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so the EC2 list will look like this:

![1](https://user-images.githubusercontent.com/93729559/166095007-e9871130-3acc-4cfc-82fd-026ce495c63c.png)



- Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.

![2](https://user-images.githubusercontent.com/93729559/166095197-68e9d265-a6b3-43eb-aaa9-d1b90685bd89.png)



- Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:

![3](https://user-images.githubusercontent.com/93729559/166096254-65358c0f-0295-4a43-80f7-262560978fa1.png)

![4](https://user-images.githubusercontent.com/93729559/166096257-52c9abee-3edb-4824-914b-209d1122a102.png)

![5](https://user-images.githubusercontent.com/93729559/166096258-5ea68bbc-25db-4554-b7e3-627582dfddd3.png)



- Making sure apache2 is up and running!

![6](https://user-images.githubusercontent.com/93729559/166096304-786a34c3-886e-4b85-977c-c44075df7570.png)


- Configure load balancing

![7](https://user-images.githubusercontent.com/93729559/166096791-734c9c88-2860-4e95-a1f2-9b02cde865a1.png)



- Verifying that the configuration works – try to access the LB’s public IP address or Public DNS name from the browser:

![8](https://user-images.githubusercontent.com/93729559/166097102-9a65dcc9-d0d8-4aef-b0ae-3175f6f4ed88.png)


- Open two ssh consoles for both Web Servers and run following command: sudo tail -f /var/log/httpd/access_log

![9](https://user-images.githubusercontent.com/93729559/166097363-d9ea8680-ddc7-4874-9de0-69ea218003ee.png)


Try to refresh your browser page http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php several times and make sure that both servers
 receive HTTP GET requests from your LB – new records must appear in each server’s log file. 
 The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers – it means that traffic will be disctributed evenly between them.

 

![10](https://user-images.githubusercontent.com/93729559/166097466-9a79d1fc-a0cb-490f-8a85-e23e11872245.png)
 
 If you have configured everything correctly – your users will not even notice that their requests are served by more than one server.
 
 
 - I have just implemented a Load balancing Web Solution for a DevOps team!!



















