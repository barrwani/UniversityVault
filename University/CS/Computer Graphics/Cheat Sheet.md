

>Graphics Pipeline:
>- Feed pipeline with geometric objects -> Operate vertices in vertex processing stage -> Send created primitives to rasterization stage
>- Break each primitive into fragments by rasterizer
>- Process fragments in fragment processing stage
>- Combine fragments for each pixel in fragment blending stage
>Cohen-Sutherland Clipping
> - Extend window size to infinity and break up space into 9 regions
> - Each region assigned 4 bit long binary outcode $b_0,b_1,b_2,b_3$
> ![[Pasted image 20241219005011.png|200]]
> - Test for trivially accepted (2 endpoints $\in$ rectangle)
> - Test for trivially rejected (2 points in same half-square of clipping edge)
> - If cannot be trivially accepted/rejected subdivide into 2 segments at clip-edge so one can be trivially rejected 
> 
> - For a line with outcodes $O_1,O_2$:
> 	- $O_1 = O_2 = 0 \implies$ line is fully in window
> 	- $O_1 <> 0, O_2 = 0$ or vice versa = one is inside window, one is outside
> 	- $O_1 \text{ \& } O_2 <> 0 =$ both outside
> - Subdividing the line:
> 	- Done by a crossing edge
> 	- Find endpoint edge that lies outside
> 	- Test the outcode to find the edge that is crossed based on the order: 
> 	- top->bottom, left->right
> - In 3D, subdivide window into 27 regions and assign 6 bit outcode
>Ray tracing: Image-order rendering
>- Compute ray from viewpoint through pixel center
>- Find first object hit by ray
>- Determine intersection point
>- Calculate pixel shading 
>- Higher quality, greater computational cost
>
>Object Order rendering: for each object find all pixels it influences
> Image Order rendering: For each pixel find all objects that influence it
> 
> Step-by-Step
> 1. Ray Generation: origin and direction of each pixels viewing ray computed based on camera geometry
> 2. Ray intersection: Find closest object intersecting viewing ray and its surface normal $n$
> 3. Shading: Compute pixel colour based on results of ray intersection, normal, and light
> 
> Viewing rays: represented by 3D parametric line
> - 3D parametric line from eye $e$ to point $s$: $p(t) = e+ t(s-e)$
> - Useful facts:
> 	- $p(0) = e$
> 	- $p(1) = s$
> 	- If $0 < t_1<t_2$ then:
> 		- $p(t_1)$ closer to eye than $p(t_2)$
> 	- If $t<0$ $p(t)$ is behind eye
>Ray Sphere Intersection:
> - Ray: $p(t) = e + td$
> - Find points that satisfy $f(p(t)) = f(e+td) = 0$
> - $f(e+td) = (e+td-c)(e+td-c)-R^2$
> 
> Ray Triangle Intersection:
> - Barycentric coordinates $p(t_1,t_2,t_3) = t_1A_1 + t_2A_2+t_3A_3$
>Shadow Maps
>1. Render scene from light's POV
>2. Store depth map (as texture)
>3.  Improve performance by disabling texturing/lighting/colour updates
>4. Draw scene from camera POV
>	- Find object coordinates seen from light
>	- Compare coordinate with depth map
>	- Draw object either as in shadow or in light
>Shadow Maps:
>1. Render Scene and initialise depth buffer
>2. Stencil Approach
>	- Render shadow volume twice using face culling
>		- render front faces and increment stencil when depth test passes
>		- render back faces and decrement stencil when depth test passes
>	- If stencil = 0 then pixel is illuminated
>	- else pixel in shadow
>Bezier Curves:
>$$p(t) = \sum_{i=0}^nc_iB_i^n(t)$$
>Control Points: $c_i \in \mathbb{R}^k$
>Basis Functions: $B_i^n(t) \in \Pi^n$ 
>
>Properties:
> - Smoothness
> - Control points are coefficients
> - Predictable behaviour
> - Affine invariance
> - Linear precision
> - Local Control
>Bounding Volumes:
> - Sphere (most memory efficient)
> - Axis Aligned Bounding Box AABB
> - Oriented Bounding Box OBB
> - Convex Hull (fastest)
>Sphere BV: No overlap if $(c_1-c_2)^2 > (r_1-r_2)^2$
>AABB: No overlap if $(c_1-c_2) \cdot x > r1_x + r2_x$ (same for y)
>OBB: No overlap if $T \cdot L > pA+ pB$
>- $pA = a_1A_1L + a_2A_2L$
>- $pB = b_1B_1L + b_2B_2L$
> Subdivision approaches:
> - Uniform Grid: subdivide into uniform cells
> - Octree: subdivide cells based on density of objects. Larger cells = more objects 
> - KD Tree: $k$ dimension (2 or 3) axis aligned hyper planes a snodes
> - Binary space partitioning tree: subdivide space using arbitrary oriented planes into convex cells
>Explicit vs Implicit Integration
>- One unknown:
>	- $x_{t+h} = x_t + hv_t$
>	- $v_{t+h} = v_t + h \frac 1 m F_t$
>- Many unknowns:
>	- $x_{t+h} = x_t + hv_{t+h}$
>	- $v_{t+h} = v_t + h \frac 1 m F_{t+h}$
>
>Euler-Cromer integration:
>- Compute forces
>- Compute vel with old vel
>- Compute pos with old pos and new vel
>Marching Cubes: Voxelization of the domain
>- Scalar vertex values used to polygonise voxel
>- $2^8$ different  configurations -> reduced to 15 by symmetry
>- Polygons represent part of isosurface
>- March in whole space cell by cell
>- Challenges:
>	- Parameter setup time consuming and error prone
>	- Obtaining detailed, smooth, and artifact free surfaces
>Quaternions
>- Extension of complex numbers
>- $q = w + xi + yj + zk, w,x,y,z \in \mathbb{R}$
>- $i,j,k \in \mathbb{Q}$
>- $i^2 = j^2 = k^2 = ijk = -1$
>- Norm $|a|^2 = w^2 + x ^2 + y ^2 + z^2$
>	- Conjugate $\overline q = w - xi-yj-zk$
>	- Inverse: $q^{-1} \frac{\overline a}{|a|^2}$
>
>Rotations:
>- axis $[u_x,u_y,u_z]$
>- Angle $\theta$
>- $$q  = cos(\frac \theta 2) + sin(\frac \theta 2)u_xi = sin(\frac \theta 2)u_yj + sin(\frac \theta 2)u_2k$$
>
>Gimbal Lock: 3D system aligns and can only move in 2D
>