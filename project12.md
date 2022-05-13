
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


