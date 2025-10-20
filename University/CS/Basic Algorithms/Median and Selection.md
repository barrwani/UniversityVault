# Median and Selection
#cs #math 

#### **Selection Problem**

**Input:** An array of $n$ distinct numbers and an index of $i \in \{1,...,n\}$

**Goal:** Output $i$-th smallest element.

- $i = \frac n2$ is median.
- Case: $i = 1$: linear time $\Theta(n)$
- Case: $i =n$: same.
- Possible in linear time for any $i$.


Implies $\Theta(n\log n)$ quicksort algorithm using median as pivot.

>[!NOTE] **Definition:** 
>An **Approximate Median** is an element that is not in the bottom or top 30th percentile. 

```
SELECT(A,i):
	1. j = APPROXMEDIAN(A)
	2. k = PARTITION(A,j)
	3. if k == i:
		return A[k]
	4. else if k > i:
		return SELECT(A[1...k-1], i)
	5. else: //k < i
	   // i-k th smallest in right part is i-th smallest overall 
		return SELECT(A[k+1...n],i-k) 
```

**Correctness:** Algorithm outputs $i$-th smallest element no matter how you implement.

##### **Implementation 1:** (fast, randomized)

- Use random index $j \in \{1,..,n\}$
- With probability 40% we get approx. median. For simplicity, assume it always gives  an element in the 30th or 70th percentile. 

**Runtime:** $$T(n) = T\left(\frac 7 {10}n\right) + c \cdot n$$$$ = c \cdot n + c \cdot \frac {7}{10}n + c \cdot \frac{49}{100}n+...$$
$$= \Theta(n)$$

##### **Implementation 2:** (deterministic, very slow)

- Apply recursively:
	- `MEDIAN(A[1... 3/5 n])`
	- `SELECT(A[1...3/5n], 3/10n)`

Always outputs approx. median of $A$. 

This is because the output has $\geq 0.3 \cdot n$ elements above it and also $\geq 0.3 \cdot n$ below it.
![[Pasted image 20251020152457.png|200]]

**Runtime:**
$$T(n)  = T\left(\frac 35 n\right) + T\left(\frac 7{10} n\right) + c \cdot n$$
$$ = c \cdot n + c \cdot \frac{13}{10}n + c \cdot (\frac{13}{10})^2n +...+ c \cdot (\frac{13}{10})^{\log_{\frac{10}{7}}n} \cdot n$$
$$ = \Omega(n^{1.73...})$$

##### **Implementation 3:** (Median of medians)

- Partition $A$ into $\frac n5$ groups of size 5
- Find median inside each group (3rd smallest)
- Output the median of these $\frac n5$ medians.

**Claim:** This outputs approx. median.

**Proof:** The output has $\frac n{10}$ medians smaller than it, and each one of them has two more elements from its group that are even smaller.

So, $\frac 3{10}n$ elements are smaller than the output. similarly for larger than the output.


**Runtime:** 
$$T(n) = T(\frac n5) + T(\frac7{10}n) + c \cdot n$$
$$ = c \cdot n + c \cdot \frac {9}{10}n + c \cdot \left(\frac {9}{10}\right)^2 n + ... +  c \cdot \left(\frac {9}{10}\right)^{\log_{\frac{10}{7}}n} n$$
$$= \Theta(n)$$
