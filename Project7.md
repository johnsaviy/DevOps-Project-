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



