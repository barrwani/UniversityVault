# Review
#cs 


## Math

2D Gradient: $\nabla f(x,y) = (\frac {\delta f}{\delta x}, \frac{\delta f}{\delta y})$

Surface normal to an implicit 3D surface: $f(x,y,z) = f(p) = 0$
$n = f(p) =  (\frac {\delta f(p)}{\delta x}, \frac{\delta f(p)}{\delta y}, \frac{\delta f(p)}{\delta z})$

Implicit Plane: $(p-a)-n=0$ with $abc$ on plane $(b-a) \cdot (c-a) = n$

2D Parametric lines: $p(t) = p_0 + t(p_1-p_0)$

Barycentric Coordinates: $p(t_1,t_2,t_3) = t_1A_1 + t_2A_2+t_3A_3 \implies t_1+t_2+t_3 =1$


## Images

Raster display: rectangular array of light emitting pixels

Raster Image: 2D array storing pixel value for each pixel (RGB color value)

Vector Images: Store description of shapes

Raster Displays: TV, Monitor, Projector

Pixels consist of RGB sub-pixels

Frame buffer is a pixmap that allows for colour pictures

Liquid Crystal: Material where molecular structure enables it to rotate the polarization of light that passes through

Demosaicing: process of reconstructing full colour image from incomplete colour samples

Image as a function: $I(x,y) = R \rightarrow V \implies R \subset \mathbb{R}^2, V = {\text{possible pixel values}}$

Gamma: encode and decode luminance values in video or still image systems.
- $\text{Displayed intensity} = (\text{maximum intensity}) \cdot \alpha ^\gamma$ 
- $\alpha:$ input value between 0 and 1
- $\gamma:$ gamma value


Pixel coverage: fraction of pixels covered by foreground layer:
- $C = \propto C_f + (1-\propto)C_b$
- $\propto:$ pixel coverage
- $C_f:$ foreground colour
- $C_b:$ background colour

## Graphics Pipeline


Graphics Pipeline: 
- GPU reads vertex array
- Rasterizer clips and fragments triangles
- Fragment shader outputs colour & depth values for fragments
- Frame buffer contains rendering output

Object Order Rendering:
- Draw objects 1 by 1 onto the screen

>[!NOTE] For Cheat Sheet
>
>Graphics Pipeline:
>- Feed pipeline with geometric objects 
>- Operate vertices in vertex processing stage
>- Send created primitives to rasterization stage
>- Break each primitive into fragments by rasterizer
>- Process fragments in fragment processing stage
>- Combine fragments for each pixel in fragment blending stage

Line-Segment Clipping:
- Line-Rectangle: Brute force approach, requires floating point multiplication

>[!NOTE] For Cheat Sheet
>
>Cohen-Sutherland Clipping
> - Extend window size to infinity and break up space into 9 regions
> - Each region assigned 4 bit long binary outcode $b_0,b_1,b_2,b_3$
> - $$b_0=\begin{cases} 1, & \text{if $y > y_{max}$}.\\ 0, & \text{otherwise}. \end{cases}$$
> - $$b_1=\begin{cases} 1, & \text{if $y < y_{min}$}.\\ 0, & \text{otherwise}. \end{cases}$$
> - $$b_2=\begin{cases} 1, & \text{if $x > x_{max}$}.\\ 0, & \text{otherwise}. \end{cases}$$
> - $$b_3=\begin{cases} 1, & \text{if $x < x_{min}$}.\\ 0, & \text{otherwise}. \end{cases}$$
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

Anti Aliasing:
- Rasterization produces jaggy lines and triangle edges
- Box filtering
	- Set pixel value to the average color of the image over the square area belonging to the pixel using super sampling
- Super Sampling 
	- Creates high-res images
	- EG: Goal 256 x 256 pixel image of line with width 1.2 pixels 
		- Rasterize rectangle version of line with width 4.8 pixels on a 1024 x 1024 screen
		- Average 4x4 groups of pixels to get colours for each 256 x 256 pixel in the shrunken image


Culling: Throwing away invisible geometry to save processing time
- View Volume Culling: remove geometry that is outside of view volume
- Occlusion Culling: remove geometry that is obstructed/obscured
- Backface Culling: remove primitives facing away from camera


## Textures

Texture Mapping: method for adding detail, surface texture, colour to CG model 
- Adds realism without raising geometrical complexity

