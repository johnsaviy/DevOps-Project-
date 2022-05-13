
## ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

Code Refactoring
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

In this case, I'll move things around a little bit in the code, but the overal state of the infrastructure remains the same.



### Step 1 – Jenkins job enhancement.

Before we begin, let's make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient 
when we want to run some commands from one place.

Besides, it consumes space on Jenkins server with each subsequent change. Let us enhance it by
introducing a new Jenkins project/job – we will require Copy Artifact plugin.
