# Iterators and Generators
#cs 


## Iterators


```python
for elem in iterable-collection:
	---
	---
	---
```


**Iterable-collection**: An object that produces an iterator via the syntax `iter(iterable-collection)`

**Iterator**: An object that exposes a series of values by making subsequent calls to `next(iterator)`


```python
>>>lst = [1,2,3]
>>>lst_iter = iter(lst)
>>>lst_iter
<list_iterator object at 0xc1eefc8>
>>> next(lst_iter)
>>> 1
>>> next(lst_iter)
>>> 2
>>> next(list_iter)
>>> 3
>>> next(list_iter)
ERROR
```

### Simulating For Loops with Iteration

Standard For Loop:
```python
s = "abc"
for item in s:
	print(item)
```


Simulated For Loop using While and Iteration
```python
s = "abc"
s_iter = iter(s)
end = False
while (end == false):
	try:
		item = next(s_iter)
		print(item)
	except StopIteration:
	end = True
	
```

### Simulating the `range(...)` function

Standard usage of Range Function:
```python
for elem in range(3,10,0.5):
	print(elem)
```


Simulated `range()` function:
```python
for elem in my_range(3,10,0.5):
	print(elem)

def my_range(start, stop, step):
	res = []
	curr = start
	while (curr < stop):
		res.append(curr)
		curr += step
	return res
```

**The entire sequence above was completely calculated before the first iteration. ls is stored (as a list) all at once in the memory**. 


## Generators

A Generator is an iterator that allows for a  `break`  in execution:

```python 
def f():
	x = 1
	yield x
	x += 1
	yield x
	x += 1
	yield x

>>> g = f()
>>> g
<generator object f at 0x1343814b8>
>>> next(g)
1
>>> next(g)
2
>>> next(g)
3
```

When `yield` is reached, a snapshot of the active data frame is taken and stored (together with the last position from where the execution should later resume)

When `next` is called, the data that was saved is restored and the execution resumes from where it left off.


**Improved Simulated `range()` Function using a Generator:**

```python
for elem in my_range(3,10,0.5):
	print(elem)

def my_range(start, stop, step):
	curr = start
	while (curr < stop):
		yield curr
		curr += step
```

When using generators we perform **lazy evaluation**, that produces an implicit iterable sequence. This sequence is used:

- Holding execution and exposing an element, only when/if its needed. 
- Without constructing a data structure that stores the elements at once in the memory.