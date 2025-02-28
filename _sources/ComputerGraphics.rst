=================
Computer Graphics
=================

1. `Basics`_
2. `Graphics Pipeline`_
3. `Maths`_

`back to top <#computer-graphics>`_

Basics
======

* `Graphics API`_, `Making Code Fast`_, `Designing Graphics Programs`_
* computer graphics is mainly used in areas such as
    * modeling, rendering, animation, user interfacing, virtual reality, visualization
    * image processing, 3-D scanning, computational photography, visual effects
    * video games, cartoons, animated films, CAD/CAM, simulation, medical imaging
* every display and graphics are 2D array of pixels, Picture Element, and every pixel has a
  color
* 3D graphics is just pixels colored to look 2D image like a 3D world
* rendering: the process of converting 3D world into 2D image

Graphics API
------------
    * API (Application Programming Interface): standard collection of functions
    * graphics API: for visual output
    * user-interface API: to get input from user
    * **Integrate Paradigm APIs**
        - such as in Java, where toolkits are integrated and portable packages
    * **Library Paradigm APIs**
        - such as in Direct3D and OpenGL, where drawing commands are part of software library
        - user-interface software is independent and might vary from system to system
        - can be problematic to write portable code, but possible to use portable library
        layer

Making Code Fast
----------------
    * write code in most straightforward way
    * compile in optimized mode
    * use profiling tools to find bottlenecks
    * review data structures to improve locality
    * make data unit sizes match the cache/page size if possible
    * examine assembly code to improve if needed

Designing Graphics Programs
---------------------------
    * have good classes or routines for geometric entities such as vectors and metrices
    * **Basic Classes**
        - vector2, vector3: 2D and 3D vectorss to store x and y components, should have
        operations for vector addition, subtraction, dot product, cross product, scalar
        multiplication and division
        - hvector: homogeneous vector with four components
        - rgb: to store RGB, three color components, should have operations for RGB addition,
        RGB subtraction, RGB multiplication, scalar multiplication and division
        - transform: 4x4 matrix for transformations, should have matrix multiply, functions
        to apply to locations, directions and surface normal vectors
        - image: 2D array of RGB pixels with output operation
    * best to keep memory usage down and maintain coherent memory access
    * consider tradeoffs between using single-precision data and double-precision arithmetic
    * use doubles for geometric computation and floats for color computation
    * for memory heavy data such as triangle meshes, store float data, but convert to double
      when data are accessed through member functions

`back to top <#computer-graphics>`_

Graphics Pipeline
=================

* `Basic Operation`_, `4D Coordinate Space`_, `Level of Detail`_
* `Rasterization`_
* special software/hardware subsystem to draw 3D primitives
* optimized for processing 3D triangles with shared vertices

Basic Operation
---------------
    * map 3D vertex locations to 2D screen positions
    * shade the triangles to look realistic and appear in proper back-to-front order, usually
      using the z-buffer

4D Coordinate Space
-------------------
    * composed of three traditional geometric coordinates and a fourth homogeneous coordinate
      that helps with perspective viewing
    * 4D coordinates are manipulated using 4x4 matrices and 4 vectors

* image generation speed strongly depends on the number of triangles being drawn

Level of Detail
---------------
    * it is useful to represent a model with varying level of detail, LOD
    * fewer triangles are needed when a model is viewed in the distance
    * more triangles are needed when a model is viewed in from closer distance

Rasterization
-------------
    * rasterizer: rendering system that uses rasterization
    * all objects are empty shells, which are made of triangles
    * more triangles are generated for objects that are closer or larger
    * planar quadrilaterals: four-sided objects, where all points lie in the same plane, used
      by some rasterizers
    * hardware-based rasterizers used triangles as all lines of a triangle are guaranteed to be
      in the same plane
    * geometry/model/mesh: series of triangles that define outer surface of an object
    * rasterization has several phases, ordered into a pipeline, and is also amenable to
      hardware acceleration
    * the order of triangles and meshes submitted to the rasterizer can affect its output
    * OpenGL: API for accessing hardware-based rasterizer
    * **Clip Space Transformation**
        - first phase of rasterization
        - transform vertices of each triangle into certain region of space
        - only things within the volume will be rendered to the output image
        - clip space: the transformed triangle volume
        - clip coordinates: positions of triangle's vertices in clip space, has four
          coordinates, (X, Y, Z, W), W is range of clip space for the vertex, [-W, W]
        - clip space can be different for different vertices within a triangle
        - as each vertex can have independent W, each vertex of a triangle exists in its own
          clip space
        - positive X is to the right, positive Y is up, and positive Z is away from viewer
        - triangles partially outside of clip space undergo a process called clipping
        - clipping: break the triangle apart into number of smaller triangles, such that
          smaller ones are all entirely within clip space
    * **Normalized Device Coordinates**
        - more reasonable coordinate space from transforming clip space
        - obtained by dividing X, Y, Z of each vertex is with W
        - space of normalized device coordinates is just clip space, with range of X, Y, Z
          being [-1, 1]
        - directions are same as clip space
        - division by W is important in projecting 3D triangles onto 2D images
    * **Window Transformation**
        - convert normalized device coordinates to window coordinates
        - still 3D coordinates with same direction as clip space, still floating-point values
        - but bounds depend on the viewable window, with Z having [0, 1]
        - have bottom-left as (0, 0) origin point, but can transform to top-left if needed
    * **Scan Conversion**
        - takes a triangle and breaks it up based on the arrangement of window pixels over the
          output image that the triangle covers
        - sample: center of pixel, discrete location within the area of a pixel
        - triangle will produce a fragment for every pixel sample within the 2D area
        - as triangles are rendered with shared edges, if shared edge vertex positions are
          identical, there will be no sample gaps during scan conversion
        - invariance guarantee: identical output when passing same input vertex data through
          same vertex processor
        - gap-less scan conversion depends on the user to use same input vertices
        - as scan conversion is 2D operation, it only uses X and Y of triangle in window
          coordinates to generate fragments
        - Z value is used to determine the depth of the fragment
    * **Fragment Processing**
        - transform a fragment from scan converted triangle to one or more color values and
          single depth value
        - all fragments from one triangle must be processed before another triangle
        - Direct3D calls this stage pixel processing or pixel shading
    * **Fragment Writing**
        - fragment with colors and depth values is written to the destination image
        - combining the color and depth with the colors that are currently in the image can be
          a lot of computations
    * **Colors**
        - a color is usually a series of number with range [0, 1], each number defining
          intensity of particular reference color
        - color space: set of reference colors, e.g. RGB, CMYK
    * **Shader**
        - program designed to be run on a renderer, as part of rendering operation
        - can only be execute at certain points in the rendering process
        - shader stages: hooks to add algorithms to create specific visual effect
        - various shading languages available to various APIs, e.g. OpenGL Shading Language or
          GLSL

