## MIGRATION TO THE СLOUD WITH CONTAINERIZATION. PART 1 – DOCKER &AMP; DOCKER COMPOSE


- Install Docker and prepare for migration to the Cloud

First, we need to install Docker Engine, which is a client-server application that contains:

- A server with a long-running daemon process dockerd.
- APIs that specify interfaces that programs can use to talk to and instruct the Docker daemon.
- A command-line interface (CLI) client docker.


#### Pull MySQL Docker Image from Docker Hub Registry

- Start by pulling the appropriate Docker image for MySQL

![1](https://user-images.githubusercontent.com/93729559/176187939-6c4f83ea-7081-4733-9a91-480b59a15770.png)


#### Step 2: Deploy the MySQL Container to your Docker Engine

- Once you have the image, move on to deploying a new MySQL container

![3](https://user-images.githubusercontent.com/93729559/176188699-5ae08aae-5338-4a19-8545-9ee26249ab8f.png)


#### Prepare database schema

- Clone the Tooling-app repository from  https://github.com/darey-devops/tooling.git 

- On your terminal, export the location of the SQL file

- export tooling_db_schema=/tooling_db_schema.sql and verify that the path is exported

 - Use the SQL script to create the database and prepare the schema. With the docker exec command, you can execute a command in a running container.
 
 $ docker exec -i mysql-server mysql -uroot -p$MYSQL_PW < $tooling_db_schema 

- Update the .env file with connection details to the database

![4](https://user-images.githubusercontent.com/93729559/176190052-2fe15ee1-7d83-4368-8252-2647ae83ae32.png)

![5](https://user-images.githubusercontent.com/93729559/176190055-ea9a9f84-411d-4839-8aca-d85e94fc272f.png)

![6](https://user-images.githubusercontent.com/93729559/176190062-8d8f3598-5b9f-4e78-aa8b-8230d65cf66f.png)



#### Run the Tooling App

- firstly we will run docker build to build our application

- Ensure you are inside the directory "tooling" that has the file Dockerfile and build your container  using the command:

 $ docker build -t tooling:0.0.1 . 
 
In the above command, we specify a parameter -t, so that the image can be tagged tooling"0.0.1 - Also, you have to notice the . at the end. This is important as that tells Docker to locate the Dockerfile in the current directory you are running the command.


![7](https://user-images.githubusercontent.com/93729559/176190776-3171687f-35f5-4604-ac22-723bf55bb3b9.png)


Now from the output above, the docker file has been successfully built!

- Next we will Run the container and open our application


![7b](https://user-images.githubusercontent.com/93729559/176191440-23b7d24f-275f-4387-a4f7-f1b300094b26.png)

![8](https://user-images.githubusercontent.com/93729559/176191451-39338890-28ae-449b-bfe6-d8b1449023de.png)






