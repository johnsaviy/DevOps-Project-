
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



