Store reference of object as:
- Texture Map
	- A function
	- Pixel based image
- Map onto surface

Texture mapping classified by different properties
- Dimensionality of texture function
	- 2D, 3D
- Correspondence defined by 2 points on surface and 2 points in texture function
- Whether texture function is primarily procedural or primarily table look-up


Texture Arrays:
- 2D array in 2D space
- Dimensions to be mapped $u,v$
- Texture image  $n_x \times n_y$
- every $u,v$ must have associated colour from image

3D Texture Mapping:
- Diffuse reflectance at point $C_0$
- If no solid colour
	- Replace $C_0$ with $C_0(P)$
	- Map 3D points to RGB Colour
- Create 3D texture that defines an RGB value at every point in 3D space

How Texturing Works:
- Remove integer position of $u,v$ so that it lines in unit square
- Tile entire $uv$ plane with copies of square texture
- Interpolate and compute image colour for that coordinate
	- bilinear interpolation hermite smoothing

Stripe textures: with 2 colours $c_0, c_1$ find oscillating function eg: $sin()$

Procedural textures:
- no need to store (reduced GPU transfer)
- can be calculated at any point
- infinite resolution (similar to vector graphics)

Bump Textures: changing surface normal to create illusion of displacement
Displacement Map: Change geometry, increases realism done to shadows


## Shaders

Shaders: computer program to calculate colour and light

GPGPU: general purpose computing on a GPU
- Use of GPU to handle computations normally handled by CPU
- parallel processing between many GPUs and CPUs to analyze graphics data

Vertex Shader:
- Position
- Texture Coordinates
- Colour
  
Pixel Shader:
- Colour and Alpha
- Depth

Unified Shader Model: Shader stages in pipeline have access to reading textures and buffer


FFP Vs Shaders:
- FFP: Fixed Function Pipeline
	- Lighting and texturing in hardcoded manner
- Shaders are programable


Pipeline:
- CPU sends instructions and geometry data to GPU
- Geometry transformed in vertex shader
- Geometrical changes copied if geometry shader exists 
- Geometry subdivided if translation shader in system
- Calculated geometry is triangulated
- Triangles broken into fragments: 2x2 primitives (quads)
- Fragments modified in pixel/fragment shader
- 4 fragments pass depth test -> written to screen


## Radiometry

Radiometry: measuring light
- Collection of photons
- Photon: fundamental light particle. Speed $c$, wavelength $\lambda$
	- Unit of wavelength $nm = 10^{-4}m$, angstrom $= 10nm$

Frequency of light $\frac c \lambda$
Amount of energy carried by photon $g = \frac {hc} \lambda$
- $h = 6.63 \times 10^{-34}J$
- Total energy of photon $\sum q_i = Q$
- Spectral energy $Q_\lambda$ clarity function to tell density of energy at infinitesimally small point 


Power: rate of energy production for light source
- Measured in watts: 100W = 100 J/s

Human eye can only see $360-380nm$

Visual effect of light:
- Hue: dominant wavelength
- Saturation: amount of white light
- Brightness: intensity of light

Colour models:
- RGB: Red Green Blue
- CMY: Cyan Magenta Yellow


## Raytracing

Ray tracing: generating an image by tracing the path of light through pixels in an image plane and simulating the effects of its encounter with virtual objects.

Projective methods: Object Order Rendering
- Project 3D-> 2D triangles on screen
- Project vertices
- Shade 2D triangles
- Fast, goal for real-time apps

>[!NOTE] For Cheat Sheet
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
>   
> 


Generate viewing rays from camera frame 
- Eye point (view point) $e$
- Basis vectors $u,v,w$

Perspective View:
- Rays have
	- Same origin
	- Different direction
	- Image plane position at distance $d$

Fitting on image:
- Image with $n_x \times n_y$ pixels 
- Fit rectangle of size $(r-c) \times (f-b)$
- Space pixels:
	- $(r-c)/n_x$ apart horizontally
	- $(f-b)/n_y$ apart vertically
	- Half-pixel offset
- $(u,v)$ pixels position on image plane wrt. origin $e$ and basis $\{u,v\}$ 
- Pixel at position $(i,j)$ in a raster image has the position
	- $u = (c + (r-c)(i + 0.5)) /n_x$
	- $v - (b + (t-b)(j+0.5))/n_y$

