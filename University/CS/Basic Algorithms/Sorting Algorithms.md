# Sorting Algorithms
#cs 


## Insertion Sort

Sorting left to right, shifting each number leftwards until everything to the left of it is in sorted order. 

```cpp
void insertionsort(int arr[], int n)
{
	for (int i = 1; i < n; i++)
	{
		int key = arr[i];
		int j = i - 1;
		
		while(j >= 0 && arr[j] > key)
		{
			arr[j+1] = arr[j];
			j = j - 1 ;
		}
		arr[j+1] = key;
	}
}
```
### Proof of correctness

Proving by [[Induction]]. 

**Loop invariant**: at the beginning of the $j^{th}$ iteration (or equivalently, end of $j-1^{st}$ iteration), $A[1..j-1]$ contains the same elements it had initially, but in sorted order.

If true, then at the end of the $n^{th}$ iteration, $A$ is sorted. (termination condition)

Claim: loop invariant holds for $j = 2,3,...,n+1$.

**Proof by induction:** when $j = 2$ (inductive base) $A[1]$ is just one number and therefore sorted.

**Inductive step:** Assume claim holds for $j$, we will show it holds for $j+1$. By assumption, $A[1...j-1]$ is sorted. during the $j^{th}$ iteration we insert $A[j]$ into its correct position. Therefore invariant still holds. 



#### Runtime

$$\sum_{j=2}^n (c + c^\prime  \cdot t_j)$$
$t_j =$ # of iterations of $j^{th}$ inner loop.
$c, c^\prime =$ some constants. 
$\sum_{j=2}^n =$ outer loop.


$$ = c \cdot (n-1) + c ^\prime \cdot \sum_{j=2}^n t_j$$
##### Worst case analysis:
$$\leq  c \cdot (n-1) + c ^\prime \cdot \sum_{j=2}^n j$$
$$\leq  c \cdot (n-1) + c ^\prime \cdot \frac{n \cdot n+1}{2} -1$$
$$= \frac{c^\prime}{2} \cdot n^2 + (c+ \frac{c^\prime}{2}) \cdot n - (c+ c^\prime)$$
$$ = \Theta(n^2) $$

- Worst-case running time. 

- If the array is randomly shuffled, $t_j$ is roughly $\frac{j}2$ on average, so we save factor of 2 in running time compared to worst case. 

- This is asymptotically still $\Theta(n^2)$ .

- Insertion Sort is an "in place" algorithm. Only constant extra memory is needed. 

- It is also "stable": identical keys in input remain in the same order.


### Cheat Sheet:

| Notation | Means  |
| -------- | ------ |
| $O$      | $\leq$ |
| $\Omega$ | $\geq$ |
| $\Theta$ | =      |

**Def:** for functions $f, g : \mathbb{N} \rightarrow \mathbb{R}^{+}$ we say

1) $f = O(g)$ if $\exists$ a constant $c$ such that for all big enough $n$: $$\frac{f(n)}{g(n)} \leq c$$
2) $f = \Omega(g)$ if $\exists$ a constant $c$ such that for all big enough $n$: $$\frac{f(n)}{g(n)} \geq c$$
3) $f = \Theta(g)$ if $\exists$ constants $c_1, c_2$ such that for all big enough $n$: $$c_1 \leq \frac{f(n)}{g(n)}   \leq c_2$$
In other words, $f = O(g)$ AND $f = \Omega(g)$




#### Examples

- $2n^2 + 7n + 11 = O(n^2)$ 
	- True because $\frac{2n^2+7n+11}{n^2} \leq 3$ for big enough $n$

 - $2n^2 + 7n + 11 = \Omega(n^2)$ 
	 - True because $2n^2 + 7n + 11 \geq 1$

- $2n^2 + 7n + 11 = \Theta(n^2)$ 
	- True because $1 \leq 2n^2 + 7n + 11 \leq 3$ 

- $2n^2 + 7n + 11 = \Theta(n^3)$ 
	- False because $\frac{2n^2 + 7n + 11}{n^3}$ tends to 0 as $n$ grows
  
- $2n^2 + 7n + 11 = O(n^3)$ 
	- True because $\frac{2n^2+7n+11}{n^3} \leq 3$ for big enough $n$

- $n\log_2(n) = O(n^2)$
	- True $\frac{n\log_2(n)}{n^2} \rightarrow_{n\rightarrow\infty}0$   

- $n\log_2(n) = O(n)$
	- False $\frac{n\log_2(n)}{n} \rightarrow_{n\rightarrow\infty}\infty$

- $4^n = O(2^n)$
	- False $\frac{4^n}{2^n} = 2^n \rightarrow_{n \rightarrow \infty} \infty$ 
	  
- $2^{n+3} = O(2^n)$
	- True $2^{n+3} = 2^3 \cdot 2^n = 8 \cdot 2^n$


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

**Runtime:** Denote by $T(n)$ the runtime on input of size $n$. 

Then,

$T(n) =$ Step 1 + Step 2 + Step 3 ( $O(n)$)
$$T(n) = T(\frac n2) + T(\frac n2) + c \cdot n$$
$$ \implies T(n) = T(\frac n2) + T(\frac n2) + n$$
$$ \implies T(n) = 2 \cdot T(\frac n2) + n$$

Three methods:

1. Recursion Tree
2. Substitution method (induction basically)
3. Master theory, basically recipe


##### **Recursion Tree:** $$T(n) =  2 \cdot T(\frac n2) + n$$
$$= 2 \cdot( 2 \cdot T(\frac n4) + \frac n2) +n$$
$$T(n) = T(\frac n2) + T(\frac n2)$$
$$ = (\frac n2 + \frac n2) = (\frac n4 + \frac n4 + \frac n4 + \frac n4 )  = (\frac n8 + \frac n8 +\frac n8 +\frac n8 +\frac n8 +\frac n8 +\frac n8 +\frac n8)=......$$
$$ = 1 +1 + 1 +..... + 1$$
Depth of recursive tree is $\log_2(n)$. Number of individual ($1$) operations is $n$. So:

Total: $T(n) = n \log_2(n)$




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

"Divide" 

1. Select **pivot** element
   - For simplicity lets use the last element
     
2. Partition: 
   - Everything smaller than pivot goes left.
   - Everything bigger goes right.
   - Pivot in between.
   - Notice that pivot is in correct position at this point.
    
"Conquer" 
3. Recursively sort both parts.



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


