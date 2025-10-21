# Knapsack & Interval Scheduling
#cs #math 

## Knapsack

**Input:** given a list of $n$ item weights: $\{w_1,...,w_n\}$ and values $\{v_1,...,v_n\}$ and weight limit $W$. 

**Goal:** find subset of $\{1,2,..n\}$ with maximum total value and total weight $\leq W$

**Novice solution:** Try all subsets. Takes $2^n$ time.


![[Pasted image 20251020224203.png]]

**Greedy Approach**: Repeatedly pick item maximising $\frac {v_i}{w_i}$. 
Not optimal. Take above example: $\{1,2,5\}$ with value 35. Optimum is 40, achieved by $\{3,4\}$. 


**Dynamic Programming Solution:**

1. **Subproblems:** for $i = 0,...,n$ and $w = 0,..., W$, define $DP[i,w]$ = maximum value using only items $1,..,i$ and weight limit $W$.
2. **Guess:** use item $i$ or not?
3. **Recurrence:** $DP[i,w]=$
	1. 0 if $i = 0$
	2. $DP[i-1,w]$ if $w_i > w$
	3. $\text{max}(DP[i-1,w], DP[i-1,w-w_i]+v_i)$
4. **Algorithm:**
5. **Runtime:** $\Theta (n \cdot W)$


**Remark:** Not a polynomial time algorithm. Input size is $n \log W$. Running time is exponential. 

Knapsack is an NP-complete problem, poly time is not expected. 


## Internal Scheduling

