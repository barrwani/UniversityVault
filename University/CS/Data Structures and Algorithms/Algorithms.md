# Analysing Algorithms
#cs 

## Correctness

 - Correct for Every Valid Input if it:
	- Terminates (Halts)
	-  Provides the desired output

### Performance
-  Measure the resources an algorithm requires:
	- Time of computation
	- space (memory use)
	-  disk use
	- disk-memory communication
	- number of processors (in a parallel program)
	- amount of communication (in a distributed program)
	- etc.


# Primality Testing

### Version 1:

Code checks every integer from 1 to n looking for a factor of n. 

```python
def is_prime(num):
	count_divs = 0
	for curr in range(1, num+1):
		if (num % curr == 0):
			count_divs +=1
	if (count_divs == 2):
		return True
	else:
		return False
```

Time complexity of $\Theta (n)$

### Version 2:

Code checks every integer from 1 to n/2 looking for a factor of n.

```python
def is_prime(num):
	count_divs = 0
	for curr in range(1, (num//2)+1):
		if (num % curr == 0):
			count_divs +=1
	if (count_divs == 1):
		return True
	else:
		return False
```


Time complexity of $\Theta (n)$

### Version 3:

Code checks every integer from 1 to $\sqrt{n}$  looking for a factor of n.
```python
def is_prime(num):
	count_divs = 0
	for curr in range(1, math.sqrt(num)):
		if (num % curr == 0):
			count_divs +=1
	if (count_divs == 1):
		return True
	else:
		return False
```
Time complexity of $\Theta (\sqrt n)$



## Problem Solving Methods

### Two Pointers

- Two pointers is an effective technique for searching pairs in a sorted array. It involves the use of two different pointers to keep track of list indices.

- Solutions may involve using the two pointers to represent boundaries, or one pointer which moves at a slow pace and another which moves at twice the speed. 

#### Sliding Window

- Used to replace nested loops with a single loop to reduce time complexity.
  
- It is essentially a sublist that moves through the entire list, checking, storing, or modifying values.
  
- An example problem could involve finding the largest sum of five consecutive list elements.

The bounds of the sliding window can be defined using two pointers.

#### Moving Inwards/Outwards

- Involves having the two pointers move towards/away from each other in a list

Other methods include [[Recursion]] and reordering elements in place :
(`lst[a], lst[b] = lst[b], lst[a]`)
