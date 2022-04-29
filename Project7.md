## DEVOPS TOOLING WEBSITE SOLUTION.

In the previous Project (project 6), I implemented a WordPress based solution that is ready to be filled with content and can be used as a full fledged website or blog.

Moving further I will add some more value to solutions that DevOps team could utilize. I want to utilize a set of DevOps tools that will
help DevOps team in day to day activities in managing, developing, testing, deploying and monitoring different projects.

The tools I'll be using in this project are well known and widely used by multiple DevOps teams. 

So I'll will utilize a single DevOps Tooling Solution that will consist of:

1. Jenkins – free and open source automation server used to build CI/CD pipelines.
2. Kubernetes – an open-source container-orchestration system for automating computer application deployment, scaling, and management.
3. Jfrog Artifactory – Universal Repository Manager supporting all major packaging formats, build tools and CI servers. Artifactory.
4. Rancher – an open source software platform that enables organizations to run and manage Docker and Kubernetes in production.
5. Grafana – a multi-platform open source analytics and interactive visualization web application.
6. Prometheus – An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.
7. Kibana – Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.


#### - Setup and Technologies used in Project 7.

As a member of a DevOps team, I will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure
easily accessible.

- In this project I will implement a solution that consists of following components:

1. Infrastructure: AWS
2. Webserver Linux: Red Hat Enterprise Linux 8
3. Database Server: Ubuntu 20.04 + MySQL
4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server
5. Programming Language: PHP
6. Code Repository: GitHub

On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files 
using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate 
hardware – for Web Servers it look like a local file system from where they can serve the same files. 


