# Karatsuba
#cs #math 

## The Problem

**Naive Algorithm:** Input two $n$ digit numbers.

Example: $256 \times 116$. 
$$ = 1518+ 2530 + 25300 = 29348$$
$n^2$ digits, so runtime of $\Theta(n^2)$. 


**Terrible Algorithm:** $a \times b = a + a + a + .... + a$,  $b$ times. 

Runtime is $O(b) = O(10^n)$, exponential in terms of input size (# digits). 


## Mini Karatsuba

Use Divide and Conquer. 

**Example:** $a = 1733, b = 2516$

$$a \times b = 1733 \times 2516 = (17 \times 100 + 33) \times (25 \times 100 + 16)$$
$$ = 17 \times 25 \times 10,000 + (17 \times 16 + 33 \times 25) \times 100 + (33 \times 16)$$

$$a_h = 17, a_l = 33$$
$$b_h = 25, b_l = 16$$

Generally, we split $n$ digits of $a$ and $b$ into two halves.

$$a = a_h \cdot 10^{\frac n2} + a_l$$
$$b = b_h \cdot 10^{\frac n2} + b_l$$

Then, $$a \cdot b = (a_h \cdot b_h \cdot 10^n) + (a_h \cdot b_l + a_l \cdot b_h) \cdot 10^{\frac n2} + (a_l \cdot b_l)$$

```MULT(a,b)
	1. recursively compute:
		ab1 = MULT(a_h, b_h)
		ab2 = MULT(a_h, b_l)
		ab3 = MULT(a_l, b_h)
		ab4 = MULT(a_l,b_l)
	2. OUTPUT:
		(ab1 * 10^n) + ((ab2 + ab3) * 10^(n/2)) + (ab4)
```

**Runtime:** 
$$T(n) =  4 \cdot T(\frac n2) + c \cdot n$$
$$ = c \cdot n + 4 \left(c \cdot \frac n2\right) + 16 \left(c \cdot \frac n4\right) + 64\left(c \cdot \frac{n}{8}\right) +...$$
Log base for depth is $\frac 1 {\frac 12} = 2$, and we can expand and simplify such that $$T(n) = cn + 2cn + 4cn + .... + 2^{\log_2 n}cn$$
$$ = \Theta(n^2)$$

## Karatsuba

```
MULT(a,b)
	1. COMPUTE
		H = MULT(a_h,b_h)
		M = MULT(a_h + a_l, b_h + b_l) 
		L = MULT(a_l, b_l)
	2. OUTPUT
		H * 10^n + (M-L-H) * 10^(n/2) + L // = a*b
```

**Correctness:** expand output


**Runtime:**
$$T(n) = 3 \cdot T(\frac n2) = c\cdot n$$
$$ = c \cdot n + 3(c \cdot \frac n2) + 9(c \cdot \frac n4) + ... $$
$$ = cn + 3(c \cdot \frac n2) + 9(c \cdot \frac n4) +... + (\frac{3}{2})^{\log_2 n}$$
$$ = \Theta((\frac{3}{2})^{\log_2 n} \cdot n)$$
$$ = \Theta(2^{(\log_2 \frac32) \cdot (\log_2 n)} \cdot n)$$
$$ = \Theta(n^{\log_2 \frac 32} \cdot n)$$
$$= \Theta(n^{1 + \log_2 \frac 32}) = \Theta(n^{1.585...})$$