`back to top <#computer-graphics>`_

Maths
=====

* `IEE Floating Point`_, `2D Positions`_, `Vectors`_, `Random Number`_

IEE Floating Point
------------------
    * infinity, minus infinity, NaN (not a number), positive zero, negative zero
    * **Example Behaviors**
        - -a/(+&infin;) = -0, +a/(-&infin;) = -0, -a/(-&infin;) = +0
        - &infin;-&infin; = NaN, &infin;/&infin; = NaN, &infin;/0 = &infin;, 0/0 = NaN
        - -&infin; < all finite valid numbers < +&infin;
        - -&infin; < +&infin;
        - any arithmetic expression that includes NaN results in NaN
        - any boolean expression involving NaN is false
    * **Divide by Zero**
        - +a/+0 = +&infin;, -a/+0 = &infin;
    * false: NaN > 0, -&infin; > 0
    * true: +&infin; > 0

2D Positions
------------
    * system display is a pixel grid in two dimensions with (x, y) for positions
    * can use Euclidean plane to visualize
    * most APIs have (0, 0) in top-left
    * can represent a 2D value as a vector, ``a = (4, 2), b = (6, 7)``
    * subtraction: Destination - Start = Distance, b(6, 7) - a(4, 2) = c(2, 5)
    * addition: Start + Distance = Destination
    * **Trigonometry**
        - SOH: sin&Theta; = Opposite / Hypotenuse
        - CAH: cos&Theta; = Adjacent / Hypotenuse
        - TOA: tan&Theta; = Opposite / Adjacent
    * **Circle Collisions**
        - c1(x1, y1, r1), c2(x2, y2, r2)
        - Vector D = (x2 - x1, y2 - y1), Dist = sqrt(D.x<sup>2</sup> + D.y<sup>2</sup>)
        - collision = Dist < Radius Sum = Dist < r1 + r2
        - since sqrt is expensive, can optimize by squaring both sides
        - DistSq = D.x<sup>2</sup> + D.y<sup>2</sup>, collision = DistSq < (r1 + r2)<sup>2</sup>

Vectors
-------
    * **Dimensionality**
        - represents the number of dimensions a vector has
        - e.g two-dimensional vector is in single plane and three-dimensional vector in
          physical space
        - can have higher dimensions, but generally deal with dimensions between 2 and 4
        - a vector with only one dimension is called a scalar
    * **Geometrical**
        - a vector can represent a position or a direction within a space
        - direction vectors do not have an origin, simply specify a direction in space
    * **Numerical**
        - a vector can be a sequence of numbers
        - two-dimensional vector has two numbers, three-dimensional vector has three numbers
        - scalars are just single number
    * **Components**
        - numbers within a vector
        - e.g 3D vector of (0, 2, 4) has x = 0, y = 2, z = 4
        - component-wise operation: any operation performed on each component of a vector,
          requires vectors to have same dimensionality
    * **Addition**
        - vector directions can be shifted around without changing values
        - if two vectors are put head to tail, vector sum is the direction from the tail of
          the first vector to the head of the last
        - numerical sum of two vectors is the sum of corresponding components
    * **Negation & Subtraction**
        - negation reverses the direction of a vector, negate each component of the vector
        - subtraction is the same as addition with a negated second vector
    * **Multiplication**
        - no real geometric equivalent, but numerical equivalent is useful
        - two vectors are multiplied component-wise, like addition
    * **Vector/Scalar Operations**
        - vectors can be operated on by scalar values
        - magnifies or shrinks vector length, depending on the scalar value
        - scaling: component-wise, each component of vector is multiplied with a scalar
        - addition: component-wise, each component of vector is added with a scalar
    * **Vector Algebra**
        - vector addition and multiplication, and vector/scalar operations are commutative,
          associative and distributive
    * **Length**
        - distance from starting point to ending point
        - use Pythagorean theorem to compute any arbitrary dimensions
        - e.g. &Sqrt;x<sup>2</sup> + y<sup>2</sup> + z<sup>2</sup>
    * **Normalization**
        - unit vector: vector with length one, represents pure direction with unit length
        - normalization maintains its angle, but changes its length to 1, converted to unit
          vector
        - normalize a vector by dividing with its length, or multiplying with reciprocal of
          the length, N = Vec2(V.x / L, V.y / L)

Random Number
-------------
    * **Inclusive Range**
        - given range [min, max], range_size = 1 + max - min
        - r = rand() % range_size, r is in range [0, range_size - 1]
        - r = r + min, add back min to get proper range
        - r = min + (rand() % (1 + max - min))

`back to top <#computer-graphics>`_
