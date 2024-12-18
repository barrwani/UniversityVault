# Collision Detection
#cs 

## Motivation

Collision detection for:
- Handling interpenetrating objects

What to solve:
- Different surface types and representations:
	- polygonal, non-polygonal, convex, concave, closed, open, etc.


Why needed?
- For creating physically realistic simulation
## Basic

Below is a naive collision detection algorithm:

```
for each polygon p
	for each polygon q
		if(p!=q)
			for each edge e in p
				for each edge f in q
					if e intersects f
						collision detected
```

This is the simplest form of collision detection. The problem is it checks every primitive one by one, which is highly inefficient. 


## Bounding Volume Hierarchies 

- Encapsulate objects
- Accelerates the collision detection

![[Pasted image 20241215171031.png]]


### Types of BVs:

- Sphere
- Axis-Aligned Bounding Box (AABB)
- Oriented (OBB)
- Convex-Hull
- etc..

### Why and How to Generate

- Why?
	- Efficient Intersection testing
- How?
	- Tight Fitting
	- In a way that's possible to update efficiently
	- Memory-efficient
		- ![[Pasted image 20241215175920.png|400]]

#### Sphere BV 

![[Pasted image 20241215172739.png]]

No overlap if: $$(c_1-c_2)^2 > (r_1+r_2)^2$$
#### AABB

![[Pasted image 20241215172813.png]]

No overlap if: 
- $(c_1-c_2) \cdot x > r1_x + r2_x$ OR
- $(c_1-c_2) \cdot y > r1_y + r2_y$




### Cost Function:

$T=NvCv+NpCp+NuCu$

- $T$: total cost
- $N_v$​: number of BV pair overlap tests
- $C_v$​: cost of BV test
- $N_p$​: number of primitive pair tests
- $C_p$​: cost of a primitive pair test
- $N_u$​: number of updated BVs due to motion
- $C_u$​: cost of updating a BV


## Bounding Volume Hierarchies (BVH)

- Subdivide objects to builds multiple BVs
- Easy overlap rejection for parts of an object



![[Pasted image 20241215194816.png|400]]


### BV Tree

![[Pasted image 20241216170422.png]]

#### BV Tree Overlap Test

- Check BV levels starting with root nodes
- If there is no overlap, test child by child
- At leaf level, check primitive by primitive

We can check using the edge-triangle test:

$i$ = intersection point
$a,b$ = end points of edge
$i = a + \lambda (b-a)$
- $0 \leq \lambda \leq 1$
$i = p_0 + \mu_1(t_1-t_0) + \mu_2$
- $\mu_1,\mu_2 \geq 0$, $\mu_1 + \mu_2 \leq 1$

$r = b-a$
$x =a-t_0$
$d_1= t_1-t_0$
$d_2 = t_2-t_0$
$\begin{pmatrix}\lambda \\ \mu_1 \\ \mu_2 \end{pmatrix} = \frac{1}{-r \cdot(d_1 \times d_2)} \begin{pmatrix} x \cdot (d_1 \times d_2) \\ d_2 \cdot (x \times r) \\ -d_1 \cdot (x \times r) \end{pmatrix}$ 

![[Pasted image 20241216212745.png]]



### Generating and Updating BVs

### Sphere

- Start with AABB of all points
	- Center of the sphere is given by the center of the AABB
	- Radius of the sphere is given by the largest distance from the center to a point

### AABB

- Compute six extremal points for the center and the radii
- AABBs can be translated with an object
- AABBs cannot be rotated with an object 
	- Overlap test does not work for arbitrarily oriented AABBs
- Rotation issue addressed by:
	- Computing AABB for bounding sphere
	- Update AABB only considering then ew positions and original extremal points
	- Hill climbing on convex objects or pre-computed convex hulls of concave objects




### BVH Construction

- Our aim is:
	- To create a balanced tree 
	- Tight fitting 
	- Minimal redundancy
- Depends on 
	- BV Type
	- Top-down / bottom-up
	- How many primitives per leaf

### Deformable Objects

Which BV should be preferred for deformable objects? Why

- BV has to be updated in each time step
- Hierarchy generation significantly influences performance
- AABBs commonly used
- AABBs can be updated efficiently compared to OBB or sphere
- Problem: AABB do not optimally approximate objects


## Spatial Partitioning


BVH is model partitioning. 

Space partitioning works by subdividing the space into cells.

These cells keep references to the primitives that intersect itself.
- Pairwise object tests that reside in the same region of space

Efficient for testing the collision of a large number of objects.

![[Pasted image 20241216220407.png|400]]



### Uniform Grid

- Subdivide space into cells
- Place primitives into cells
- Check primitives in the same cell
- Fast rejection for primitives that do not share same cell

### Hashing

- Use hash tables
- Space is unbounded
- Hash function calculates hash table index
- Steps:
	- Hash vertices with respect to cells
	- Hash tetrahedrons/triangles with respect to cells touched by their bounding box
	- Test vertices and triangles/tetras in the same cell for collision

#### Vertex in Triangle Test

For all 3 edges check:
$$(x-v_0) \cdot ((v_2 - v_0) \times n) > 0$$
With surface normal $n$ and given vertex $x$

![[Pasted image 20241216222728.png]]

Using Barycentric Coordinates:

$x' = x - v_0$
$e_1 = v_1 - v_0$
$e_2 = v_2 - v_0$
$\alpha = \frac{(x' \times e_2)}{(e_1 \times e_2)}$
$\beta = \frac{(x' \times e_1)}{(e_1 \times e_2)}$


If $\alpha < 0, \beta < 0$ or $\alpha + \beta > 1$, no collision occured.


For Tetrahedrons:

- Store all vertices in hash table
- Compute hash table indices for tetrahedron bounding boxes
- Check for intersection of respective vertices in the hash cell

Crucial Parameters:
- Grid cell size (voxel size)
- Hash table size
- Hash function

Voxel Size:
- Affects number of primitives mapped to the same hash index
- Larger size = Larger number of primitives, larger number of tests
- If voxel size is smaller than tetra size, tetra covers many cells and has to be check with many vertices
- OPTIMAL: Voxel size = average edge length of tetrahedrons \[3]
- Most important factor is overall performance


Hash table size:
- Hash collisions reduce performance
- Larger hash tables prevent this
- Not too large for memory management purposes
- Use prime number as table size

### Octree

- Partition space into rectangular, axis-aligned cells
- Hierarchical 
- AABB of the object is the root node of the octree
- Large cells for low density of primitives
- Small cells for high density 
- Cells with small number of primitives can be merged
- Cells with a large number of primitives can be subdivided

### KD-Tree

- K: dimension (2 or 3)
- Axis-aligned hyper planes as nodes
- Point in front of the plane is on the left
- Point in behind of the plane is on the right


### Binary Space Partitioning Tree (BSP)

- Subdivide space using arbitrary oriented planes into convex cells
- Hierarchical structure
- No axis-alignment restriction
- Generally used for ray-casting and rendering


## Collision Response

- First detect collision, then resolve it
- Physics based:
	-  Change velocity of objects
	- Apply repulsion forced based on penetration distance
- Non-physics based
	- Place objects back to point of collision