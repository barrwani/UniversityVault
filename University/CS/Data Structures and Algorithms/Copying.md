# Copying
#cs

## Mutation vs Construction

Below is a table of methods which can be used to either mutate and existing list or construct a new list.

Mutating an existing list will result in the reference to the list being shared, whereas constructing a new list will result in a new object and reference being created.

>[!NOTE]
>Functions and Methods are not the same. 
>Functions are defined outside of the class, whereas methods are defined within it.
>

Mutating Existing Lists | Constructing New Lists
---------------|-----------------------
indexed assignment (`lst[i] = __ `) | list literals ( `[1,2,3], []`)
`append()` method| list constructor (`list(...)`)
`insert()` method | `copy.copy()` function
`pop()` method | `copy.deepcopy()` function
`reverse()` method | `+ `  operator
`sort()` method | `* ` operator
`extend()` method| slicing ( `lst[_:__]`)
`+= ` operator| list comprehension

#### Shallow Copy vs Deep Copy 
In Shallow copy, a new list object is created with a new container (to store the items) but the contents (of the original list) are shared.

Shallow Copy is much faster than Deep Copy. 

This is because Shallow Copy stores the references of objects to the original memory address. Any changes made in one list will be reflected in the other. 

Shallow Copy simply copies the **surface level references** to objects.
```python
- import copy
- use copy.copy(...) function
- use copy method in list class (lst.copy())
```

![[University/PastedImages/Pasted image 20220912114917.png|300]] ![[University/PastedImages/Pasted image 20220912115113.png|300]] 

In Deep Copy, a new list object is created with a new container (to store the items) but the contents (of the original list) are nestedly copied as well. 

Deep Copy is much slower than Shallow Copy.

Deep Copy copies **every single item** and **creates new references** to connect them, rather than just copying surface level references.
```python
- import copy
- use copy.deepcopy() function
```


![[University/PastedImages/Pasted image 20220912114308.png|300]] ![[University/PastedImages/Pasted image 20220912114331.png|300]]

#### List Comprehension

```python
lst1 = [Counter() for i in range(3)]
for c in lst1:
	c.inc()
	print(c)
```

#### * Operator and + operator
The + operator will combine two arrays to construct a new one, for example:

```python
>>> x = [1,2,3]
>>> y = [4,5,6]
>>> x+y
[1,2,3,4,5,6]
```

The * operator will create a new list by multiplying an existing list with an integer, for example:

```python
>>> x = [1,2,3]
>>> x * 3
[1,2,3,1,2,3,1,2,3]
```

#### Slicing
List Slicing will construct a new list from a sublist of an existing list:

```python
>>> x = [1,2,3,4]
>>> y = x[1:4]
>>> y
[2,3,4]
```

#### Extend
Extend will modify an existing list, extending it with another lists values:

```python
>>> x = [1,2,3]
>>> y = [4,5,6]
>>> x.extend(y)
>>> x
[1,2,3,4,5,6]
```

