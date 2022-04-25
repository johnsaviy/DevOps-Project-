## WEB SOLUTION WITH WORDPRESS.

In this project I will prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. 
WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

This Project consists of two parts:

1. Configure storage subsystem for Web and Database servers based on Linux OS. 
  Here, I'll be working with disks, partitions and volumes in Linux (REDHAT LINUX DISTRIBUTION).

2. Install WordPress and connect it to a remote MySQL database server.


#### Step 1 — Prepare a Web Server

I'll Launch an EC2 instance in AWS that will serve as "Web Server". Create 3 volumes in the same AZ(Availability zone) as the Web Server EC2, each of 10 GiB.

- Adding EBS(Elastic block store) Volume to the EC2.

![1](https://user-images.githubusercontent.com/93729559/165050103-9a6245dd-cd62-46f5-955f-7794e84d747a.png)

![2](https://user-images.githubusercontent.com/93729559/165050115-811ea934-340a-4926-b108-5351947771c0.png)



