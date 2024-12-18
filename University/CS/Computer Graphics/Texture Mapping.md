# Texture Mapping
#cs 


Texture mapping is a way to add detail, surface texture, or color to a graphic or model

It allows us to add realism without raising geometric complexity

Normal mapping allows us to edit normals without altering complexity too much

## Texture Arrays

- Texture mapping is a 2D array in 2D space, with dimensions in $u,v$
- We need to create a reflectance function $R(u,v)$
	- ?
- Then take an image, assign $u,v$ coordinates, and map onto a 3D object


## Unwrapping

- Making a 3D object 2D by unwrapping surfaces into a flat plane
	- Unwrap object into 2D then paint the triangles
- Unwrapped plane can then be painted


## Texture Mapping on Rasterized Triangles

- Store $u,v$ texture coords at each vertex of mesh
- Barycentric Coordinates


## Bump Texture

- Changing surface normal: Gives illusion of displacement
- Geometry need not be complex to have complex bump patterns
- Imagine a map that alters surface normals by certain amount

## Displacement Mapping

- Bump map cannot cast shadows or effect silhouette. 
	- No true change in geometry
- Displacement map changes geometry using a texture map