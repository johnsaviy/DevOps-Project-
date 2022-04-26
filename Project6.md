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

<br>


#### - Attaching all three volumes one by one to the Web Server EC2 instance.

![3](https://user-images.githubusercontent.com/93729559/165051002-8b38c377-5c18-45ed-8d58-15be32e618d1.png)

<br>

#### - Using lsblk command to inspect what block devices are attached to the server.

![4](https://user-images.githubusercontent.com/93729559/165203381-44df3363-01f5-4883-b62f-c71a74f35af0.png)



#### - Using df -h command to see all mounts and free space on your server.


![4b](https://user-images.githubusercontent.com/93729559/165204245-27660200-3439-4284-b733-9c9f250e4548.png)



#### - Using gdisk utility to create a single partition on each of the 3 disks

![5](https://user-images.githubusercontent.com/93729559/165206310-ce841345-6599-4101-9823-4784ea3692ad.png)

![5b](https://user-images.githubusercontent.com/93729559/165206313-93971801-d65c-4477-9a9b-0bafc0e3ca95.png)

![6](https://user-images.githubusercontent.com/93729559/165206314-e1bd8aaf-f953-4e91-b58a-63d71a7e20f0.png)

![7](https://user-images.githubusercontent.com/93729559/165206316-054a640a-7e9a-4afd-bd36-3818d3b07230.png)

<br>


#### - Using lsblk utility to view the newly configured partition on each of the 3 disks.


![8](https://user-images.githubusercontent.com/93729559/165206800-20d2caff-0ad1-46a5-af19-863e96bed655.png)

<br>


#### - Next, Installing lvm2 package using sudo yum install lvm2 and Run sudo lvmdiskscan command to check for available partitions.


![9](https://user-images.githubusercontent.com/93729559/165209927-f90d6ad3-ccf8-45d7-91a2-034d158af5d9.png)

<br>


#### - Using pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM and Verifing that the Physical volume has been created successfully by running sudo pvs


![9a](https://user-images.githubusercontent.com/93729559/165211390-f0e44a22-38e8-4e4d-8d28-75211166609d.png)
<br>


#### - Next, Using vgcreate utility to add all 3 PVs to a volume group (VG) and naming the VG webdata-vg.
Also verifying that the VG has been created successfully by running sudo vgs

![9b](https://user-images.githubusercontent.com/93729559/165212961-8abe8291-fa3e-4ed3-9ffe-72c7696639ba.png)
![9bi](https://user-images.githubusercontent.com/93729559/165212963-eaae9aa4-2704-4573-bbf1-3514137fd9e5.png)

<br>

#### - Using lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

- Also  Verifying that the Logical Volume has been created successfully by running sudo lvs.

![9c](https://user-images.githubusercontent.com/93729559/165214168-ef032846-7416-4aef-8d3b-d02949715543.png)


#### - Verifying the entire setup.

![9d](https://user-images.githubusercontent.com/93729559/165214783-3ef64fd4-6eb5-4dd1-a2e9-5982d38a9bdf.png)


















