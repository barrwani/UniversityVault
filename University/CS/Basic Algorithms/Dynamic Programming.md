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










