
## ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

Code Refactoring
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

In this case, I'll move things around a little bit in the code, but the overal state of the infrastructure remains the same.



### Step 1 – Jenkins job enhancement.

Before we begin, let's make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient 
when we want to run some commands from one place.

Besides, it consumes space on Jenkins server with each subsequent change. Let us enhance it by
introducing a new Jenkins project/job – we will require Copy Artifact plugin.


- Go to Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

- Change permissions to this directory, so Jenkins could save files there –

![1](https://user-images.githubusercontent.com/93729559/168260745-469d2b05-e820-4064-a01a-a3d9d5e6c23b.png)
<br>

- Install  Copy Artifact plugin in the Jenkins web console, configure it and test the set up by making some change in README.MD file inside the ansible-config-mgt repository (right inside main branch).


![2](https://user-images.githubusercontent.com/93729559/168284866-dc5761e9-e2e2-4e29-bac7-1cc71cdd391a.png)

![3](https://user-images.githubusercontent.com/93729559/168284871-2130c389-21d1-4b4f-a12d-d1116ab640ff.png)

![4](https://user-images.githubusercontent.com/93729559/168284873-f60a25a1-32dc-420a-a59d-789ed3c729b3.png)

![5](https://user-images.githubusercontent.com/93729559/168284875-63226d6e-2ad7-4854-bdd8-3a625e414a22.png)

![6](https://user-images.githubusercontent.com/93729559/168284880-3a75a5a7-bbc6-4a71-ad08-58d1f259e535.png)


<br>

### Step 2 – Refactor Ansible code by importing other playbooks into site.yml

Before starting to refactor the codes, ensure that you have pulled down the latest code from main branch, and created a new branch, name it refactor.

DevOps philosophy implies constant iterative improvement for better efficiency – refactoring is one of the techniques that can be used, but you always have an answer to question "why?". Why do we need to change something if it works well?

In Project 11 I wrote all tasks in a single playbook common.yml, now it is pretty simple set of instructions for only 2 types of OS, but imagine you have many more tasks and you need to apply this playbook to other servers with different requirements. In this case, you will have to read through the whole playbook to check if all tasks written there are applicable and is there anything that you need to add for certain server/OS families. Very fast it will become a tedious exercise and your playbook will become messy with many commented parts. Your DevOps colleagues will not appreciate such organization of your codes and it will be difficult for them to use your playbook.

Most Ansible users learn the one-file approach first. However, breaking tasks up into different files is an excellent way to organize complex sets of tasks and reuse them.

Let see code re-use in action by importing other playbooks.

- Within playbooks folder, create a new file and name it site.yml – 
This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that I created previously. 


- Create a new folder in root of the repository and name it static-assignments. 
The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. 

- Move common.yml file into the newly created static-assignments folder.

- Inside site.yml file, import common.yml playbook.

![7](https://user-images.githubusercontent.com/93729559/168287877-7072f00f-726f-45c4-80f1-d4129eaee09f.png)

<br>



- Run ansible-playbook command against the dev environment.

Since I need to apply some tasks to the dev servers and wireshark is already installed – I can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, I'll configure deletion of wireshark utility.


![8](https://user-images.githubusercontent.com/93729559/168289699-311e8ef8-8978-4442-aa53-76ce441e7a08.png)



- update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers:


![9](https://user-images.githubusercontent.com/93729559/168289694-efe637f1-8c7a-4dc3-a9bd-7e70f9ad8b14.png)



![10](https://user-images.githubusercontent.com/93729559/168314429-6d890726-8f9a-4864-b6f4-654abd3ff717.png)



### Step 3 – Configure UAT Webservers with a role ‘Webserver’


- Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly – Web1-UAT and Web2-UAT.



- To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.


![11](https://user-images.githubusercontent.com/93729559/168470848-38c24a31-7fda-4c2f-8cc2-4304278be963.png)


<br>

- Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of the 2 UAT Web servers.


![12](https://user-images.githubusercontent.com/93729559/168471477-3f5f22f1-c79e-49e7-8450-2e752f72a267.png)




<br>

- In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-artifact/roles, so Ansible could know where to find configured roles.


![13](https://user-images.githubusercontent.com/93729559/168472037-d1e247c5-1958-4e64-a7ec-339884cc1918.png)

<br>
<br>

- Now lets add some logic to the webserver role. 

- Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

- Install and configure Apache (httpd service)
 
- Clone Tooling website from GitHub https ://github.com/johnsaviy/tooling.git.

- Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.

- Make sure httpd service is started.
  
  
![14](https://user-images.githubusercontent.com/93729559/168472626-aef263cc-f950-44ba-b5f0-92c7fccce96d.png)

































