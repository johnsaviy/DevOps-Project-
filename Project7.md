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











