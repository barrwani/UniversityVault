# Merge & Quick Sort
#cs #math 

## Merge Sort

#### Merging

- First, the merging algorithm:

```
MERGE(A[1..m], B[1..n])
	i=1
	j=1
	for k = 1 to m+n
		if A[i] < B[j]
			C[k] = A[i]
			i = i+1
		else
			C[k] = B[j]
			j= j+1
			
	return C[1..m+n]
```
#### Proof of correctness

Proving by [[Induction]]. 

**Loop invariant**: At the beginning of the $k^{th}$ iteration, $C[1 ... k-1]$ contains the $k-1$ smallest elements of $A \cup B$ in sorted order, and these elements are $A[1,..,i-1]$ and $B[1,...,j-1]$.

**Initialization:** $k=1$, $i=1$, $j=1$, all OK.

**Maintenance:** If true for $k$, also true for $k+1$.

**Termination:** If loop invariant holds at the end of last iteration $(k = m+ n + 1)$, $C$ contains all elements of $A \cup B$ in sorted order.


**Runtime:** $\Theta(m+n)$ 

#### Sorting

-  We use "divide-and-conquer" for sorting. 

```
MERGESORT(A[1..n])
	1. M1 = MERGESORT(A..n/2)
	2. M2 = MERGESORT(A[(n/2)+1..n])
	3. MERGE(M1,M2)
```


#### Proof of correctness

Correctness follows easily from MERGE.

#### Runtime Analysis

**Denote** by $T(n)$ the runtime on input of size $n$. 

Then,

$T(n) =$ Step 1 + Step 2 + Step 3 ($O(n)$)
$$T(n) = T(\frac n2) + T(\frac n2) + c \cdot n$$
$$ \implies T(n) = T(\frac n2) + T(\frac n2) + n$$
$$ \implies T(n) = 2 \cdot T(\frac n2) + n$$

Three methods:

1. Recursion Tree
2. Substitution method (induction basically)
3. Master theory, basically recipe


##### **Recursion Tree:** 
$$T(n) = T(\frac n2) + T(\frac n2)$$ $$T(n) =  2 \cdot T(\frac n2) + n$$$$= 2 \cdot( 2 \cdot T(\frac n4) + \frac n2) +n$$
$$ = (\frac n2 + \frac n2) = (\frac n4 + \frac n4 + \frac n4 + \frac n4 )  = (\frac n8 + \frac n8 +\frac n8 +\frac n8 +\frac n8 +\frac n8 +\frac n8 +\frac n8)=......$$
$$ = 1 +1 + 1 +..... + 1$$
Depth of recursive tree is $\log(n)$. Number of individual ($1$) operations is $n$. So:

Total: $T(n) = n \log(n)$


##### **Substitution Method:**

**Claim:** $\forall n \leq 2$, $T(n) \leq 10 \cdot n \cdot \log_2(n)$  

This will imply that $T(n) = O(n\log_2(n))$

**Proof:** Proving by induction.

Base case: $n = 2$. 

Inductive Step: Assume inductive hypothesis holds for $1,2,...,n-1$, lets prove for $n$. 

$$T(n) = 2 \cdot T(\frac{n}2) + n$$
$$\leq 2 \cdot(10 \cdot \frac n2 \cdot \log_2(\frac n2)) +n$$
>[!NOTE] Note on $\log$:
>$\log_2 8 = 3$
>$\log_2 16 = 4$
>$\log_2 (\frac n2) = \log_2 (n-1)$

$$10 \cdot n \cdot (\log_2 (n-1)) +n$$
$$= 10 \cdot n \cdot \log_2n - 9\cdot n$$
$$\leq 10 \cdot n \cdot \log_2n$$

### Examples
#### Ex 1:

**Prove:** $T(n) \geq 0.1 \cdot n \log_2n$
This will imply that $T(n) = \Omega(n\log n)$. 

**Proof:**


#### Ex 2:

**Prove:** Prove $T(n) \leq 10 \cdot n^2$

**Proof:** 

$$T(n) = 2 \cdot T(\frac n2) + n$$
$$\leq 2 \cdot (10 \cdot \frac {n^2} 2) +n$$
	$$= 10 \cdot n^2$$
$$\leq 10 \cdot n^2$$

#### Ex 3:

**Prove:** Try to prove the **wrong** claim that $T(n) \leq 100 \cdot n$

**Attempted Proof:**

$$T(n) = 2 \cdot T(\frac n2) + n$$
$$\leq 2 \cdot (100 \cdot \frac n 2) + n$$
 $$= 101 \cdot n$$
This is **not** the induction hypothesis we were supposed to prove. 

 >[!NOTE]
 >If the proof mentions a constant, you must prove by that constant.



## Quick Sort

- Also based on divide-and-conquer

- "Merge sort in reverse"


Main idea: 

**"Divide"** 

1. Select **pivot** element
   - For simplicity lets use the last element
 
1. Partition: 
   - Everything smaller than pivot goes left.
   - Everything bigger goes right.
   - Pivot in between.
   - Notice that pivot is in correct position at this point.

**"Conquer"** 
1. Recursively sort both parts.



```
QUICKSORT(A[1..n])
	1. k = PARTITION(A) -> k is final and correct position of pivot
	2. QUICKSORT(A[1..k-1])
	3. QUICKSORT(A[k+1..n])
```


**Correctness:** more or less obvious

```
PARTITION(A[1..n])
	1.pivot = A[n]
	2. i =1
	3. for j = 1 to n-1:
		   if A[j] <= PIVOT:
				SWAP a[j]
```


### Runtime of Quicksort

**Worst Case:** when the input is already sorted and we use the last element as pivot. Each call to partition just puts everything on the left. 

$$T(n) = T(n-1) + T(0) + c \cdot n$$
Where $T(n-1) =$ recursion on left, $T(0) =$ recursion on right, $c \cdot n =$ partition. So we have worst case runtime total of:
$$c \cdot \sum_{j=1}^{n} j = \Theta (n^2)$$ **Best Case:** choose median as pivot. Recurrence is $$T(n) = T(\frac n2) + T(\frac n2) + c \cdot n$$
$$= 2 \cdot T(\frac n2) + c \cdot n$$

Same as with merge sort, so we have $$T(n) = \Theta(n \log n)$$
**Average Case:** 

Assume for simplicity that all elements are distinct.

Last element in the array is unlikely to be in the top or bottom 10 percentiles:
- 80% probability that pivot is not top or bottom 10 percentile
- Assume pivot is always in 10th percentile

Runtime is: $$T(n) = T(\frac n{10})  +T (\frac {9n}{10}) + c \cdot n$$
$$ = T(\frac{n}{100}) + T(\frac{9n}{100}) + T(\frac{9n}{100}) + T(\frac{81n}{100}) + c \cdot n$$
Cost per layer is always $c \cdot n$.

>[!NOTE] Tip for finding number of levels
>Suppose at every partition, the larger of the two subarrays has at most size $pn$ where $0 < p < 1$.
>
>After one level the largest subproblem is size $pn$. After $k$ levels the largest subproblem is $p^k n$. 
>
>Stop when $p^k n \leq 1$
>$$k \geq \log_p (\frac 1n) = -\log_p n = \log_{\frac 1p}n$$
>
>So number of levels is $\Theta(\log n)$ base $\frac 1p$

For average case we have a biased split of $\frac 9 {10}$ and $\frac 1 {10}$. We take the larger subarray as $p = \frac 9 {10}$. 

So number of layers is $\log_{\frac {10} 9} n$. 

So on average, running time is $$\log n \cdot c n = \Theta (n \log n)$$