Orthographic View:
- Ray direction $-w$
- Parallel view 
- No viewpoint
- Ray start on plane defined at origin of camera


>[!NOTE] For Cheat Sheet
>Ray Sphere Intersection:
> - Ray: $p(t) = e + td$
> - Find points that satisfy $f(p(t)) = f(e+td) = 0$
> - $f(e+td) = (e+td-c)(e+td-c)-R^2$
> 
> Ray Triangle Intersection:
> - Barycentric coordinates $p(t_1,t_2,t_3) = t_1A_1 + t_2A_2+t_3A_3$


Lambertian Shading: $L = k_d \cdot I_\text{max}(0, n \cdot l)$
- $k_d$: diffuse coefficient
- $I$: intensity of light source

 Blin-Phong Shading: $L = k_d \cdot  I_\text{max}(0, n \cdot l) + k_s  I_\text{max}(0, n \cdot h)^p$
 - $k_s$: specular color of surface
 - $h = \frac{v+1}{||v_+1||}$
 

Ambient Shading: Add constant component to shading
- Ambient light comes from everywhere

Shadow:
- More realistic results
- Illustrates spatial relation between objects


Projection Shadows: Project primitives of occluders onto receiving plane based on position of light source
- Restricted to planar receivers 


>[!NOTE] For Cheat Sheet
>Shadow Maps
>1. Render scene from light's POV
>2. Store depth map (as texture)
>3.  Improve performance by disabling texturing/lighting/colour updates
>4. Draw scene from camera POV
>	- Find object coordinates seen from light
>	- Compare coordinate with depth map
>	- Draw object either as in shadow or in light


Aliasing:
- Problem: Discretized representation of depth values
	- Result: Aliasing
- Solution: using Offset

>[!NOTE] For Cheat Sheet
>Shadow Maps:
>1. Render Scene and initialise depth buffer
>2. Stencil Approach
>	- Render shadow volume twice using face culling
>		- render front faces and increment stencil when depth test passes
>		- render back faces and decrement stencil when depth test passes
>	- If stencil = 0 then pixel is illuminated
>	- else pixel in shadow


## Meshes

Mesh: Surface made of polygons glued at common edges

- Vertex based relations
	- VE: For each vertex, list of incident edges arranged in radial CCW around vertex
	- VV: For each vertex list of adjacent vertices arranged in radial CCW around vertex
	- VF: For each vertex, list of adjacent faces arranged in radial CCW around vertex
- Edge Based Relations:
	- EV: For each edge, 2 incident vertices
	- EF: For each edge, 2 incident faces
	- EE: For each edge, 2 points of edge sharing vertex and face with edge
- Face based relations:
	- FE: For each face, list of incident edges in CCW around face
	- FV: For each face, list of incident vertices in CCW around face
	- FF: For each face, list of adjacent faces in CCW around face
- Topological Relations
- Constant relations return constant number of elements
		- EV: each edge has two endpoints
		- EE: each edge has four adjacent edges
		- EF: each edge has two incident faces
- Variable relations return variable number of elements
	- VV,VE,VF,FV,FE,FF: number of v/e/f incident/adjacent to given f/f is not constant and can be some order of total number of v/e/f

Operators for triangle meshes:
- Refinement operators: add more detail by producing more v/e/f
	- Triangle split: insert new v into triangle t and connect v to vertices of t by splitting into 3 triangles
	- Edge split: insert new v on edge and connect to opposite vertices of triangle incident at e by splitting e as well as each such t in two
	- Vertex Split: cut open mesh along two edges $e_1,e_2$ incident at a common vertex $v$ by duplicating such edges as well as $v$
- Simplification Operators: get rid of detail by removing v/e/f
	- Edge Collapse (vertex split reverse): collapse e, remove e and 2 incident triangles
	- Edge Merge (edge split reverse): take internal vertex v with valence 4, delete v with incident t and e, fill hole with 2 t sharing e
	- Vertex Delete (triangle split reverse): remove internal v of valence 3 along with incident t and e
- Neutral operator: does nothing to LOD
	- Edge swap: replace e with opposite diagonal in quadrilateral

## Curves

