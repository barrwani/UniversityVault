# Dynamic Programming
#cs #math 

## Fibonacci

$$F_1 = F_2 = 1$$
$$\forall n \geq 3, F_n = F_{n-1}+ F_{n-2}$$
Naive recursive algorithm:

```FIB(n):
	if n <= 2:
		f = 1
	else:
		f = fib(n-1) + fib(n-2)
	return f
```

**Correctness**: yes

**Runtime:** $T(n) = T(n-1) +T(n-2) + c$

Exponential in $n$, very slow.

Solution is caching.

```
memo = {}
fib(n):
	if n in memo:
		return memo[n]
	if n <= 2:
		f = 1
	else:
		f = fib(n-1) + fib(n-2)
	memo[n] = f
	return f
```


**Runtime:** No recurrence in dynamic programming runtime. 

$$\text{\# Subproblems} \times \text{time per subproblem (excluding recursive calls)}$$
$$ = n + \Theta(1) = \Theta(n)$$

We can use a bottom-up version of DP (no recursion):

```
fib = []
for k = 1 to n:
	if k <=2:
		f = 1
	else:
		f = fib[k-1] + fib[k-2] // array lookups, not recursive calls
	fib[k] = f
return fib[n]
```

Same runtime $\Theta (n)$, but we also used $\Theta (n)$ memory. 


## Rod Cutting

**Input:** Given a number $n$ and a price list $P_1,...,P_n$

![[Pasted image 20251020200923.png]]

**Goal:** Compute optimal revenue $r_n$ obtainable from $n$ feet rod.

**Remark:** Number of ways to cut a rod of length $n$ is $2^{n-1}$. Even ignoring permutations, the number of partitions is huge.

**Remark:** A natural **greedy** approach is to take the piece with the highest unit price ($ / meter). Unfortunately this might not give the optimal solution. 

For example, when $n =4$ we get $3+1$ partition which is $\$9$ instead of optimal $\$ 10$.


### **Dynamic Programming** 

1. Subproblems
	- $r_0, r_1,...,r_n$ where $r_j$ is optimal revenue for length $j$ rod
	- $r_8 = \text{max}(p_1 + r_7, p_2 + r_6, ...)$
	  
2. Guess the position of the first cut in the optimal solution (length of the first piece)
	- If the optimal revenue $r_n$ is achieved by a partition whose first piece is of length $i$, then $r_n = p_i + r_{n-1}$
	  
3. Recurrence
	- $r_n = \text{max}(p_1 + r_{n-1}, p_2 + r_{n-2},..., p_n + r_0)$
	- $r_0 = 0$
	  
4. Algorithm
	1. Top Down
	``` 
		CUTROD(n)
		if n in memo:
			return memo[n]
		if n == 0:
			return 0
		q = -infinity
		for i = 1 to n:
			q = max(q, p_i + CUTROD(n-i)) // recurrence from (3)
		memo[n] = q
		return q
	```
	 - Runtime  $\Theta(n^2)$
	   
	2. Bottom Up
	```
	CUTROD(n)
		r[0] = 0
		for j = 1 to n:
			q = -infinity
			for i = 1 to j:
				if p_i + r[j-i] > q:
					q = p_q + r[j-i]
					s[j] = i
			r[j] = q
		return r[n]
	```
	 - Runtime  $\Theta(n^2)$

**Remark:** instead of first cut being optimum, we might want to guess one of the cuts. This is as good as previous solution. 

Recurrence will be: $r_n = \text{max}(r_1 + r_{n-1}, r_2 +r_{n-2},..., r_{n-1} + r_1, P_n)$

We're guessing that the optimum has a cut at $\{1,2,...,n-1\}$, with $n$ having no cuts at optimum.

Runtime is $\Theta(n^2)$. 

**Remark:** can try to guess a piece of the optimum solution. Correct but much slower.

## Longest Common Subsequence


"HY**P**ERL**INK**ING"
"DOL**P**H**IN**SPEA**K**"

Both have common subsequence "**PINK**". 

**Problem:** given strings $X = X_{1...,m}, Y = Y_{1,...,n}$, find (the length of) the longest common subsequence.

1. **Subproblems:** `for i = 0,...,m` and `j = 0,...,n`, let $c[i,j]$ be the length of LCS of $X_{1,...,i}$ and $Y_{1,...,n}$. 
2. **Guess:** if $X_i \neq Y_j$, which of them is **not** in the LCS of $X_{1..i}$ and $Y_{1..j}$
3. **Recurrence:** $c[i,j] =$:
	1. $0$ if $i = 0$ or $j=0$
	2. $c[i-1,j-1] +1$ if $i> 0$ and $j>0$ and $X_i = Y_j$
	3. $\text{max}(c[i,j-1], c[i-1,j])$ if $i > 0, j>0$ and $X_i \neq Y_j$
4. **Algorithm:** (bottom up)
	```
	for i = 0 to m:
		c[i,0] = 0
	for j = 0 to n:
		c[o,j] = 0
	for i = 1 to m:
		for j = 1 to n:
			if X_i == Y_j:
				c[i,j] = c[i-1, j-1]+1
			else if c[i-1,j] >= c[i,j-1]:
				c[i,j] = c[i-1,j]
			else:
				c[i,j] = c[i, j-1]
	return c[m,n]
	```

**Runtime:** $\Theta(m \cdot n)$ 


**Claim:** Recurrence is correct. 

**Proof:** Base case $i=0$ or $j = 0$. 

Assume $i> 0$ and $j >0$

Two cases:

**Case 1:** $X_i = Y_j$. We want to show that $c[i,j] =c[i-1,j-1]+1$.

**Part 1:** $c[i,j] \geq c[i-1,j-1]+1$ 

Take LCS of $X_{1,...,i-1}, Y_{1,...,j-1}$ and append the letter $X_i = Y_j$ to it.

**Part 2:** $c[i,j] \leq c[i-1,j-1]+1$

Take LCS of $X_{1,...,i-1}, Y_{1,...,j-1}$ and remove it's last letter. Result is a common subsequence of $X_{1,...,i-1}, Y_{1,...,j-1}$

## Simplifying DP

Dynamic Programming is all about combining your subproblem results to build the solution for a larger one. 

- Lay out clear base cases
- Choose your subproblem operation(s):
	- `min` - least cost, fewest steps, smallest distance
	- `max` - largest possible, best score, maximum subarray sum
	- `addition` - Counting number of ways, counting subsequences/partitions
	- Whatever else - usually will be modifications of the existing ones (Knapsack, LCS, Fib, Rod Cutting)

- If the problem says _maximize_ → use **max()**.
- If the problem says _minimize_ → use **min()**.
- If it says _count all ways_ → use **sum()**.
- If it asks _is it possible_ → use **OR/AND** logic.

Think about the logic:
- What decision is being made at each step?
- How many small subproblems combine to form bigger ones?
- What describes progress?

Lay out a simple example and use it to test your ideas. 

Example from the practice midterm question: 

- Lay out base cases:
	- We start at our base case, when $i=2$:
		- index $i$ will hold element `A[1] + A[2]` since that's the only even subarray possible
	- We're looking for a maximum even subarray, so we use `max`
  
- At each following step $i$:
	- We compare our smallest even subarray that ends at $i$ with our previous results, stored in the DP array
	  
	- We want to have it so that each element of the DP array holds the sum of the max even subarray ending a that index
		- Starting small with $i=3$, since its an odd index there's only one we can choose. We put that into our DP array.
		  
		- Next with $i=4$, we have two possible subarrays. 
		  
		- The DP array now contains values in indexes 2 and 3. Our smallest subarray sum is just `A[i] + A[i-1]`. 
		  
		- There's only one other possible subarray, and that is our last 4 elements in $A$. 
			- We already got the sum for the back half of the subarray and stored it in our DP array, so we can just compare our two subarrays:
			  
			- `MAX(A[i] + A[i-1], A[i] + A[i-1] + DP[2])`
			  
			- Ok, we're getting somewhere, we can generalise it for $i$ , but let's try $i=5$ first
			  
		- For $i=5$ its the same, with two possible subarrays. DP contains up to element 4, and our smallest subarray sum is always going to be `A[i] + A[i-1]`.  
		  
		- The other possible subarray is our last 4 elements in $A$ again.
			- We, again, have the sum for the back half of this subarray in our DP array:
			- `MAX(A[i] + A[i-1], A[i] + A[i-1] + DP[3])`

Ok, we've got some examples, now we can generalise. 

It seems like we always add `DP[i-2]` to get the sum of the largest subarray that forms an even subarray with our new index. 

`MAX(A[i] + A[i-1], A[i] + A[i-1] + DP[i-2])`

We're able to keep using this since we're updating DP with whatever the largest subarray sum for that index is. 

Logic follows that if its the largest, it would either be used in the next index's largest, or be smaller than the sum for that index. You can prove this by contradiction. 


Now that we have the formula and an array of largest values, we just need the max value of the array to get the max even subarray sum of $A$. 