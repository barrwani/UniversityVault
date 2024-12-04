# The Map ADT
#cs 


The Map ADT stores multiple (key, value) pairs.

## Operations

`m = Map()` - Creates an empty map

**Insert:**
`m[k] = v` - Adds value `v` with key `k` to `m`, if `k` already exists in `m` it replaces the value already associated with `k` with `v`

**Find:**
`m[k]` - Returns the value associated with `k` in `m`, or raises `KeyError` if `k` is not in `m` 

**Delete:**
`del m[k]` - Removes the value associated with `k` in `m`, or raises `KeyError` if `k` is not in `m`

`len(m)` - Returns the number of entries in `m`
`iter(m)` - returns an iterator that allows to iterate over the keys in `m`


## Code

An implementation of an unsorted array map data structure. 

Item Class:

```python
class Item:
    def __init__(self, key, value=None):
        self.key = key
        self.value = value
```

Find:
```python
def __getitem__(self, key):
    for item in self.table:
        if key == item.key:
            return item.value
    raise KeyError("Key Error: " + str(key))
```

Insert:
```python
def __setitem__(self, key, value):
    for item in self.table:
        if key == item.key:
            item.value = value
            return
    self.table.append(UnsortedArrayMap.Item(key, value))
```

Delete:
```python
def __delitem__(self, key):
    for j in range(len(self.table)):
        if key == self.table[j].key:
            self.table.pop(j)
            return
    raise KeyError("Key Error: " + str(key))
```

[[Iterators and Generators|Iterate]]:
```python
def __iter__(self):
    for item in self.table:
        yield item.key
```


## Data Structures with Map ADT

The Map ADT may be used with other data structures for additional functionality. 

Below is a table comparing the time-space analysis of different functions used in tandem with the Map ADT
![[University/PastedImages/Pasted image 20221214005945.png]]