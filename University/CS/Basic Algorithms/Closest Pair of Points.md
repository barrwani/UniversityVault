# Closest Pair of Points
#cs #math


## One Dimension 

**Problem:** Given $n$ numbers $p, p_2, ..., p_n \in \mathbb{Z}$, find $i,j, i \neq j$ such that:
$$\text{dist}(p_i, p_j) = |p_i - p_j|$$
is as small as possible.

**Naive Algorithm:** Try all pairs $i < j$ and output closest pair. Runtime $\left( \frac n2\right) = \Theta(n^2)$

**Better algorithm:** Sort numbers, then check for closest pair among **consecutive** numbers

**Correctness:** Closest pair must be consecutive. 

Using Merge sort:
**Runtime:** $\Theta(n \log n) + \Theta(n) = \Theta(n \log n)$


## Two dimensions

**Problem:** Given $n$ points $p, p_2, ..., p_n \in \mathbb{Z}^2$, find a pair of points that are closest in Euclidean Distance: 
$$\text{dist}(p,q) = \sqrt{(p_x - q_x)^2 +(p_y - q_y)^2}$$

**Naive:** Try all pairs, runtime again $\Theta(n^2)$ (same as 1-dim)

**Second Naive:** Recursively solve left half, then right half, then check all "crossing" pairs (from left to right).

Still takes $\Theta(n^2)$. 

`CLOSESTPAIR`

1. **Divide** points into two halves using vertical line $L$ (based on $x$-coordinate)
   
2. **Conquer**: recursively find closest pair in left half and right half.  
	1. Let $d_1$ and $d_2$ be the distances found and define $d = \text{min}(d_1, d_2)$
	2. Using `MEDIAN`, time complexity is $\Theta(n)$
	   
3. **Combine**
	1. **Intuition:** 
		1. only care about points in a strip of width $2d$ around $L$
		2. Because all points on left and right are $d$-separated, the problem inside the strip is basically one dimensional.
	2. Take all points in a strip of width $2d$ around $L$. 
	3. Sort by $y$-coordinate
	4. Let $q_1,...,q_k$ be sorted points
		1. For each point $q_i$ compute distance to next 11 points $q_{i+1},...q_{i+11}$
	5. Let $d_{1,2}$ be shortest distance found. 
	   
4. Output $\text{min}(d, d_{1,2})$ 

![[Pasted image 20251020164903.png|400]]


**Correctness:** follows from:

**Claim:** If $j > i+11, \text{dist}(q_i,q_j) \geq d$ 

**Proof:** Partition the strip into squares of side length $\frac d2$. 

Each square contains at most 1 point (since diameter is $\frac d {\sqrt 2} < d$).

Moreover, points that are 3 or more rows below $q_i$ must be at distance $\geq d$ from $q_i$, so we can ignore them.

This leaves us with at most 11 points that appear after $q_i$ in sorted order. 

![[Pasted image 20251020164009.png|400]]

**Runtime:** $c \cdot n \cdot \log n$ is the cost of sorting points in strip, so our time complexity is:

$$T(n) = 2 \cdot T(\frac n2) + c \cdot n \cdot \log n$$
$$= c \cdot n \cdot \log n +  c \cdot n\log \frac n2 + c \cdot n\log \frac n4 + ... + c \cdot n\log Z$$
$$ = \Theta(n \log^2 n)$$
where $Z$ is smallest subproblem size (1). 


**Remark:** Being a bit more careful, you can just sort based on $y$-coordinate once and for all.

Pre-sorting the set into two sets, one sorted by $x$ and the other by $y$, costs $\Theta(n\log n)$ once. 

Then, the runtime is $$T(n) = 2T(\frac n2) + c \cdot n$$
$$ = \Theta(n\log n)$$


The **difference** between $\Theta(n\log^2 n)$ and $Θ(n\log ⁡n)$ is whether you re-sort the strip at each combine (bad) or preserve y-order and partition it in linear time (good). 



