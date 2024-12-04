# Linked Lists
#cs

A linked list is made up of nodes containing both data and a pointer pointing to the next node.

In an array, items can be accessed using an index, but in linked lists, you must follow the pointers of each node to access a wanted element.

>[!NOTE]
>To traverse a linked list, you must start from the header node (first node) and follow each consecutive node's pointer to the next node.`


Below is an implementation of a linked list:

```python
class LinkedList:
	class Node: ### Defines the Node object
		def __init__(self):
			self.data = None
			self.next = None
			
	def __init__(self):
		self.head = None 
		self.n = 0
		
	def __len__(self):
		return self.n
		
	def add_first(self,val): ### Creates a new header node that points to the old header
		new_node = Node()
		new_node.data = val
		new_node.next = self.head
		self.head = new_node 
		self.n += 1
```

In order to insert in the middle or the end, you would have to iterate through the linked list until you reach your desired location. 

You would then have to point the new node to the previous nodes pointer, and set the previous nodes pointer to point to the new node:

```python
def add_after(self, node, val):
	prev_node = node
	next_node = node.next
	new_node = Node()
	new_node.data = val
	prev_node.next = new_node
	new_node.next = next_node
	self.n +=1
	return new_node
```

A few things to note:

`add_first()` runs in a **constant** worst case time complexity. This is an **advantage** of using a linked list.

It would also be useful to store the **tail** node as a data member to allow for a constant worst case time for end insertion. 



## Doubly Linked Lists (DLLs)

A DLL is a linked list which can be traversed in both forwards and backwards directions rather than just forwards.

Each node contains pointers which lead to the previous node and next node.

This allows us to remove data from both ends at a constant worst case. 

The following DLL implementation will use empty 'Sentinel' Nodes (null pointers) at the ends of the linked list, allowing for a simpler implementation and less branched edge cases. They are not included in the logical list or data.


```python
Class DoublyLinkedList:
	class Node:
		def __init__(self, data = None):
			self.data = data
			self.next = None
			self.prev = None
		
		def disconnect(self):
			self.data = None
			self.next = None
			self.prev = None
			
	def __init__(self):
		self.header  = DoublyLinkedList.Node()
		self.trailer = DoublyLinkedList.Node()
		self.header.next = self.trailer
		self.trailer.prev = self.header
		self.n = 0
		
	def __len__(self):
		return self.n
```


```python
	def add_first(self,val):
		return self.add_after(self.header, val)
		
	def add_last(self,val):
		return self.add_after(self.trailer.prev, val)
		
	def delete_first(self):
		if (self.is_empty()):
			raise Exception("List is Empty")
		return self.delete_node(self.header.next)
		
	def delete_last(self):
		if (self.is_empty()):
			raise Exception("List is Empty")
		return self.delete_node(self.trailer.prev)
		
	def add_after(self, node, val):
		prev_node = node
		next_node = node.next
		new_node = DoublyLinkedList.Node(val)
		prev_node.next = new_node
		new_node.prev = prev_node
		next_node.prev = new_node
		new_node.next = next_node
		return new_node
		
	def add_before(self, node, val):
		return self.add_after(node.prev, val)
		
	def delete_node(self, node):
		prev_node = node.prev
		next_node = node.next
		prev_node.next = next_node
		next_node.prev = prev_node
		self.n -= 1
		res = node.data
		node.disconnect()
		return res
```



