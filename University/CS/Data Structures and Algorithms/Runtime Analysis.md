# Runtime Analysis
#cs 

We define an abstract measure for analyzing the runtime of an algorithm:

- Analyze the runtime as a function of the **size of the input** -> find T(n)
  
- Use the 'random-access machine' (RAM) model of computation:
	- Any number occupies **constant storage** and can be read and/or written in constant time
	- Each **primitive operation** (+ . - , $\times$ , $\div$ , = , $\sqrt{}$ , etc. ) takes constant time

- Make **asymptomatic anaylsis** -> find the order of growth of T(n)


## Informal Criteria

Find the **asymptotic order** of the **number of primitive operations** executed by an algorithm, **as a function of its input size**

To get the order of growth:
$$T(n) = 3n^2 + 6n -15 = \theta (n^2)$$

Drop the Low-Order Terms:
$$T(n) = 3n^2 = \theta (n^2)$$
Ignore Leading Constants:
$$T(n) = n^2 = \theta (n^2)$$

## Formal Criteria

### Big o definition

Let $f(n)$ and $g(n)$ be two functions mapping positive integers to positive real numbers. 

We say that $f(n) = O(g(n))$ if there exists positive real constant $c$ and a positive integers constant $n_0$ such that $f(n) \le c \cdot g(n)$ for all $n \ge n_0$ 

Finds the upper bound of $f(n)$ in the form of  $c \cdot g(n)$ .

$n_0$ is when $f(n)$ begins lower bounding  $c \cdot g(n)$, typically 1.

![[University/PastedImages/Pasted image 20220926113837.png]]

### Big $\Omega$ Definition

Let $f(n)$ and $g(n)$ be two functions mapping positive integers to positive real numbers. 

We say that $f(n) = \Omega(g(n))$ if there exists positive real constant $c$ and a positive integers constant $n_0$ such that $f(n) \ge c \cdot g(n)$ for all $n \ge n_0$ 

Finds the lower bound of $f(n)$ in the form of  $c \cdot g(n)$ .

$n_0$ is when $f(n)$ begins upper bounding  $c \cdot g(n)$, typically 1.


![[University/PastedImages/Pasted image 20220926113819.png]]



### Big $\Theta$ Definition
Let $f(n)$ and $g(n)$ be two functions mapping positive integers to positive real numbers. 

We say that $f(n) = \Theta(g(n))$ if there exists real positive constants $c_1$ , $c_2$  and a positive integer constant $n_0$ such that $c_2 \cdot g(n) \le f(n) \le c_1 \cdot g(n)$ for all  $n \ge n_0$ 

Finds the upper and lower bounds of  $f(n)$ in the form of  $c_1 \cdot g(n)$ and $c_2 \cdot g(n)$ respectively.

$n_0$ is when both $c_1 \cdot g(n)$ and $c_2 \cdot g(n)$ begin upper and lower bounding $f(n)$, typically 1.

![[University/PastedImages/Pasted image 20220926113744.png]]

# Important Sums

### Arithmetic Series

$1+2+3+4+...+n = \frac{n(n+1)}2 = \Theta (n^2)$ 

$1+2+3+4+...+\sqrt n = \Theta (n)$

$1+2+3+4+...+\log(n) = \Theta (\log^2(n))$ 


### Geometric Series 

$1+ 2 + 4 + 8 + 16 +... + 2^n = 2^{n+1} - 1 = \Theta(2^n)$ [n terms]

$1+ 2 + 4 + 8 + 16 +... + n = 2n-1= \Theta(n)$ [logn terms]

# Amortized Analysis

Amortized analysis takes the cost of a given operation over an extended period of time.

To calculate the Amortized cost of an operation/algorithm, you take the worst case running time 

When given a sequence of operations with a known pattern, amortized cost analysis can be performed.

$$T_{amortized} (n)= \frac{\text{Total cost of the entire series of n operations}}{n} $$

Amortization is used for algorithms where an **occasional operation** is very **slow**, but **most** other operations **are faster**.

It is **not** the same as the average case. Average case is used for a **single** operation, amortized is used for a **series** of operations.
