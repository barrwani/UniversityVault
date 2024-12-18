# Math
#cs #math 

## Vectors

- Dot product
- Projections
- Orthonormal
- Orthogonal
- Determinants
- Matrices
- Eigenvalues
- Sets/Mapping

Important:

Constructing a proper basis from two vectors:
- Start with two Vectors A and B
- Ensure they are orthonormal
- Take cross product of those vector
- This third vector will be perpendicular to both
	- Normalize


## Curves and Surfaces

Curves are described by an implicit equation $f(x,y) = 0$.

Points $(x,y)$ where $f$ is zero on the curve


Triangles are the fundamental modelling primitive. Information is tagged on vertices and interpolated across the triangle.

### Barycentric Coordinates

The centre of mass of 3 masses. 

$p(t_1,t_2,t_3) = t_1A_1 + t_2A_2 + t_3A_3$
$t_1+t_2+t_3 = 1$
![[Pasted image 20240917103417.png]]

- A point $p$ is inside the triangle formed by $A_1,A_2,A_3$ if and only if $0 < t_{1,2,3} < 1$
- If one of the coordinates is zero and the other two are between zero and one, then you are on an edge
- If two of the coordinates are zero, then the other is one, and you are at a vertex
- Fast rejection while coding


## Linear Algebra


Matrices
- Array of numeric elements that follow certain arithmetic rules 
- Important for spatial transformations (scale, rotation, translation)
- Multiply rows of the first matrix with the column of the second matrix 
- Matrix multiplication is not commutative

Determinants
- Given 2D vectors $a,b$
- Determinant $|ab|$ is the area of the parallelogram 
	- $|ab| = -|ba|$
	  
![[Pasted image 20241023125455.png|200]]

- Given 3D vectors $a,b,c$
- Determinant $|abc|$ is the volume of parallelepiped 

![[Pasted image 20241023125502.png|200]]

Determinant Rules:

- $|k(a)b| = |a(kb)| =k|ab|$
	- Scaling one of the sides scales are in 2D
- $|(a+kb)b| = |a(b+ka)| = |ab|$
	- Shearing does not affect area

### Matrix Operations

- Inverse of $x$ is $x^{-1} =\frac 1 x \rightarrow x^{-1}x = 1$ for a real number
- We have the Identity Matrix to represent 1 as a matrix
- $I = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 &1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$
- For matrix $A: AA^{-1} = I$
- $(AB)^{-1} = B^{-1}A^{-1}$