2D Parametric curves $p(t) = \begin{pmatrix} x(t) \\ y(t) \end{pmatrix}, t \in [t_0,t_1]$
Tangent vector $p(t) =  \begin{pmatrix} r \text{ cos } t \\ r \text{ sin } t \end{pmatrix}$
$p'(t) =  \begin{pmatrix} -r \text{ sin } t \\ r \text{ cos } t \end{pmatrix}$

$||p'(t)|| =$speed

$\frac{p'(t)}{||p'(t)||} = T(t) =$ unit tangent


Arc Length $s(t) = \int_{t_0}^t ||p'(t)|| dt$


>[!NOTE] For Cheat Sheet
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



## Collisions

>[!NOTE] For Cheat Sheet
>Bounding Volumes:
> - Sphere (most memory efficient)
> - Axis Aligned Bounding Box AABB
> - Oriented Bounding Box OBB
> - Convex Hull (fastest)

Allows for intersection test

>[!NOTE] For Cheat Sheet
>Sphere BV: No overlap if $(c_1-c_2)^2 > (r_1-r_2)^2$
>AABB: No overlap if $(c_1-c_2) \cdot x > r1_x + r2_x$ (same for y)
>OBB: No overlap if $T \cdot L > pA+ pB$
>- $pA = a_1A_1L + a_2A_2L$
>- $pB = b_1B_1L + b_2B_2L$


Bounding Volume Hierarchies: Subdivide objects and build multiple BV

Space partitioning:
- Subdivide space into cells
- Cells keep references to intersecting primitives
- Pairwise object tests that lie in some cell

>[!NOTE] For Cheat Sheet
> Subdivision approaches:
> - Uniform Grid: subdivide into uniform cells
> - Octree: subdivide cells based on density of objects. Larger cells = more objects 
> - KD Tree: $k$ dimension (2 or 3) axis aligned hyper planes a snodes
> - Binary space partitioning tree: subdivide space using arbitrary oriented planes into convex cells

Temporal coherence: objects nearby tend to stay nearby


## Animation

- Conventional
	- Interpolate between keyframes
	- Create function curves
	- Non-interactive
- Physically based
	- Physical laws apply
	- real-time and interactive

Simulations should be realistic, high performance, stable
	-Imitate natural phenomenon.


Mass points
- Approximate a physical system using collection of points
- Simulate dynamics of body using points
- Interpoint forces via springs
- Topology preservation

Rigid body: ideal solid body with no deformation allowed


Particle:
- Mass $m \in \mathbb{R}$
- Velocity $v \in \mathbb{R}^3$
- Position $x \in \mathbb{R}^3$
- Applied forces $F \in \mathbb{R}^3$
- Colour, transparency, lifetime


>[!NOTE] For Cheat Sheet
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


Deformable objects:
- Simple mass-spring system
- Topology of mass points must be preserved
- Parameters
	- Mass, velocity, position of mass points
	- Stiffness and initial length of springs


Rigid Body:
- Have springs with infinite stiffness
- position + orientation applies to entire body
- Mass Distributed
- Center of mass: point at geometric center


## SPH

>[!NOTE] For Cheat Sheet
>Marching Cubes: Voxelization of the domain

- Surface reconstruction algorithm for viewing 3D point clouds
- Steps:
	- Divide simulations pace into equidistant grid cells 
		- Divide the AABB of the particle net
		- Compute scalar values of corner vertices 
		- scalar value >< than threshold value

>[!NOTE] For Cheat Sheet
>- Scalar vertex values used to polygonise voxel
>- $2^8$ different  configurations -> reduced to 15 by symmetry
>- Polygons represent part of isosurface
>- March in whole space cell by cell
>- Challenges:
>	- Parameter setup time consuming and error prone
>	- Obtaining detailed, smooth, and artifact free surfaces


Surface reconstruction: 
- Consider narrow band where surface actually lies
- Use parallelization: 50 times faster on GPU


Post processing for high surface quality:
- Step 1: compute scalar field and generate triangulated surface using MC
- Step 2: decimate surface mesh
- Step 3: subdivide surface mesh


Decimation:
- Aim:
	- reduce bumps in flat regions
	- Maintain fidelity to the original mesh

Subdivision:
- Aim
	- Smooth, sharp features
	- Preserve mesh smoothness

Combining both results in smoother mesh, 20% reduction in size



## Math

>[!NOTE] For Cheat Sheet
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


