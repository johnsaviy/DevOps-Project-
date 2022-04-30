
#### Continous Integration Pipeline For Tooling Website

- TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS.

In previous Project 8 we introduced horizontal scalability concept, which allow us to add new Web Servers to our Tooling Website and I have successfully deployed a set up with 2 Web Servers and also a Load Balancer to distribute traffic between them. If it is just two or three servers – it is not a big deal to configure them manually. Imagine that you would need to repeat the same task over and over again adding dozens or even hundreds of servers.

DevOps is about Agility, and speedy release of software and web solutions. One of the ways to guarantee fast and repeatable deployments is Automation of routine tasks.

In this project we are going to start automating part of our routine tasks with a free and open source automation server – Jenkins. It is one of the most popular CI/CD tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project originally had a named "Hudson".

Acording to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.

In this project I'm going to utilize Jenkins CI capabilities to make sure that every change made to the source code in GitHub https://github.com/johnsaviy/tooling will be automatically be updated to the Tooling Website.


#### Task

Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

Here is how the updated architecture will look like upon competion of this project:

![add_jenkins](https://user-images.githubusercontent.com/93729559/166098176-ff23f42f-f291-4bb4-abf9-5d00febea3ef.png)



<br>

#### Step 1 – Install Jenkins server

- Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"

![1](https://user-images.githubusercontent.com/93729559/166101546-dbbb38cb-4e5c-4540-b852-37e6139147a2.png)


- Install JDK (since Jenkins is a Java-based application)


![2](https://user-images.githubusercontent.com/93729559/166101550-97c74f48-f49e-4b91-ab6d-8e0271d7ed2a.png)


![2a](https://user-images.githubusercontent.com/93729559/166101553-e5d72296-a858-4b59-a0e9-e7ee2df00095.png)


<br>

- Install Jenkins


![3](https://user-images.githubusercontent.com/93729559/166101554-281f2a5b-5c36-44fb-ae02-4055936f7f4b.png)

![3a](https://user-images.githubusercontent.com/93729559/166101556-c2fa8069-8839-46f1-b2c1-3f60a1110c60.png)


- Making sure Jenkins is up and running

![3b](https://user-images.githubusercontent.com/93729559/166101557-d046f4ec-6c98-4d29-bac6-5b4523869e68.png)


<br>


- By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in the EC2 Security Group

![4](https://user-images.githubusercontent.com/93729559/166101558-85156ebc-fac2-47fd-b988-9af0f9ad12d5.png)

<br>

- Perform initial Jenkins setup.

- From the browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

- You will be prompted to provide a default admin password
  
  ![5](https://user-images.githubusercontent.com/93729559/166101560-cf0197de-b3c8-46a2-81fd-744cfea7dda9.png)
  
  
  <br>


Retrieve it from your server: sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  
  
Then you will be asked which plugings to install – choose suggested plugins.
  
  ![6](https://user-images.githubusercontent.com/93729559/166101561-84a42278-ee14-49e8-8a07-3923891da0db.png)
  
  <br>
  
  Once plugins installation is done – create an admin user and you will get your Jenkins server address.

  The installation is completed!


  ![7](https://user-images.githubusercontent.com/93729559/166101564-dc97ff65-244a-4470-a540-96cffd0a1b8e.png)
  
  
  
  <br>
  
 #### Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks

  In this part, I will configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will be triggered by GitHub webhooks and will 
  execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.


  - Enable webhooks in your GitHub repository settings
  
  ![8](https://user-images.githubusercontent.com/93729559/166101568-71460499-ee44-44d6-a3f4-7166217fa9e8.png)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  






