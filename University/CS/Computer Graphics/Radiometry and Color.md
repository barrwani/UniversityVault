# Radiometry and Color
#cs 

## Radiometry 

- **Definition**: The measurement of light using photons.

### Power

- Total energy per second for light source (Watts, joules per second)


### Radiosity

- The application of the finite element method to solve rendering equations for scenes with surfaces that diffuse light
	- Global illumination algorithm
- Raytracing is an image space algorithm (per pixel), radiosity is object space
- Raytracing cannot simulate interreflection of light between diffuse surfaces, but good for specular reflections and transparent objects
	- Neither are good for real time
- Radiosity great for natural light scenes

## Color

### Human Visual System

- **Mechanism**:
  - Vision relies on cones and rods, which are light-sensitive cells.
  - Signals from these cells are interpreted by the brain.
- **Cones**:
  - Responsible for color sensitivity.
  - Three types for different color ranges: Red (R), Green (G), and Blue (B).
- **Rods**:
  - Responsible for brightness sensitivity.