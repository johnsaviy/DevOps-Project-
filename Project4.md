
## MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

MEAN Stack is a combination of following components:

1. MongoDB (Document database) – Stores and allows to retrieve data.
2. Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
3. Angular (Front-end application framework) – Handles Client and Server Requests.
4. Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user.


### Task
In this project I'll be implementing a simple Book Register web form using MEAN stack.

<b>- Step 2 - INSTALLING NODEJS<b>
  
  Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. 
  Node.js is used in this project to set up the Express routes and AngularJS controllers.
  
  After connecting to the EC2 Instance in AWS, I"ll install nodejs in to the vm.
  
  ![1](https://user-images.githubusercontent.com/93729559/162446284-80e3acc8-dda6-4fe6-bbe8-d698770f4b8d.png)
  
  
  
  Step 2: Install MongoDB
  
 MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. 
 For our example application, I'll be adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
  
 ![2](https://user-images.githubusercontent.com/93729559/162447808-ea3a6f66-b177-4cc1-9d36-66b91496236a.png)

  
 - Starting The server and verifying that the service is up and running.
  
  ![2a](https://user-images.githubusercontent.com/93729559/162448317-e007b2bb-92e5-475c-99b4-699b81418b12.png)

  
 - Install npm (Node package manager) and body-parser package.
  
![3](https://user-images.githubusercontent.com/93729559/162449424-e0fd4471-ad95-4a9f-8c55-f1df190ac512.png)
  
![3a](https://user-images.githubusercontent.com/93729559/162449433-6f3d3f9f-180d-47e3-8f2d-712fe3cd988a.png)
  

- Next I'll Create a folder named ‘Books’, Initialize npm project in the Books directory and add a file to it named server.js with the following web server code as shown below:
  
![4](https://user-images.githubusercontent.com/93729559/162450986-e317528a-f878-4ec1-9a58-363b39aa9ca0.png)
  
![4a](https://user-images.githubusercontent.com/93729559/162451009-3d3984bb-b41e-497f-b1dc-0f913953e541.png)
  
  
Step 3: Install Express and set up routes to the server.
  
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. I will use Express in to pass book information to and from our MongoDB database.

I also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. 
I will use Mongoose to establish a schema for the database to store data of our book register.
  

![5](https://user-images.githubusercontent.com/93729559/162452522-488de2e6-ded2-4e89-b0af-bae109fd5aa2.png)
  
- In ‘Books’ folder, I'll create a folder named apps, create a file named routes.js and write the following code into the route.js file as shown below:
  
 ![6](https://user-images.githubusercontent.com/93729559/162453665-9aa90e42-f001-4966-b6aa-3355243caa86.png)
  
  
 - In the ‘apps’ folder, I'll create a folder named models, Create a file named book.js with the following code as shown below:
  
 ![6a](https://user-images.githubusercontent.com/93729559/162454285-2be321c7-5b85-4a84-a258-8cc9b0cddc66.png)
  
  
 Step 4 – Access the routes with AngularJS.

  AngularJS provides a web framework for creating dynamic views in your web applications. 
  In this project, I'll use AngularJS to connect our web page with Express and perform actions on our book register.
  
 - I'll navigate back to the ‘Books’ directory, create a folder named public and add a file named script.js with the following code as shown below;
  
 ![7](https://user-images.githubusercontent.com/93729559/162455793-eaaa252b-c32c-4986-b1f4-54e5a922de13.png)

  
- In public folder, I'll create a file named index.html with the following code;
  
 ![7a](https://user-images.githubusercontent.com/93729559/162456980-226286de-c3a1-48b7-9e74-94626b006335.png)

 
 - Now I'll start the server
  
 ![8](https://user-images.githubusercontent.com/93729559/162457654-54efc4da-c89d-4b75-958a-a8db127f6908.png)
  
  
 The server is now up and running, we can connect it via port 3300. 
 I'll need to open TCP port 3300 in the AWS Web Console for the EC2 Instance.
  
 ![8a](https://user-images.githubusercontent.com/93729559/162459819-e124c62f-d912-4ec3-902b-95f3ff8c11e0.png)
  

 - Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.
  
  
  ![10](https://user-images.githubusercontent.com/93729559/162463112-7c8550cd-7f0a-41bb-b541-23754aa1fe06.png)
  
  Now I have the Web Book Register Application deployed!!

  
  

  

  
  
  




  


  
  

  
  
  
 
  
  
  

  
  
  
 
