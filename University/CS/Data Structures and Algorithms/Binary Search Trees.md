# Binary Search Trees
#cs 

## Definition

A binary search tree is a rooted binary tree with special properties.

For every node X in the tree:
- Every key in the left subtree is less than X's key
- Every key in the right subtree is greater than X's key

BSTs also output a sorted order by doing **in order** traversal. 


## Searching

To search for a node: 
- Traverse left if the node is smaller than the current node
- Traverse right if the node is greater than the current node

The node doesn't exist if we need to traverse to a non existent child 

Search runtime is $O(H)$ were $H$ is the height of the tree.


## Insertion

Same concept as searching, but with an insertion:

- Traverse left if the new node is smaller than the current node
- Traverse right if the node is greater than the current node

The added node is always a leaf node.

Insertion runtime is $O(H)$ were $H$ is the height of the tree.


## Deletion

Deletion has 3 cases:

### **No Children**
- It is a leaf node
- Remove the parent's pointer to it
- $O(H)$

### **1 Child**
- Replace the node with its child
	- Set it's parents child to its child instead
-  $O(H)$

### **2 Children**
- Replace node with successor or predecessor
	- Find Successor or Predecessor (key directly above or below it in sorted order)
	- Replace node with its successor or predecessor
-  $O(H)$


## BST Map

- Stores a pair of values (key,value) at each node

- BST is sorted by the value of the key

- Runtime is proportional to tree height

- Balanced Binary Tree has a height of $O(\log n)$

- If the tree is balanced we can set, get and delete items in $O(\log n)$ time
	- A list would perform these operations in linear time.
