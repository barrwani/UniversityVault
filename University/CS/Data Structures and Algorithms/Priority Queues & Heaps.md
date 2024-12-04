# Priority Queues
#cs 


A priority [[Stacks & Queues|Queue]] is an ADT in which every element is assigned a priority value, and the element with the highest value is served before ones with lower priority values. 

## Operations

`p = PriorityQueue()` - Initialises an empty Priority Queue Object
`p.insert(pri, val)` - Inserts an item with priority `pri` and value `val`
`p.min()` - Returns the pair `pri,val` with the lowest value, or raises `Exception` if `p` is empty 
`p.delete_min()` - Removes and returns the pair `pri, val` with the lowest priority in `p`, or raises `Exception` if `p` is empty 
`len(p)` - Returns the number of items in  `p`
`p.is_empty()` - Returns `True` if empty, `False` otherwise


## Min vs Max

A minimum priority queue puts priority on the elements with the lowest `pri` values. 

A maximum priority queue puts priority on the elements with the highest `pri` values.

# Heaps

A [[Rooted Binary Trees|binary tree]] is considered a Heap if it satisfies:

- **Heap Order Property** - In every node of the tree, the priority of `n` is less than or equal to the priorities of n's children
- **Nearly-Complete Property** - If `H` is the height of the tree, then all levels {0, 1, 2, ..., `H`-1} have the maximum number of nodes possible (level `i` has $2^i$ nodes), and the remaining nodes at level `H` reside in the leftmost positions.

The height of a heap with $n$ nodes is $O(\log n)$ 

![[University/PastedImages/Pasted image 20221217001049.png]]

## Time Complexity for Priority Queue ADTs

![[University/PastedImages/Pasted image 20221217001125.png]]


## Tree vs Array Representation

Heaps can be represented in two ways:

![[University/PastedImages/Pasted image 20221218163151.png|400]]

