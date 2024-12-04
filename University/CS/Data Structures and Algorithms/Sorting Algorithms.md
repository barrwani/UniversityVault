# Sorting Algorithms
#cs


## **Selection** Sort

- The smallest value in a list is initially found
  
- It is placed at the beginning of the list
  
- From there, the process is repeated. 
  
- The algorithm searches for the smallest unsorted value and places it in the front of the list after the sorted values.

```python
def selectionSort(array, size):

	for x in range(size):
		min_index = x
		
		for i in range(s + 1, size):
		# For sorting in descending order
		# for minimum element in each loop
		
		if array[i] < array[min_index]:
			min_index = i
			
		# Arranging min at the correct position
		(array[x], array[min_index]) = (array[min_index], array[x])
```

Selection sort has a time complexity of $O(n^2)$ , $\Theta (n^2)$, and $\Omega (n^2)$

## **Insertion** Sort

- In the first iteration, the first two elements in the array are compared. If the second value is smaller, it is inserted in the position of the first element.
  
- In each consecutive iteration, then next element is compared to the previous (which is contained in the sorted subarray), and placed into the subarray as a sorted element
  
- The process is repeated until the entire list is sorted.


```python
def insertion_sort(list1):

		# Outer loop to traverse on len(list1)
		for i in range(1, len(list1)):
			a = list1[i]

			# Move elements of list1[0 to i-1],
			# which are greater to one position
			# ahead of their current position
			j = i - 1
		
			while j >= 0 and a < list1[j]:
				list1[j + 1] = list1[j]
				j -= 1
				
			list1[j + 1] = a
			
		return list1

```


Insertion sort has a time complexity of $O(n^2)$ , $\Theta (n^2)$, and $\Omega (n^2)$

## **Bubble** Sort

- Two adjacent list elements are compared

- If the element on the left is larger than the element on the right, they are swapped.

- The comparison is then shifted to the next pair of adjacent list elements.

- The process is repeated until the algorithm reaches the end of the list, where it goes back to the beginning and begins comparing again.

- Once a pass is completed in which all elements are in order, the algorithm is complete and the list is sorted.

```python
def bubbleSort(arr):
	
	n = len(arr)

	# For loop to traverse through all
	# element in an array
	for i in range(n):
		for j in range(0, n - i - 1):
			
			# Range of the array is from 0 to n-i-1
			# Swap the elements if the element found
			#is greater than the adjacent element
			if arr[j] > arr[j + 1]:
				arr[j], arr[j + 1] = arr[j + 1], arr[j]


```

Bubble sort has a time complexity of $O(n^2)$ , $\Theta (n^2)$, and $\Omega (n)$

## **Merge** Sort

Merge sort is a **recursive divide and conquer** algorithm.

- Recursively sort the first half
- Recursively sort the second half
- Merge the two halves together into one sorted sequence


### Example of Merge Sort

```python
def merge_sort(lst):
	if (len(lst) == 0):
		return
	elif (len(lst) == 1):
		return
	else:
		mid = (len(lst) -1) // 2
		left_lst = lst[ : mid +1]
		right_lst = lst[mid+1: ]
		merge_sort(left_lst)
		merge_sort(right_lst)
		mergedlst = merge(left_lst, right_lst)
		lst [ : ] = merge[ : ]

def merge(srt_lst1, srt_lst2):
	merged_list = []
	i1 = 0
	i2 = 0
	while ((i1 < len(srt_lst1)) and (i2 < len(srt_lst2))):
		if (srt_lst1[i1] < srt_lst[i2]):
			merged_list.append(srt_lst1[i1])
			i1 += 1
		else:
			merged_list.append(srt_lst2[i2])
			i2 += 1
	while (i1 < len(srt_lst1)):
		merged_list.append(srt_lst1[i1])
		i1 += 1
	while (i2 < len(srt_lst1)):
		merged_list.append(srt_lst2[i2])
		i2 += 1
	return merged_list
```

### merge_sort()
Here, a list is recursively split into halves until all elements are in their own lists. 

They are then merged through the `merge()` function, which will sort them. 

The merging is done until all elements have been merged, and the list is sorted.


### merge()
Here, two variables are stored which indicate the index of the lowest number in each list half.

The lowest value between the two is then appended to the merged list, and the process is repeated until all elements have been merged back in order.

