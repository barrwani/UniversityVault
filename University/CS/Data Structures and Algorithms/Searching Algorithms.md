# Searching Algorithms
#cs 

## **Linear** Search

A basic linear search algorithm **returns an index** in list `lst` for a specified value `val`, or `None` if `val` is not one of `lst`'s elements.

```python
def linear_search(lst, val):
	for i in range(len(lst)): #O(n)
		if (lst[i] == val): #O(1)
			return i
			
	return None #O(1)
```

The runtime cost of this algorithm is $\Theta(n)$

## The **Sorted Search** Problem

The function is given a sorted list `srt_lst` and a value `val` to search for. It should return an index where `val` appears, or `None` if `val` isn't an element in `srt_lst`.

Using a basic linear search algorithm to solve this problem will lead to a worstcase runtime of $\Theta(n)$.

Because we know the array is sorted, we don't have to check every value, which takes unnecessary amounts of time.

### **Binary** Search

Since we know the list is sorted, we can check the midpoint and see if our value `val` is greater than or less than it.  (Don't forget edge case `val == midpoint`)

From there, only half of the list will have to be checked. 

This process can be repeated using list slicing, checking smaller and smaller sublists until our value is found (or not).

Using two pointers to indicate the leftmost and rightmost indexes

```python
def binary_search(srt_lst, val)
	left = 0
	right = len(srt_lst) - 1
	ind = None
	found = False
	
	while((found == False) and (left <= right)):
		mid = (left + right) // 2
		
		if (srt_lst[mid] == val):
			ind = mid
			found = True
			
		elif (val < srt_lst[mid]):
			right = mid - 1
			
		else:
			left = mid + 1
			
	return ind
```

Here, the time complexity and number of operations is $\Theta (\log_2(n))$. This can be found by doing the following:

Lay out the size of the searching range in relation to the iteration number, finding a common equation for the size $\frac{n}{2^{k-1}}$
![[University/PastedImages/Pasted image 20221021193340.png|250]]

In order to find the number of iterations k when the searching range is 1, we do the following:

$$\frac{n}{2^{k-1}} = 1$$
$$n = 2^{k-1}$$

Solving for k, we get:

$$k = 1 + \log_2(n) = \Theta (\log_2(n))$$

This is much more efficient than $\Theta (n)$ , 

![[University/PastedImages/Pasted image 20221021193955.png]]