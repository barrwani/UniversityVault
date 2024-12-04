# Recursion
#cs 


Recursion is a problem solving technique closely related to mathematical induction.

We define the solution as a combination of solutions to smalelr instances of the same problem. 

## Step 1: **The Base Case**

- Solve the problem for the **smallest possible input**:
	- Identify how to measure the size of the input
	  
	- Find the condition testing for the smallest possible input
	  
	- Formulate the solution of the problem for the smallest possible input

## Step 2: **The Recursive Step**

- Define the recursion hypothesis - Assume that "when **calling the function** with **a smaller input**, it would **do it's job**"
- Based on this assumption, find how to combine the calls to smaller instances in order to solve the problem for the given input.


## Recursive [[Rooted Binary Trees|Trees]]

### Structure

Each **recursive call** is represented as a **node** in the tree. Inside each node, we write the **size** of the input it was called with. 

If 'A' directly calls 'B', we draw an edge from the node representing 'A' to the node representing 'B'

### Cost

At the **side** of each node, we **write the local cost** of that recursive call (**excluding the calls** the call makes)

The **sum** of the **local costs** will be equal to the **total cost** of the recursive process.