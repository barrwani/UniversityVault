# Rooted Binary Trees
#cs 


## Definition

A binary tree is an ADT made of nodes, where each node contains a "left" pointer, a "right" pointer, and a data element. 

The "root" pointer points to the topmost node in the tree. 

The left and right pointers **recursively** point to smaller "sub-trees" on either side. 

![Binary Tree](https://i2.wp.com/techsprobe.com/wp-content/uploads/2020/03/Untitled-1-2.jpg?fit=1011%2C356&ssl=1)

A null or `None` pointer is a pointer which does not lead to a new node.

A leaf node is a node with two null pointers.

## In-Order Traversal
1. Left
2. Root
3. Right

![|200](https://www.techiedelight.com/wp-content/uploads/Inorder-Traversal.png)

## Post-Order Traversal
1. Left
2. Right
3. Root

![|200](https://www.techiedelight.com/wp-content/uploads/Postorder-Traversal.png)

## Pre-Order Traversal
1. Root
2. Left
3. Right

![|200](https://www.techiedelight.com/wp-content/uploads/Preorder-Traversal.png)


## Parent-Child Relationship

A parent-child node relationship is simple. Any node which contains at least one child node is considered a parent, and all nodes other than the root node are considered children of some other node.

## Subtrees

Subtrees are segments of a tree which are rooted by child nodes of existing trees. 


## Full vs Complete Binary Trees

A full binary tree is a tree where the number of children of each node is either 2 or 0. 

![[University/PastedImages/Pasted image 20221213013009.png]]
A complete binary tree is a tree where each level contains the maximum possible amount of nodes (each parent node has 2 children, all leaves are on the same level)

![[University/PastedImages/Pasted image 20221213013002.png]]


## Implementation

Node Class:
```python
class Node:
	def __init__(self, data, left=None, right=None):
		self.data = data
		self.left = left
		if (left is not None):
			self.left.parent = self
		self.right = right
		if (right is not None):
			self.right.parent = self
		self.parent = None
```

Initialisation, Length, and Is_Empty:
```python
class LinkedBinaryTree:
	def __init__(self, root=None):
		self.root = root
		self.size = self.count_nodes()
	
	def __len__(self):
		return self.size
	
	def is_empty(self):
		return (len(self) == 0)
```

Count Nodes:
```python
def count_nodes(self):
	def subtree_count(root):
		if(root is None):
			return 0
		else:
			left_count = subtree_count(root.left)
			right_count = subtree_count(root.right)
			return left_count + right_count + 1
	return subtree_count(self.root)
```

Recursive sum of all values in tree:
```python
def sum(self):
	def subtree_sum(root):
		if(root is None):
			return 0
		else:
			left_sum = subtree_sum(root.left)
			right_sum = subtree_sum(root.right)
			return left_sum + right_sum + root.data
	return subtree_sum(self.root)
```


Recursive Calculation of Height:
```python
def height(self):
	def subtree_height(root):
		if((root.left is None) and (root.right is None)):
			return 0
		elif(root.right is None): #left is not None
			left_height = subtree_height(root.left)
			return 1 + left_height
		elif(root.left is None): #right is not None
			right_height = subtree_height(root.right)
			return 1 + right_height
		else: #both subtrees are not None
			left_height = subtree_height(root.left)
			right_height = subtree_height(root.right)
			return 1 + max(left_height, right_height)

	if(self.is_empty()):
		raise Exception("Tree is empty")
	return subtree_height(self.root)

```


Pre Order. Post Order, In Order, and Breadth First Traversal:
```python
def preorder(self):
	def subtree_preorder(root):
		if(root is None):
			pass
		else:
			yield root
			yield from subtree_preorder(root.left)
			yield from subtree_preorder(root.right)
	yield from subtree_preorder(self.root)

def inorder(self):
	def subtree_inorder(root):
		if(root is None):
			pass
		else:
			yield from subtree_inorder(root.left)
			yield root
			yield from subtree_inorder(root.right)
	yield from subtree_inorder(self.root)

def postorder(self):
	def subtree_postorder(root):
		if (root is None):
			pass
		else:
			yield from subtree_postorder(root.left)
			yield from subtree_postorder(root.right)
			yield root
	yield from subtree_postorder(self.root)

def breadth_first(self):
	if(self.is_empty()):
		return
	bfs_queue = ArrayQueue()
	bfs_queue.enqueue(self.root)
	while(bfs_queue.is_empty() == False):
		curr_node = bfs_queue.dequeue()
		yield curr_node
		if(curr_node.left is not None):
			bfs_queue.enqueue(curr_node.left)
		if(curr_node.right is not None):
			bfs_queue.enqueue(curr_node.right)
```


Iterate through Binary Tree in Breadth First order
```python
def __iter__(self):
	for node in self.breadth_first():
		yield node.data
```