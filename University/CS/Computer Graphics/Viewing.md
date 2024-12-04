# Viewing
#cs 

## Graphics Pipeline and Viewing


- Graphics hardware employs a sequence of coordinate systems which is called a pipeline
	- The location of the geometry is expressed in each coordinate system in turn, and modified along the way


### Coordinate Space

- Each individual object is defined in its **local** coordinate system easily
- Everything in the world is transformed into one coordinate system, the **world coordinate**
	- Has origin, and three coordinate directions $x,y,z$
	- Lighting defined in this space
	- Camera defined with respect to this space


### View Space

- Define coordinate system based on the eye and image plane (camera)
	- eye is center of projection, like aperture on camera
	- Image plane is the orientation of the plane on which the image should appear, like the film plane of a camera
- Some camera parameters are easiest to define in this space
	- Focal length, image size


### Canonical View Volume

- A cube, with the origin at the center, the viewer looking down –z, x to the right, and y up
	- Only things inside the canonical volume can appear in the window
- Parallel sides and unit dimensions make many operations easier
	- Clipping - decide whats in the window
	- Rasterization - Decide which pixels are covered
	- Hidden surface removal - decide what is in front
	- Shading - decide what color things are


### Window Space

- Origin in one corner of the "window" on the screen, x and y match screen x and y
- Window appears somewhere on the screen
	- Typically you want the thing you are drawing to appear in your window
	- May have no control over where the window appears
- You target window space, windowing system takes care of putting it on screen


Problem: Transform Canonical View Volume into Window Space (real screen coordinates)
- Drop the depth coordinate and translate
- Graphics hardware and windowing system typically takes care of this
- Windowing system adds one final transformation to get your window on the screen in the right place



Typically, windows are specified by corner, width, and height
- Corner expressed in terms of screen location
- Converted to ($x_{min},y_{min}$) and ($x_{max},y_{max}$)
Map points in canonical view space into the window
- Canonical view space goes from $(-1,-1,-1)$ to $(1,1,1)$
- Leave z unchanged

## View Volumes

- Only stuff inside CVV gets drawn
	- Window is finite size, finite number of pixels
	- We can only store discrete, finite range of depths
	- Points too close or too far wont be drawn
	- Inconvenient to model the world as a unit box
- A view volume is the region of space we wish to transform into the CVV for drawing. 
	- Only stuff inside the view volume gets drawn
	- Describing view volume is major part of defining the view


## Cameras

View space is the camera's local coordinates
- Camera is in some location
- Looking in some direction
- tilted in some orientation

Inconvenient to model everything in terms of view space

We specify the camera, and hence view space, with respect to world space

To specify a view we need a
- Location with respect to world space
	- Point in World Space for origin of view space -> vector
- Direction in which we are looking(gaze direction)
	- Specified as a vector -> will be normal to the image plane
- Direction that we want to by up in the image
	- Vector -> does not have to be perpendicular to gaze
- We need the size of the view volume


## View Space

- Origin is at the eye
- Normal vector is the normalized viewing direction $n$
- We know which way up should be, and we know we have a right handed system, so $u = up \times n$, normalized
- We have two vectors in a right handed system, so for third we do $v = n \times u$


Left v Right handed
- Just two ways of defining the view space
- Right handed You can define $u$ as right, $v$ as up, and $n$ towards viewer for a right handed system
- Left handed You can also define $u$ as right, $v$ as up, and $n$ as into the scene