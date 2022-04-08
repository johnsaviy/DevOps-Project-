
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


  
  

  
  
  
 
  
  
  

  
  
  
 