![Tooling-Website-Infrastructure](https://user-images.githubusercontent.com/93729559/165704767-005c5856-8531-4f62-a14f-6f0be23882a5.png)

It is important to know what storage solution is suitable for what use cases, for this – you need to answer following questions: what data will be stored, in what 
format, how this data will be accessed, by whom, from where, how frequently, etc. Base on this you will be able to choose the right storage system for your solution.


#### Step 1 – Prepare NFS Server

- Spin up a new EC2 instance with RHEL Linux 8 Operating System.

- Based on the LVM experience from Project 6, I'll Configure LVM on the Server.

- Instead of formating the disks as ext4 I will have to format them as xfs

- Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs

- I'll Create mount points on /mnt directory for the logical volumes as follow:
- Mount lv-apps on /mnt/apps – To be used by webservers
- Mount lv-logs on /mnt/logs – To be used by webserver logs
- Mount lv-opt on /mnt/opt – To be used by Jenkins server in the next project.


![1](https://user-images.githubusercontent.com/93729559/165929343-e171fa70-18af-49e3-83c0-7b9b59898581.png)

![2](https://user-images.githubusercontent.com/93729559/165929344-d2bd6c70-ddd6-407c-a1e0-0840ed62e2ef.png)

![3](https://user-images.githubusercontent.com/93729559/165929347-51f96254-e5c0-4411-9d15-dee82b4cd6e9.png)

![4](https://user-images.githubusercontent.com/93729559/165929350-d1e5982a-71e7-4a8f-8f0e-d2e64ec8d86d.png)

![5](https://user-images.githubusercontent.com/93729559/165929353-0fe1153f-c68d-42fc-83a8-68ae15324379.png)

![6](https://user-images.githubusercontent.com/93729559/165929355-e27b0976-fd7c-454d-b958-20c6b6961f55.png)

![7](https://user-images.githubusercontent.com/93729559/165929358-9f1f7c56-d8f5-4ea2-812e-9d7f8c7881da.png)

![8](https://user-images.githubusercontent.com/93729559/165929360-fe85dcf6-6dfe-4e22-a3f1-2fefac0ec9b1.png)

![9](https://user-images.githubusercontent.com/93729559/165929363-1a485d7a-62d6-435d-a334-930e276d18d7.png)

![10](https://user-images.githubusercontent.com/93729559/165929365-bb68cdbd-512c-4920-ab60-2b8788131e6c.png)



#### - Next I'll Install NFS server, configure it to start on reboot and make sure it is up and running!


- Running sudo yum -y update first.

![11](https://user-images.githubusercontent.com/93729559/165930939-c479a765-530b-4065-b4ec-5bc9eaf4c40f.png)

![13](https://user-images.githubusercontent.com/93729559/165932135-8aaa47cb-a9d5-4448-a3dd-7bbeb9aceff2.png)

![14](https://user-images.githubusercontent.com/93729559/165932140-ed1d79e2-8600-423b-82c3-a14e4da26001.png)



<br>

#### - Export the mounts for webservers’ subnet cidr to connect as clients. 
For simplicity, I will install your all three Web Servers inside the same subnet, but in production set up I would probably want to separate each tier inside its own subnet for higher level of security.
To check the subnet cidr – I will open the EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:

![12](https://user-images.githubusercontent.com/93729559/165931151-824a8941-0634-4628-b612-20d13493f34c.png)



- I'll make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

![15a](https://user-images.githubusercontent.com/93729559/165933538-18e567bb-d035-4499-bd83-6d1aff293fbe.png)

![15b](https://user-images.githubusercontent.com/93729559/165933540-fb803e8a-8627-4465-bf92-8ac3e82d110b.png)



- Restart the nfs-server to ensure the changes are effected!

![16](https://user-images.githubusercontent.com/93729559/165934056-7e7dc3ac-9cf9-4c52-8128-d4a56f169485.png)



- Configure access to NFS for clients within the same subnet.

![17a](https://user-images.githubusercontent.com/93729559/165935400-c525ceb3-9114-4708-a176-01955af2d16f.png)

![17b](https://user-images.githubusercontent.com/93729559/165935402-b4e3ee2b-be01-48ea-8398-d16c0dd5e452.png)

![17c](https://user-images.githubusercontent.com/93729559/165935407-317bdf59-3a93-4bb4-a3fd-358a9129d0a1.png)


#### - Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

![18](https://user-images.githubusercontent.com/93729559/165935695-2bed6005-e7dc-4b8b-b87c-c135171c9eac.png)


- Important note: In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049

In this case I opened all traffic since its for testing purpose and this opens all ports.


![18a](https://user-images.githubusercontent.com/93729559/165936797-f57c01a9-9ebb-4dde-b07a-7c22c2b46172.png)




#### STEP 2 — CONFIGURE THE DATABASE SERVER

- Install MySQL server
- Create a database and name it tooling
- Create a database user and name it webaccess
- Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr


![19](https://user-images.githubusercontent.com/93729559/165941700-cc7c77bf-3332-43e8-a0ed-0bb2820c2623.png)

![19a](https://user-images.githubusercontent.com/93729559/165941707-004a3e64-613e-4b9e-8000-3361345006b5.png)

![19b](https://user-images.githubusercontent.com/93729559/165941711-b153a60f-7962-44c4-8147-3cd8768ec072.png)

![19c](https://user-images.githubusercontent.com/93729559/165941713-61bce957-aa27-4687-892f-4393b706ef44.png)

![19d](https://user-images.githubusercontent.com/93729559/165941716-b68e9b9e-fc54-4706-bf0a-d8ace1bc01f7.png)

![19e](https://user-images.githubusercontent.com/93729559/165941718-4e03885e-eb56-4839-8ce6-8e3094afecbf.png)

![19f](https://user-images.githubusercontent.com/93729559/165941720-9efc17a8-4f09-469b-8e51-8c1b8d95fb80.png)



<br>

#### Step 3 — Prepare the Web Servers

We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case – NFS Server and MySQL database.
You already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that our Web Servers will use – I'll will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores files to be served to the users (/var/www).

This approach will make our Web Servers stateless, which means we will be able to add new ones or remove them whenever we need, and the integrity of the data (in the database and on NFS) will be preserved.

During the next steps I'll will do following:

  - Configure NFS client (this step must be done on all three servers)
  - Deploy a Tooling application to our Web Servers into a shared NFS folder
  - Configure the Web Servers to work with a single MySQL database



- Launch a new EC2 instance with RHEL 8 Operating System in AWS and Install NFS client.


![20](https://user-images.githubusercontent.com/93729559/165942972-e5bac9b0-0015-4053-a00f-c595e380ad9f.png)



- Mount /var/www/ and target the NFS server’s export for apps
- Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot: add following line


![20a](https://user-images.githubusercontent.com/93729559/165945788-7b350263-4969-4adf-a6e5-31bb61a56e53.png)

![20b](https://user-images.githubusercontent.com/93729559/165945792-87c18dad-f51a-4bf6-aaa1-9cd318fdc0d7.png)











































