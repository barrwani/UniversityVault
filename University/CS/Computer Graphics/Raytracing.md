# Raytracing
#cs 

Raytracing is a technique for generating an image by tracing the path of light through pixels in an image plane and simulating the effects of its encounters with virtual objects.



| Projective                                          | Raytracing                                      |
| --------------------------------------------------- | ----------------------------------------------- |
| Project 3D triangles to 2D triangles on screen      | Compute ray from viewpoint through pixel center |
| Project vertices                                    | Find first object hit by ray                    |
| Shade 2D triangles                                  | Determine intersection point                    |
| OpenGL, DirectX                                     | Calculate pixel shading                         |
| Fast, good for real-time applications               | Produces high degree of realism                 |
| 2 or more triangles can project onto the same pixel |                                                 |
| Object-order rendering                              | Image-order rendering                           |


OOR: For each object, find and update all pixels it influences.

IOR: For each pixel, find all objects that influence it, and update accordingly.


## Rays

- **Representation**: Rays are represented as an origin point and a propagation direction using a 3D parametric line:
  - **Equation**: $p(t) = e + t(s - e)$
    - Line from eye $e$ to point $s$.
- **Ray Generation**:
  - Starts from the camera frame.
  - Represented by:
    - Eye point $e$.
    - Basis vectors $u$, $v$, and $w$.
  - Rays share the same origin with different directions.
  - The image plane is positioned at a distance $d$.
  - **Pixel Offset**: Position of the pixel is offset by 0.5 for calculations to ensure the ray goes through the middle of the pixel, not the corner.

## Orthographic View

- **Key Feature**: No viewpoint.
- All rays have direction $-w$.

![[Pasted image 20241216164546.png|300]]

## Ray-Sphere Intersection and Implicit Surfaces

- **Implicit Forms**: Review the implicit forms for surfaces and intersections.



## Ray-Triangle Intersection

- **Method**: Barycentric coordinate method.
  - **To Review**: How to calculate using this method.


## Ray-Polygon Intersection

- **Polygon Representation**: Polygon with vertices $p1, p2, ..., pm$ and surface normal $n$.
  - **Formula**:  
    $t = ((p1 - e) \cdot n) / (d \cdot n)$  
    - Where $e$ is the ray origin and $d$ is the ray direction.
- **Optimization**: Split the polygon into triangles for better efficiency.




## Shadows

### Shadow Feelers
- **Definition**: Rays sent from point $p$ towards light source $l$.
  - **Condition**:  
    If the ray hits an object, then point $p$ is in shadow.

### Importance of Shadows
- **Why Shadows Matter**:
  - Provide spatial relationships between objects for realistic results.
  - Without shadows, objects appear disconnected.

### Shadow Terminology
- **Components**:
  - **Umbra**: Full shadow.
  - **Penumbra**: Partial shadow.
  - **Antumbra**: Extended shadow.



## Shadow Techniques

### Projection Shadows
- **Description**: Projects the primitive of occluding objects onto the receiving plane (e.g., wall or ground).
- **Limitations**:
  - Restricted to planar receivers.
  - Cannot handle complex cases like self-shadowing (e.g., character placing hand over arm).

### Shadow Maps
- **Purpose**: Add realistic shadows, including curved shadows on curved surfaces.
- **Process**:
  1. Render the scene from the light's point of view.
  2. Store the depth of every surface in a map (texture).
  3. Render the scene from the camera's perspective.
  4. Apply the shadow map.

- **Aliasing Issues**:
  - Caused by the discretized representation of depth values.
  - Solutions:
    - Use high-precision floating points.
    - Apply depth offsets.

### Shadow Volume Method
- **Definition**: The volume between the occluder and the casted shadow.
- **Stencil Buffer Technique**:
  - **Stencil Value**:
    - Increase by 1 when entering a shadow volume.
    - Decrease by 1 when leaving a shadow volume.
  - **Shadow Determination**:  
    Points with a stencil value > 0 are in shadow.  
    Shadow intensity can be adjusted based on the stencil value.
