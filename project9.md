
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


  - Enable webhooks in your GitHub repository settings.
  
  In configuration of the Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository
  and credentials (user/password) so Jenkins could access files in the repository.
  
  ![8](https://user-images.githubusercontent.com/93729559/166101568-71460499-ee44-44d6-a3f4-7166217fa9e8.png)
  
  
  ![9](https://user-images.githubusercontent.com/93729559/166101571-4821840b-034d-4f1b-b218-79dbdd14bb80.png)
  
  
  
 - Save the configuration and let us try to run the build. For now we can only do it manually.
<br>
  
  
  - Click "Build Now" button, if you have configured everything correctly, the build will be successfull and you will see it under #1
  
  
  ![10](https://user-images.githubusercontent.com/93729559/166102033-446b0f6c-4824-44ef-9a9b-0348da40b519.png)
  
    <br>
  
You can open the build and check in "Console Output" if it has run successfully.
  
  
  ![11](https://user-images.githubusercontent.com/93729559/166102141-29d79101-2cf8-4085-8d62-ce811851df41.png)
  
  
  If so –  I have just made a Jenkins build!

But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.

  
  
 - Click "Configure" your job/project and add these two configurations.
  
Configure triggering the job from GitHub webhook:
  
 ![12](https://user-images.githubusercontent.com/93729559/166102487-e8aed29f-4669-4348-81df-a76502a5d870.png)
  
  
 
  - Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts"
  
![13](https://user-images.githubusercontent.com/93729559/166102488-aa5c43db-0511-4777-bfaa-269d09d1a65a.png)
  
  

 -  Now, I'll go ahead and make some change in any file in the GitHub repository (e.g. README.MD file) and push the changes to the master branch.

You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.
  
  
 ![14](https://user-images.githubusercontent.com/93729559/166102694-c43b1e72-a02a-4b5c-9774-959546f5ccf6.png)
  
 I have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstreadm) from another (upstream), poll GitHub periodically and others.

By default, the artifacts are stored on Jenkins server locally.: ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
  
  
  
 <br>

#### Step 3 – Configure Jenkins to copy files to NFS server via SSH
  
  Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.

Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called "Publish Over SSH".

 - Install "Publish Over SSH" plugin.

 - On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item.

 - On "Available" tab search for "Publish Over SSH" plugin and install it
 
  ![15](https://user-images.githubusercontent.com/93729559/166103072-a75e4795-9e27-45f0-90aa-89f3e0c832e8.png)

  
  <br>
  
 -  Configure the job/project to copy artifacts over to NFS server.

 - On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

  Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

  Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
  Arbitrary name
  Hostname – can be private IP address of your NFS server
  Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
  Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server
  
 - Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.
  
  
  ![16](https://user-images.githubusercontent.com/93729559/166105043-0831c4d9-ffb4-49d3-af35-2520411b9371.png)

  
  <br>
  
  - Save the configuration, open your Jenkins job/project configuration page and add another one "Post-build Action"
  
  ![17](https://user-images.githubusercontent.com/93729559/166105547-70bf2944-aeef-43ea-a7a5-7bb775336386.png)

  
  -Save this configuration and go ahead, change something in README.MD file in your GitHub Tooling repository.

Webhook will trigger a new job and in the "Console Output" of the job you will find something like this:

SSH: Transferred 25 file(s)
Finished: SUCCESS
  
 ![18](https://user-images.githubusercontent.com/93729559/166106262-9da4de94-c475-469e-b783-72288731d965.png)
  
![19](https://user-images.githubusercontent.com/93729559/166106264-0c7f7b5e-caf3-4cf4-989e-bf6e21147880.png)
  
  
  I have seen the changes I had previously made in the GitHub – the job works as expected.
  
 I  have just implemented your a Continous Integration solution using Jenkins CI
  
  
  

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  






