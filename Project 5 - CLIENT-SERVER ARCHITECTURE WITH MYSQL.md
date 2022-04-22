
### CLIENT-SERVER ARCHITECTURE WITH MYSQL


#### Step 1

To demonstrate a basic client-server using MySQL Relational Database Management System, first I'll create and configure two Linux-based virtual servers 
(EC2 instances in AWS) named 'mysql server' and 'mysql client'.


![1](https://user-images.githubusercontent.com/93729559/164702833-79398d4c-9c0b-40f7-b59d-6523d7339322.png)


#### Step 2.

On mysql server Linux Server I'll install MySQL Server software.

![2](https://user-images.githubusercontent.com/93729559/164704650-07ea3a78-9e46-4909-8a0e-2d74d7a8526d.png)


#### Step 3.

On mysql client Linux Server I'll install MySQL Client software.

![3](https://user-images.githubusercontent.com/93729559/164705365-d3994a98-1ddf-42ce-8bcb-b252106d95a7.png)

#### Step 4.

By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. 
I'll use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so I'll have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. 
For extra security, I'll not allow all IP addresses to reach the ‘mysql server’ – allow access only to the specific local IP address of the ‘mysql client’.


![4b](https://user-images.githubusercontent.com/93729559/164708626-dc8e7ac6-7fb8-4d8b-81e5-a62d1de65ec4.png)


![4](https://user-images.githubusercontent.com/93729559/164708399-9fbbcab3-fec3-4c10-b406-ccc164f6d918.png)


#### Step 5

Next I'll configure MySQL server to allow connections from remote hosts.
And Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:


![5](https://user-images.githubusercontent.com/93729559/164712513-dbb0e7b8-d03e-4f4c-9be4-bf63cc890f74.png)

![5b](https://user-images.githubusercontent.com/93729559/164712534-dc110283-945b-4f80-9599-1e1cd23d1261.png)


#### Step 6

From mysql client Linux Server I'll connect remotely to mysql server Database Engine using the mysql utility.










