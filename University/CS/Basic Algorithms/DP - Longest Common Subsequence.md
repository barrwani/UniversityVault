# Longest Common Subsequence
#cs #math 


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

**Part 1:** 