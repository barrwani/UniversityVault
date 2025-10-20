# Counting & Radix Sort
#cs #math 


## Counting Sort

Assume all keys are all in $\{0,1,...,k-1 \}$ 

```COUNTSORT(A)
	1. L = array of k empty linked list
	2. for j = 0 to n-1: // O(n)
		L[A[j].key].append(A[j])
	output = []
	for i = 0 to k-1: // O(n+k)
		output.extend(L[i])
	return output
```

**Correctness**: obvious.

**Runtime:** $\Theta(n) + \Theta(n+k) = \Theta(n+k)$

Stable. As long as $k = O(n)$ the algo is $\Theta(n)$.


## Radix Sort

When array contains slightly bigger numbers (for example $k = n^3$), Count Sort is too slow. So we use Radix Sort.

Assume keys are in $\{0,1,2,...,k^d -1\}$ for some numbers $k,d$.

Take this example with $k = 10, d = 3$
	$A = \{643,522,179,991,223\}$

We sort first by the unit digit, then by the 10-digit, and finally by the 100-digit:
$$	$A = \{643,522,179,991,223\}$$$$= \{991, 522,643,223,179\}$$$$= \{522, 223,643,179,991\}$$$$= \{179, 223,522,643,991\}$$
>[!NOTE]
>Think of keys as being $d$ digit numbers in base $k$, meaning that they're in $\{0,1,...,k-1\}$


```
RADIXSORT(A)
	1. for i =1 to d:
	   COUNTSORT(A) // based on i-th least significant digit
```

**Correctness for $d =2, k=10$** : At the end of the first iteration, the numbers are sorted by their unit digit.

After the second iteration, all keys in range $0-9$ come first, then all keys in $10-19$, etc. 

Because count sort is **stable inside each group**, the numbers are sorted by unit digit, so the array is sorted.

**Runtime:** $\Theta(d \cdot (n+k))$. 

If $d$ is constant and $k = O(n)$, runtime is $\Theta(n)$.

For instance, if keys are numbers from $1,...,n^{10}$ then we can sort in linear time $\Theta(n)$. 

