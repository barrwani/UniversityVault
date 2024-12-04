# Stacks & Queues
#cs 

## Stack

A stack  is a collection of items that come out in a LIFO (Last-In-First-Out) order

It is useful to think of a stack as a stack of books. The first book added to the stack will always be on the bottom and the last book added will be on the top.

In order to reach the first book added, you must remove every book on top of it (which is all the other books), so the first book is always the last book reached (FILO).

### **Operations**

`s = Stack()` - Creates an empty stack

`len(s)` - returns number of items in `s`

`s.is_empty()` - returns `True` if `s` is empty, or `False` otherwise

```python
def is_empty(self):
	return (len(self) == 0)
```

`s.push(item)` - inserts `item` to top of stack  `s`

```python
def push(self,item):
	if(self.is_full()):
		raise Exception ("Stack is full")
	self.data[self.n] = item
	self.n +=1
```

`s.pop()` removes and returns the item that was last to enter `s` (out of all the items currently in `s`),
or raises `Exception` if `s` is empty.

```python
def pop(self):
	if(self.is_empty()):
		raise Exception("Stack is empty")
	item = self.data[self.n-1]
	self.data[self.n -1] = None
	self.n -= 1
	return item
```

`s.top()` - returns the item that was last to enter `s` (out of all the items currently in `s`),
or raises `Exception` if `s` is empty.

```python
def top(self):
	if(self.is_empty()):
		raise Exception("Stack is empty")
	return self.data[-1]
```

### Static Stack Operations

Static stacks differ slightly from dynamic stacks in their methods:

`s = Stack(max_capacity)` - Creates an empty stack of fixed size
(replaces `s = Stack()`)

`s.is_full()` - Checks if the stack is full
```python
def is_full(self):
	return (len(self) == len(self.data))
```

## Queue

A queue is a collection of items that come out in a FIFO (First-In-First-Out) order.

It is useful to think of a queue as a group of people standing in a line, where the person thats first in line is at the front of the line, and the last is at the end.

In order for the last person in the line to leave the line, they must wait for every other person to leave the line first.

### **Operations**

`q = Queue()` - Creates an empty queue

`len(q)` - returns number of items in `q`

`q.is_empty()` - returns `True` if `q` is empty, or `False` otherwise

```python
def is_empty(self):
	return (len(self) == 0)
```

`q.enqueue(item)` - inserts `item` to the end of the queue `q`

```python
def enqueue(self,item):
	if(self.is_full()):
		raise Exception ("Queue is full")
    elif(self.is_empty()):
        self.data_arr[0] = item
        self.front_ind = 0
        self.n += 1
    else:
        back_ind = (self.front_ind + self.n) % self.capacity
        self.data_arr[back_ind] = item
        self.n += 1
```

`q.dequeue()` - removes and returns the item that first entered `q` (out of all current items in `q`)

```python
def dequeue(self):
    if(self.is_empty()):
        raise Exception("Queue is empty")
    value = self.data_arr[self.front_ind]
    self.data_arr[self.front_ind] = None
    self.front_ind = (self.front_ind + 1) % self.capacity
    self.n -= 1
    if(self.is_empty()):
        self.front_ind = None
    return value
```


`q.first()` - returns the item that entered `q` first (out of all current values of `q`), or raises an `Exception` if `q` is empty

```python
def first(self):
    if(self.is_empty()):
        raise Exception("Queue is empty")
    return self.data_arr[self.front_ind]
```

### Static Queue Operations

Static queues differ slightly from dynamic queues in their methods:

`q = Queue(max_capacity)` - Creates an empty queue of fixed size
(replaces `q = Queue()`)

`q.is_full()` - Checks if the queue is full
```python
def is_full(self):
	return (len(self) == len(self.data))
```
