# GLSL Ray Marching

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 5**

* Bradley Crusco
* Tested on: Google Chrome 46 on Windows 10, i7-3770K @ 3.50GHz 16GB, 2 x GTX 980 4096MB (Personal Computer)

### Live on Shadertoy

[![](img/top.png)](https://www.shadertoy.com/view/MlBSzc)
Click the image above or click [here](https://www.shadertoy.com/view/MlBSzc) to view the shader live on Shadertoy.

## Description

A GLSL ray marching shader with support for naive ray marching, sphere tracing, Lambert lighting, soft shadows, and rendering of spheres, planes, boxes, rounded boxes, and toruses. Also provides rendering for height-mapped terrain, a mandelbulb fractal object, and union, intersection, subtraction, displacement, blending, and transformation operations.

## Features

### Naive Ray Marching

Ray marching with a fixed step size. Refer to the analysis section for more information on this method.

### Sphere Tracing

Ray marching using a variable step size based on a signed distance field. Refer to the analysis section for more information on this method.

### Distance Estimators
Distance estimators are used to define the objects in our scene. Support for the following basic distance estimators is provided:
* Sphere {[McGuire 7.1](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Plane {[McGuire 7.2](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Box {[McGuire 7.3](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Rounded Box {[McGuire 7.4](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Torus {[McGuire 7.5](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}

### Height-Mapped Terrain Distance Estimator

Implemented based on the method outlined by Inigo Quilez [here](http://www.iquilezles.org/www/articles/terrainmarching/terrainmarching.htm). The height-map is defined by a simple function: ```point.y - height * sin(length * point.x) * cos(point.z)```

![](img/terrain.png "Height-Mapped Terrain Distance Estimator")

### Mandelbulb Fractal Distance Estimator

Implemented based on the method outlined [here](https://www.shadertoy.com/view/XsXXWS), this distance estimator renders the mandelbulb fractal object.

![](img/mandlebulb.png "Mandlebulb Fractal Distance Estimator")

### Lambert Lighting Computation
A simple Lambert lighting computation was used for rendering. Below is an example render using the Lambert computation without soft shadows.

#### Lambert Lighting Image (No Soft Shadows)
![](img/lambert.png "Lambert Lighting Image")

### Soft Shadows
Soft shadows are added by completing a second ray march to the light, and scaling the shadow by the ratio of the change in distance and the total distance on each iteration of the march.

#### Lambert Lighting Image (Soft Shadows)
![](img/soft_shadows.png "Lambert Lighting Image with Soft Shadows")

### Distance Operators

Distance estimators are used to define the objects in our scene. Support for the following basic distance estimators is provided:
Distance operators are used to add, remove, and combine basic shapes from the distance estimators into a full, complex scene. Support for the following distance operators is provided:

* Union {[McGuire 11.1](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Intersection {[McGuire 11.2](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Subtraction {[McGuire 11.3](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Displacement {[McGuire 11.5](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Blending {[McGuire 11.6](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}
* Transformation {[McGuire 11.5](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)}

### Debug Views
There are two debug views provided. The first shows the distance to the surface for each pixel. It can be activated by uncommenting the DISTANCE_COLOR definition in the shader. The second shows the number of ray march iterations used for each pixel. It can be activated by uncommenting the STEP_COUNT_COLOR definition in the shader. Below are examples of both images.

#### Distance Debug Image
![](img/distance_color.png "Distance Debug Image")

#### Step Count Debug Image
![](img/step_count_color.png "Step Count Debug Image")

## Analysis

### Naive Marching vs. Sphere Tracing
There are two ways to compare naive ray marching and sphere tracing. The first is to measure the time to render a frame in miliseconds and compare the values for both methods. Unfortunately here, even for the complex mandelbulb example, both the naive and sphere methods give a result of 16ms per frame. The other way to do this is to visually compare the methods by uncommenting the STEP_COUNT flag and comparing the results.

#### Naive Marching Step Count Image
![](img/naive_step.png "Naive Marching Step Count")

#### Sphere Tracing Step Count Image
![](img/sphere_step.png "Sphere Tracing Step Count")

With the STEP_COUNT flag enabled, the redder a pixel in an image, the more steps it took to color it. Since our naive marching image is orange (closer to red than green is) we can see that it took significantly more steps than the sphere tracing method. Another thing to note, the parts of the mandelbulb further away are redder, showing that those required more iterations to color because of their distance. But the sphere trace method wasn't dependent on distance, and as a result we get a mandlebulb that is the same color of green all around.

### Performance Breakdown
From this chart you can see how the performance of my shader breaks down between various different stages: the ray march, Lambert lighting calculation, and calculating soft shadows. The majority of the time is spent calculating the soft shadows, but not much longer than is spent doing the ray march.
![](img/Performance Breakdown.png "Performance Breakdown")

### Acknowledgements
* [Morgan McGuire: Numerical Methods for Ray Tracing Implicitly Defined Surfaces](http://graphics.cs.williams.edu/courses/cs371/f14/reading/implicit.pdf)
* [Inigo Quilez: Terrain Raymarching](http://www.iquilezles.org/www/articles/terrainmarching/terrainmarching.htm)
* [morgan3d: Mandelbulb Explained](https://www.shadertoy.com/view/XsXXWS)
