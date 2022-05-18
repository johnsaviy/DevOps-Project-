
## ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES


In this project we will introduce dynamic assignments by using include module.

Now you may be wondering, what is the difference between static and dynamic assignments?

Well, from Project 12, you can already tell that static assignments use import Ansible module. The module that enables dynamic assignments is include.

Hence,

import = Static
include = Dynamic

When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute site.yml playbook, Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when include module is used, all statements are processed only during execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during execution will be used.

Take note that in most cases it is recommended to use static assignments for playbooks, because it is more reliable. With dynamic ones, it is hard to debug playbook problems due to its dynamic nature. However, you can use dynamic assignments for environment specific variables as we will be introducing in this project.


### Introducing Dynamic Assignment Into Our structure


- In the ansible-config-mgt GitHub repository I'll start a new branch and call it dynamic-assignments.

- Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml. I'll instruct site.yml to include this playbook later. For now, let us keep building up the structure.


![1](https://user-images.githubusercontent.com/93729559/168986922-7987cfad-35a3-4cb5-a5bd-05ce97285607.png)


- Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as servername, ip-address etc., we will need a way to set values to variables per specific environment.

- For this reason, we will now create a folder to keep each environment’s variables file. Therefore, create a new folder env-vars, then for each environment, create new YAML files which we will use to set variables and include the instruction below into the env-vars.yml file.



![2](https://user-images.githubusercontent.com/93729559/168986934-985b8ed5-4842-4235-9212-b078d94eccd7.png)
<br>


- Notice 3 things to note here:

1. We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are: include_role, include_tasks, include_vars.

In the same version, variants of import were also introduces, such as: import_role, import_tasks.

2. We made use of a special variables { playbook_dir } and { inventory_file }. { playbook_dir } will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. { inventory_file } on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.

3. We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.



<br>

- Update site.yml with dynamic assignments.

 Update site.yml file to make use of the dynamic assignment.
 
 
 ![3](https://user-images.githubusercontent.com/93729559/168990113-2efaf789-5832-4429-ae22-ef9b997c82df.png)



 
 <br>
 
- Community Roles.

Now it is time to create a role for MySQL database – it should install the MySQL package, create a database and configure users.

- Download Mysql Ansible Role
 
 








































