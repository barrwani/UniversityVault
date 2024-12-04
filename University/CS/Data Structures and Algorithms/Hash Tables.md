	#cs 

# Hash Tables

## Hashing

Hashing involves converting a range of key values into a range of indexes of an array. 

This is done as hashing essentialy stores the data itself using the index of the array. 

Hash tables store data in this associative manner.

Data can be accessed very quickly if we know th index of the desired data. Because of this, insertion and search operations are extremely fast irrespective of size. 

Hash Tables use arrays to store data and a hash technique (hash function) to generate the index where the element is to be inserted or located. 

![[University/PastedImages/Pasted image 20221214014659.png]]

## Collisions and Chaining

Collisions occur when multiple keys are mapped to the same slot in an array.

To solve this, a technique called chaining is used, where all entries that are mapped to the same slot are stored in a secondary collection - typically an unsorted array map.

![[University/PastedImages/Pasted image 20221214020631.png]]

Chaining leads to worse performance, so in order to keep chaining to a minimum, 'good' hashing functions are used.

## Uniform Hashing Functions
A Uniform Hashing Function is a function that when given a randomly chosen key, it will be equally likely mapped to any of the N slots in the array, independent to where other keys have been hashed to.

## Multi-Function Hashing

Hash functions typically contain two separate functions to hash a key. Oftentimes this is because keys are represented using data that isn't integers that can be simply mapped to an array index, such as a string or other data type. 

![[University/PastedImages/Pasted image 20221214182500.png]]

### **Coding Function**

The coding function's role is to convert the key into a 64-bit integer. 

The following approaches exist:
- Integer Casting
- Component Sum
- Polynomial Accumulation

What we use:
- Python's built in coding function `hash(key)`
	- Can be called with built-in immutable types (int, str, float, tuple)
	- User defined classes are unhashable, unless they overload the `__hash__()` method
	- Works similar to polynomial


### **Compression Function**

The compression function's role is to convert our 64-bit integer to an index from `0, (N-1)` .

Common approaches include:

- The Division Method
	- We define `h2(k) = kmodN`
	- If keys are chosen randomly, this satisfies the  [[#Uniform Hashing Functions|Uniform Hashing Property]]
	- If keys are biased, long chains may be created.
		- N is typically chosen to be prime to prevent this

- The Multiply and Divide (MAD) method
	- Let `p` be a prime number, `p > |U|` (where |U| is the magnitude of the universe of available values) 
	- Let `a` be a random number from `{1,2,..., p-1}`
	- let `b` be a random number from `{0,1,2,...,p-1}`
	- We define `h2(k) = [(ak+b)mod p] mod N`
	- U isn't infinite since it comes from the coding function first.


## Find Method Time Analysis

In its **worst case**, all keys would be mapped to the same slot.
This means it would take $O(n)$ to scan the chain.


**Expected Time:**
- We assume our hash function satisfies the [[#Uniform Hashing Functions|Uniform Hashing Property]] 
- Let $\propto$ be the load factor of the table
	- $\propto = \frac nN$ 
	- $\propto$ = expected length of each chain
	- Expected time for find  = $O(1 + \propto)$
	- If we always maintain $n \leq N$  then $\propto  \leq 1$ then Expected Time for Find = $O(1)$


## Important Points for Implementation

- Use the built-in `hash()` function for the [[#**Coding Function**|Coding Function]] and the MAD method for the [[#**Compression Function**|Compression Function]]
- Implement [[#Collisions and Chaining|Chaining]] as an [[Maps|Unsorted Array Map]]
- For good performance, always keep $n \leq N$
	- n is the input size
	- N is the size of the array
	- This reduces the likelihood of chaining


## Implementation

An implementation of a Chaining Hash Table Map using Unsorted Array Map.

### MAD [[#**Compression Function**|Compression Function]]
 
```python
class MADHashFunction:
    def __init__(self, N, p=40206835204840513073):
        self.N = N
        self.p = p
        self.a = random.randrange(1, self.p - 1)
        self.b = random.randrange(0, self.p - 1)

    def __call__(self, key):
        return ((self.a * hash(key) + self.b) % self.p) % self.N

```



## Initialisation

```python
class ChainingHashTableMap:
    def __init__(self, N=64):
        self.table = make_array(N)
        for i in range(N):
            self.table[i] = UnsortedArrayMap()
        self.n = 0
        self.h = ChainingHashTableMap.MADHashFunction(N)
```

## Get Item

```python
def __getitem__(self, key):
    i = self.h(key)
    curr_bucket = self.table[i]
    return curr_bucket[key]
```

## Set Item

```python
def __setitem__(self, key, value):
    i = self.h(key)
    curr_bucket = self.table[i]
    old_size = len(curr_bucket)
    curr_bucket[key] = value
    new_size = len(curr_bucket)
    if (new_size > old_size):
        self.n += 1
    if (self.n > len(self.table)):
        self.rehash(2 * len(self.table))
```

## Delete Item

```python
def __delitem__(self, key):
    i = self.h(key)
    curr_bucket = self.table[i]
    del curr_bucket[key]
    self.n -= 1
    if (self.n < len(self.table) // 4):
        self.rehash(len(self.table) // 2)
```


## Check for Key

```python
    try:
        val = self[key]
        return True
    except KeyError:
        return False
```

## [[Iterators and Generators#Iterators|Iterate]]

Used to iterate through the Hash Table

```python
def __iter__(self):
    for curr_bucket in self.table:
        for key in curr_bucket:
            yield key
```

## Rehash

Rehashes the table when it fills up. Used to mitigate chaining and to keep array size at necessary level. 

Replaces `resize()`, although it stores old keys/values.

```python
def rehash(self, new_size):
    old = [(key, self[key]) for key in self]
    self.__init__(new_size)
    for (key, val) in old:
        self[key] = val
```

We reinitialise in `rehash()` to prepare the data members for the newly sized map. 

Then all keys need to be re-inserted using a for loop and `__setitem__()`
