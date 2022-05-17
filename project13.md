
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















