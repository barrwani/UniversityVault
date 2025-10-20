# Sorting Algorithms
#cs #math 


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


