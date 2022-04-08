
## MERN STACK IMPLEMENTATION IN AWS CLOUD

In this project, I'll be implementing a web solution based on MERN stack in AWS Cloud.

MERN Web stack consists of following components:

MongoDB: A document-based, No-SQL database used to store application data in a form of documents.

ExpressJS: A server side Web Application framework for Node.js.

ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.

Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

<img width="638" alt="MERN-stack" src="https://user-images.githubusercontent.com/93729559/162151724-7caba173-57a9-4e29-8e3e-390c753913e7.png">

As shown on the illustration above, a user interacts with the ReactJS UI components at the application front-end residing in the browser. 
This frontend is served by the application backend residing in a server, through ExpressJS running on top of NodeJS.
Any interaction that causes a data change request is sent to the NodeJS based Express server, 
which grabs data from the MongoDB database if required, and returns the data to the frontend of the application, which is then presented to the user.


<b>- Task<b> - 
To deploy a simple To-Do application that creates To-Do lists like this:


![mern task](https://user-images.githubusercontent.com/93729559/162174664-e6195329-45b7-4b3c-8f8a-b1ede14bbf7c.gif)
  
- Step 1 – Backend configuration.
  
  -Getting the location of Node.js software from Ubuntu repositories.
  
  ![1](https://user-images.githubusercontent.com/93729559/162179011-a8c6c6ff-bb4e-484c-b3ff-050d48522d8f.png)
  
  
  -Install Node.js on the server.
  
  ![1a](https://user-images.githubusercontent.com/93729559/162179518-e2886315-afc5-4217-8a96-0b36212b0f83.png)
  
  - Application Code Setup.
  
  Create a new directory for your To-Do project:
  
 I'll use the command npm init to initialise the project, so that a new file named package.json will be created. 
  This file will normally contain information about the application and the dependencies that it needs to run.
  
 ![1b](https://user-images.githubusercontent.com/93729559/162186493-84cd7806-c0db-4100-bd7d-2e64db161021.png)
  
 - Confirming that I have package.json file created.
  
  ![1c](https://user-images.githubusercontent.com/93729559/162187089-6ab8f519-61ed-475b-a811-41259f93d911.png)
  
  - STEP 2 - Install ExpressJS
  Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. 
  Therefore it simplifies development, and abstracts a lot of low level details. 
  For example, Express helps to define routes of your application based on HTTP methods and URLs.
  
  ![1d](https://user-images.githubusercontent.com/93729559/162188113-dc1ba469-43fc-473f-9f50-bae1a82d6a43.png)
  
  - Create a file index.js, confirm that your index.js file is successfully created and Install the dotenv module.
  
  ![1e](https://user-images.githubusercontent.com/93729559/162188803-254ab633-d462-4a6a-bfd6-f39a80840368.png)

  Open the index.js file and include the code below into it and save.
  
  ![1f](https://user-images.githubusercontent.com/93729559/162190369-9b552a46-12dd-41d6-97a4-ef7927218f93.png)
  
  Now we start our server to see if it works. Open the terminal in the same directory as your index.js file and type:
  
  ![1g](https://user-images.githubusercontent.com/93729559/162195373-a7d962f2-3226-4b41-8272-02b30e9ccd23.png)
  
  
  - Now we need to open this port in EC2 Security Groups
  
  ![1i](https://user-images.githubusercontent.com/93729559/162198542-72e40b5f-5b07-47b9-9de8-b38c598aa181.png)

  
  Now we open up the browser and try to access your server’s Public IP or Public DNS name followed by port 5000:
  
 ![1h](https://user-images.githubusercontent.com/93729559/162198877-9458201a-9877-4f42-ba02-a4c40c96ceb4.png)
  
  
  - Routes
  
  There are three actions that our To-Do application needs to be able to do:

  1. Create a new task
  2. Display list of all tasks
  3. Delete a completed task
  4. Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes,
  Change directory to routes folder,  create a file api.js, Open the file and include the following code:
  
![2a](https://user-images.githubusercontent.com/93729559/162201297-754458d9-e08f-4da6-8cd6-b206a24a892d.png)
  
![2](https://user-images.githubusercontent.com/93729559/162201310-e6e2adaa-8194-4a14-a4d4-47c24f03fe4d.png)
  
- STEP 3 - MODELS
  
Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.
We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document.
In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. 
These are known as virtual properties.
  
To create a Schema and a model, we need to install mongoose which is a Node.js package that makes working with mongodb easier.
  
![3](https://user-images.githubusercontent.com/93729559/162204356-7481442a-f2f2-426a-9574-31fd3af0ed58.png)
  
Create a new folder models, Change directory into the newly created ‘models’ folder, Inside the models folder, create a file and name it todo.js,
Open the file created with vim todo.js and include the following code
  
![3b](https://user-images.githubusercontent.com/93729559/162211944-d1c7052f-9516-484d-a9c5-823de9638632.png)
![3a](https://user-images.githubusercontent.com/93729559/162211922-bbd6db98-b304-4e3d-86a2-d2f2ff7cd5c2.png)
  
  
- STEP 4 - MongoDB Database
  
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS),

![4](https://user-images.githubusercontent.com/93729559/162219131-68bed7a3-828a-43ea-8c6c-a86e52199e1b.png)

 - In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

 I'll Create a file in the Todo directory and name it .env. ,and add the connection string to access the database in it.
  
![4a](https://user-images.githubusercontent.com/93729559/162224895-5c66b89a-172a-4e7f-8889-5a450db82f97.png)

- Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.
  
 ![4b](https://user-images.githubusercontent.com/93729559/162225628-e0c2ec9a-e1a1-466b-9e03-95a355bda805.png)
  
  Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application,
  instead of writing connection strings directly inside the index.js application file.
 
  Now we start your server
  
  ![4c](https://user-images.githubusercontent.com/93729559/162226756-64680027-7fd2-4d23-b5d7-e3a5f27914f4.png)
  
  Now we have our backend configured!
  
 - Testing Backend Code without Frontend using RESTful API.
  
  So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that.   But during development, we will need a way to test our code using RESTfulL API. 
  Therefore, I'll will need to make use of some API development client to test our code.
  In this project,  I''ll use Postman to test our API.
  
  
![4d](https://user-images.githubusercontent.com/93729559/162423551-9087addf-7f81-4839-a63a-b1fe8aa04c0f.png)
  
![4e](https://user-images.githubusercontent.com/93729559/162423570-3d0f9e7b-12e7-4c25-b8d7-9a39b5802bcd.png)
  
 I have successfully created the Backend, now let's go create the Frontend.
  
  
 -STEP 5 - Frontend creation.
  
Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, I will use the create-react-app command to scaffold our app. 
  This will create a new folder in the Todo directory called client, where I will add all the react code. 
  Before testing the react app, there are some dependencies that need to be installed - Install concurrently, Install nodemon.
  
  - In the Todo folder I'll open the package.json file and edit as shown below:
  
  ![5](https://user-images.githubusercontent.com/93729559/162425629-d6627b85-e03f-423a-9df8-3434c12bdf48.png)
  
  - Next I'll Configure Proxy in package.json in the client folder adding the key value pair in the package.json file "proxy": " http: // localhost: 5000 ".
  The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http: //localhost:5000 rather than always including the entire path like http: //localhost:5000/api/todos.
  
  
  - Now I will run the app
  
   In order to be able to access the application from the Internet I'll have to open TCP port 3000 on EC2 by adding a new Security Group rule.
  
  ![5a](https://user-images.githubusercontent.com/93729559/162425653-eec8a52b-1d29-45cd-8436-d4a45f11a7af.png)
  
  
  ![5b](https://user-images.githubusercontent.com/93729559/162428154-7dfe0666-a754-44b1-828c-e06dd434673d.png)
  
  - Creating your React Components
  
 One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful     components and one stateless component.

  ![5c](https://user-images.githubusercontent.com/93729559/162429233-2a947d1e-5823-4e88-a440-ab8d41b15117.png)
  
  -To make use of Axios, which is a Promise based HTTP client for the browser and node.js, 
  I'll need to cd into the client and run yarn add axios or npm install axios.
  
  ![5d](https://user-images.githubusercontent.com/93729559/162429847-66c9b7e0-e18f-40b0-81cb-5e1d460fca4e.png)
  
  - Then I'll have to navigate back to  ‘components’ directory, open the ListTodo.js and insert the code as shown below:
  
  ![6](https://user-images.githubusercontent.com/93729559/162430437-2e9b0b2a-0422-47bd-95d3-1fc9ad50f5dd.png)
  
  
  - Then in the Todo.js file I'll write the following code:
  
  ![6a](https://user-images.githubusercontent.com/93729559/162430756-3e6e6749-3789-4f69-b964-f6bb937c011f.png)
  
  
  -We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.
  
  ![6b](https://user-images.githubusercontent.com/93729559/162431260-e6f976b4-41a8-48f8-b34a-2a23b6e87edd.png)
  

 - In the src directory I'll open the App.css and write the code as shown below:
  
  ![6c](https://user-images.githubusercontent.com/93729559/162431931-00b8d8e4-6008-4ee5-9aa5-fb8931fada56.png)

  
  
  
  


  
  
  
  

  
  
  
 


  
 


  

  
  
  

  

 


