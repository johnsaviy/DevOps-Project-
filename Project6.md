## WEB SOLUTION WITH WORDPRESS.

In this project I will prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. 
WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

This Project consists of two parts:

1. Configure storage subsystem for Web and Database servers based on Linux OS. 
  Here, I'll be working with disks, partitions and volumes in Linux (REDHAT LINUX DISTRIBUTION).

2. Install WordPress and connect it to a remote MySQL database server.


#### Step 1 — Prepare the Web Server

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

- Also verifying that the Logical Volume has been created successfully by running sudo lvs.

![9c](https://user-images.githubusercontent.com/93729559/165214168-ef032846-7416-4aef-8d3b-d02949715543.png)


#### - Verifying the entire setup.

![9d](https://user-images.githubusercontent.com/93729559/165214783-3ef64fd4-6eb5-4dd1-a2e9-5982d38a9bdf.png)
<br>


#### - Next, Using mkfs.ext4 to format the logical volumes with ext4 filesystem.


![9e](https://user-images.githubusercontent.com/93729559/165253846-e67e309c-d2b9-4885-bc86-419482995388.png)


#### - Creating /var/www/html directory to store website files, /home/recovery/logs to store backup of log data, and Mount /var/www/html on apps-lv logical volume.

![9e1](https://user-images.githubusercontent.com/93729559/165256158-9bcdf217-46ba-4a73-acc5-f34763684131.png)
![9e2](https://user-images.githubusercontent.com/93729559/165256164-1376104b-d58b-4136-840f-8301ca404d8f.png)

<br>

#### - Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system).

![9f](https://user-images.githubusercontent.com/93729559/165258552-bb8011fe-c17e-494c-974c-4266c4bd1d12.png)

<br>

#### - Mount /var/log on logs-lv logical volume. 
(Note that all the existing data on /var/log will be deleted. That is why backing up all the files in the log directory /var/log into /home/recovery/logs above is very important)


![9g](https://user-images.githubusercontent.com/93729559/165259684-3e7f8b0e-d08c-4924-a882-f62aeeb16099.png)

<br>

#### - Restore log files back into /var/log directory

![9h](https://user-images.githubusercontent.com/93729559/165260759-f55a6243-5231-4f24-ab01-b494c7dab88d.png)


<br>

####  — Update /etc/fstab file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the /etc/fstab file;

![9i](https://user-images.githubusercontent.com/93729559/165264451-68eaa9cf-4178-465c-bf85-1b2647fbc5ca.png)


<br>

#### - Test the configuration and reload the daemon.

![9j](https://user-images.githubusercontent.com/93729559/165420439-4dcc1242-7bcb-4b58-a201-60cf175de0ea.png)


- Verifying the setup by running df -h

![9k](https://user-images.githubusercontent.com/93729559/165420846-70742149-d229-40b5-a94b-822f11960067.png)



<br>

####  Step 2 — Prepare the Database Server

Launch a second RedHat EC2 instance in AWS that will have a role – ‘DB Server’
Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv (Logical volume) and mount it to /db directory instead of /var/www/html/.


#### - Using lvcreate utility to create logical volume db-lv and verifying that the Logical Volume has been created successfully by running sudo lvs.

![db1](https://user-images.githubusercontent.com/93729559/165427733-c260d60a-9894-4b94-842a-df35c32bd4d7.png)

<br>

#### - Next, creating the /db directory and using mkfs.ext4 to format the logical volumes with ext4 filesystem.

![db2](https://user-images.githubusercontent.com/93729559/165429261-d4524593-6055-4641-b3fe-2f5e655b62d2.png)


#### - Mount /dev/vg-database/db-lv  to /db.


![db3](https://user-images.githubusercontent.com/93729559/165429789-a619f461-bc04-4613-88bf-b2663216cf2a.png)


<br>
####  — Update /etc/fstab file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the /etc/fstab file;

![db4](https://user-images.githubusercontent.com/93729559/165431588-8ecc25c4-5441-434f-b7f5-4eccaa85e7bc.png)

![db4a](https://user-images.githubusercontent.com/93729559/165431590-7f512807-2fa7-4d22-a811-40097812cd74.png)


#### - Test the configuration,  reload the daemon and verifying the setup by running df -h

![db5](https://user-images.githubusercontent.com/93729559/165432448-447f8620-abfa-4b27-b7de-e68248e4be47.png)

![db5a](https://user-images.githubusercontent.com/93729559/165432452-8a6b4706-dadf-4a52-9cc8-bb343db4bf42.png)


<br>

#### Step 3 — Install WordPress on your Web Server EC2.


- Updating the repository, Install wget, Apache and it’s dependencies, Start Apache

![10](https://user-images.githubusercontent.com/93729559/165484634-7a4dd4ff-de19-4867-9658-9b22ebf65fe0.png)



- Installing wget httpd php php-mysqlnd php-fpm php-json


![10a](https://user-images.githubusercontent.com/93729559/165492003-b9116ca8-8a8e-40a6-95f5-174d4a0c70dc.png)



#### - We are going to install the latest version of PHP using the Remi repository.

- First, install the EPEL repository using the following command: sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm


![10b](https://user-images.githubusercontent.com/93729559/165491449-b8bab95e-ef09-46fc-b84f-f8f7a64329d4.png)


- Next, install yum utils and enable remi-repository using the command: sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm 


![10c](https://user-images.githubusercontent.com/93729559/165493147-7bbb7644-5263-4441-b265-7f8c881eae64.png)


- After the successful installation of yum-utils and Remi-packages, search for the PHP modules which are available for download by running the command: sudo dnf module list php

- The output will include the available PHP modules, stream and installation profiles as shown below.


![10d](https://user-images.githubusercontent.com/93729559/165494516-064a71fe-0724-44c3-82ba-e023b8536226.png)


- The output indicates that the currently installed version of PHP is PHP 7.2. To install the newer release, reset the PHP modules, using the command: sudo dnf module reset php


![10e](https://user-images.githubusercontent.com/93729559/165495577-fdb2b3d8-2f40-4787-bcf6-e1e3f28061b5.png)


- Having reset the PHP modules, enable the PHP 7.4 module by running: sudo dnf module enable php:remi-7.4

![10f](https://user-images.githubusercontent.com/93729559/165496708-ac0ade2f-1430-4977-8d46-b071e988fae2.png)


- Finally, install PHP, PHP-FPM (FastCGI Process Manager) and associated PHP modules using the command: sudo dnf install php php-opcache php-gd php-curl php-mysqlnd


![10g](https://user-images.githubusercontent.com/93729559/165497751-7f6d7bbb-3e57-434c-844b-edd6e17cec39.png)


- To verify the version installed to run: php -v 

![10h](https://user-images.githubusercontent.com/93729559/165498111-35eebb92-330d-4ded-ba57-b45477e711d5.png)


- Perfect! We now have PHP 7.4 installed. Equally important, we need to start and enable PHP-FPM on boot-up, and also check its status.

![10i](https://user-images.githubusercontent.com/93729559/165499131-b3ad6570-b09e-45c5-bd98-2dea0d7e8fba.png)


- Next, we need to instruct SELinux to allow Apache to execute the PHP code via PHP-FPM by running: setsebool -P httpd_execmem 1

![10j](https://user-images.githubusercontent.com/93729559/165500222-6cfa6226-c864-400d-961e-716f14df07c6.png)


- Start Apache and check Apache status

![10k](https://user-images.githubusercontent.com/93729559/165501868-a56cc27b-f1ea-4853-bdc7-5a9363d22c6d.png)


<br>

#### - Download wordpress and copy wordpress to var/www/html


![11](https://user-images.githubusercontent.com/93729559/165505155-ba6bd96e-babc-4eca-a4b5-a8b151495de0.png)

![11a](https://user-images.githubusercontent.com/93729559/165506637-fa66ff6d-0072-4e7d-b2d6-61af2b05961a.png)

![11b](https://user-images.githubusercontent.com/93729559/165509524-9519dcdd-f9a2-4f6f-8fc5-98f8bb25aa9e.png)



- I'll also need to install Mysql on the web server and verify that the service is up and running

![11c](https://user-images.githubusercontent.com/93729559/165511122-747d529d-7fa7-479a-9b38-191876b97fff.png)

![11d](https://user-images.githubusercontent.com/93729559/165512209-9ade750f-620e-4aac-a937-1a83aea733c3.png)




#### Step 4 — Install MySQL on the DB Server EC2.

-Install MySQL on the DB Server EC2, start and enable it.

![12](https://user-images.githubusercontent.com/93729559/165513469-d345c939-e3ec-4572-bcff-af411d042723.png)

![12a](https://user-images.githubusercontent.com/93729559/165513476-1b117e12-8c70-47b0-921f-ea45c385a57c.png)



#### Step 5 — Configure DB to work with WordPress

![13](https://user-images.githubusercontent.com/93729559/165518064-ffeca18e-fcdf-4bee-90ee-691210564210.png)

![13a](https://user-images.githubusercontent.com/93729559/165518070-db55b67c-cbe0-4ae9-9adf-d30efb4cae01.png)


- Edit the configuration file and restart the service.

![13b](https://user-images.githubusercontent.com/93729559/165519417-44be9363-1293-4f9c-b44f-cb039e49ca9b.png)

![13c](https://user-images.githubusercontent.com/93729559/165519728-f10fae3e-77a1-4d00-8c0b-fc75a42719eb.png)




#### Step 6 — Configure WordPress to connect to remote database.

- Configure the wp-config.php 

![1a](https://user-images.githubusercontent.com/93729559/165522788-f42f4d37-9021-49fe-b400-e5b816c540a2.png)

![1](https://user-images.githubusercontent.com/93729559/165522789-873faa9c-90fe-4b50-858b-2aeeac1f4420.png)



- Disabling Apache Welcome Page using the command: mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup. Then restart Apache.

![2](https://user-images.githubusercontent.com/93729559/165524057-2c25326c-22e4-4b88-a623-77fb9933fc20.png)


- Verifying if I can successfully execute SHOW DATABASES; command and see a list of existing databases.

![3](https://user-images.githubusercontent.com/93729559/165525203-451bbe30-a33e-409e-b910-e5101daba585.png)


- Configure SELinux Policies.

![6](https://user-images.githubusercontent.com/93729559/165531901-17319797-d18d-46cb-830f-66ab0022b209.png)





#### - Now lets access from the browser the link to the WordPress.
  

![4](https://user-images.githubusercontent.com/93729559/165531439-a93fbc07-2756-4183-b506-b38de37e898a.png)

![5](https://user-images.githubusercontent.com/93729559/165531454-fa9e0b3a-d64f-4137-bda8-8e39c6a5efe3.png)


####  Perfect! I have successfully configured Linux storage susbystem and have also deployed a full-scale Web Solution using WordPress CMS and MySQL RDBMS!


















































