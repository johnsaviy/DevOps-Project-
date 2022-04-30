
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



