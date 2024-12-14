====
SFML
====

1. `Basics`_
2. `Window`_
3. `Graphics`_

`back to top <#sfml>`_

Basics
======

* `Geometry`_

Geometry
--------
    * window is a 2D object, top left corner is (0, 0)
    * x increases to right, y increases to bottom
    * objects in a window also have top left corner as (0, 0)
    * circle object position is determined by it's bounding rectangle
    * other shapes position is also determined by the bounding box
    * coordinates of circle's bounding box corners: top left (x, y), top right (x + 2r, y),
    bottom right (x + 2r, y + 2r), bottom left (x, y + 2r)

`back to top <#sfml>`_

Window
======

* define by [``sf::Window``](https://www.sfml-dev.org/documentation/2.5.1/classsf_1_1Window.php) class
* can be created and opened directly upon construction

    .. code-block:: cpp

       #include <SFML/Window.hpp>
   
       int main()
       {
           // 800 x 600 is size without title bar and borders
           sf::Window window(sf::VideoMode(800, 600), "My window");
       }


* constructor accepts third argument for style
    * ``sf::Style::None``: cannot be combined with other styles
    * ``sf::Style::Titlebar``: window has titlebar
    * ``sf::Style::Resize``: can resize and has maximize button
    * ``sf::Style::Close``: has close button
    * ``sf::Style::Fullscreen``: open in fullscreen mode, cannot combine with others,
    require valid video mode
    * ``sf::Style::Default``: default, combine Titlebar, Resize and Close
* constructor accepts fourth argument for OpenGL specific options
* use ``create()`` to create window after construction


    .. code-block:: cpp

       #include <SFML/Window.hpp>
   
       int main()
       {
           sf::Window window;
           window.create(sf::VideoMode(800, 600), "My window");
       }


* need event handling to make window alive, adding infinite loop won't work
    * ``pollEvent()`` returns true if event was pending
    * without event loop, window will become unresponsive
    * event loop provides events to the user and gives the window a chance process
    internal events

    .. code-block:: cpp

       #include <SFML/Window.hpp>
   
       int main()
       {
           sf::Window window(sf::VideoMode(800, 600), "My window");
   
           // main/game loop to ensure application will be refreshed/updated
           while (window.isOpen()) {
               sf::Event event;
               // use while event loop to process all pending events
               while (window.pollEvent(event)) {
                   // check event type and act accordingly
                   if (event.type == sf::Event::Closed)
                       // can do other things before closing window
                       window.close();
               }
           }
           return 0;
       }


*  use OpenGL directly or sfml-graphics module to draw stuff, it is not the responsibility
of sfml-window module
* ``sf::Window`` internally creates OpenGL context and can accept OpenGL calls
* SFML windows are only meant to provide environment for OpenGL or SFML drawing
* only has basic window operations, no advanced features like dedicated GUI libraries such
as Qt or wxWidgets
* some example functions

    .. code-block:: cpp

       // change the position of the window (relatively to the desktop)
       window.setPosition(sf::Vector2i(10, 50));
   
       window.setSize(sf::Vector2u(640, 480)); // change the size of the window
   
       window.setTitle("SFML window"); // change the title of the window
   
       // get the size of the window
       sf::Vector2u size = window.getSize();
       unsigned int width = size.x;
       unsigned int height = size.y;
   
       bool focus = window.hasFocus(); // check whether the window has the focus


* can create window with another library and embed SFML into it
    * use other constructor or ``create()`` of ``sf::Window`` that takes OS-specific handle
    * SFML will create drawing context inside the given window and catch events without
    interfering with parent window

    .. code-block:: cpp

       sf::WindowHandle handle = /*specific things for the library used*/;
       sf::Window window(handle);
   
       sf::Window window(sf::VideoMode(800, 600), "My Window");
       // use the handle with OS-specific functions
       sf::WindowHandle handle = window.getSystemHandle();


* can activate vertical synchronization to reduce tearing
    * tearing: application's refresh rate no synchronize with vertical frequency of
    monitor and bottom of previous frame is mixed with the top of the next one
    * vertical synchronization is auto handled by the graphics card
    * ``setVerticalSyncEnabled()`` will not work if vertical synchronization is forced to
    off in graphics driver's settings
    * can also limit to specific frame rate with ``setFramerateLimit()``, which is
    implemented by SFML itself, using `sf::Clock` and `sf::sleep`
    * ``setFramerateLimit()`` is not 100% reliable, especially for high framerates as
    ``sf::sleep`` resolution depends on underlying operating system and hardware and can be
    high as 10 or 15ms
    * do not use ``setFramerateLimit()`` to implement precise timing
    * never use ``setVerticalSyncEnabled()`` and ``setFramerateLimit()`` at the same time

    .. code-block:: cpp

       // only call once after creating the window
       window.setVerticalSyncEnabled(true); // will run at same frequency of monitor's
   
       window.setFramerateLimit(60); // run at 60fps


* can create multiple windows and handle all in the main thread or each in its own thread,
but each require an event loop
* cannot manage multiple monitors yet
* cannot choose which monitor a window appears on yet
* cannot create more than one fullscreen window yet
* event loop, ``pollEvent()`` or ``waitEvent()``, must be called in the same thread that
created the window
* if necessary, it is better to keep event handling in the main thread and move the rest,
such as rendering, physics, to a separate thread
* on macOS, windows and events must be manged in the main thread
* on Windows, window bigger than than the desktop will not behave correctly

`back to top <#sfml>`_

Graphics
========

* `Drawing`_, `Sprites & Textures`_, `Text & Fonts`_, `Shapes`_, `Transforming`_

Drawing
-------
    * must use [``sf::RenderWindow``](https://www.sfml-dev.org/documentation/2.5.1/classsf_1_1RenderWindow.php) class, which is derived from ``sf::Window``
    * adds high-level functions to help draw easily
    * ``clear()``
        - clears the whole window
        - calling it before drawing anything is mandatory
        - without clearing, contents from previous frames will be present
        - can cover entire window with drawing and not call ``clear()``, but will have
          performance impact
    * ``draw()``: draws any object passed to it
    * ``display()``
        - calling it is mandatory
        - takes what was drawn since the last call to ``display()`` and displays it on window
        - objects are not drawn directly to the window, but to a hidden buffer
        - double-buffering: hidden buffer is copied to the window when ``display()`` is called
    * always use clear/draw/display cycle
    * keeping previous frames, erasing pixels or drawing once and calling ``display()`` multiple
    times will get strange results due to double-buffering

    .. code-block:: cpp

       #include <SFML/Graphics.hpp>
   
       int main()
       {
           sf::RenderWindow window(sf::VideoMode(800, 600), "My Window");
   
           while (window.isOpen()) {
               sf::Event event;
   
               while (window.pollEvent(event)) {
                   if (event.type == sf::Event::Closed) { window.close(); }
               }
   
               window.clear(sf::Color::Blue); // clear window with black color
               // draw objects
               window.display(); // end current frame
           }
       }


    * can use ``sf::RenderTexture`` to draw a texture instead of directly to a window
        - has same functions as ``sf::RenderWindow`` to handle views and OpenGL
        - can request creation of depth buffer with ``texture.create(500, 500, true)``

        .. code-block:: cpp

           #include <SFML/Graphics.hpp>
   
           int main()
           {
               sf::RenderWindow window(sf::VideoMode(800, 600), "My Window");
   
               sf::RenderTexture texture;
               if (!texture.create(500, 500)) { return -1; }
   
               // 'getTexture()' returns read-only texture
               // can copy own 'sf::Texture' instance to modify it
               sf::Sprite sprite(texture.getTexture());
   
               while (window.isOpen()) {
                   sf::Event event;
   
                   while (window.pollEvent(event)) {
                       if (event.type == sf::Event::Closed) { window.close(); }
                   }
   
                   texture.clear(sf::Color::Red);
                   texture.draw(sprite);
                   texture.display();
   
                   window.clear();
                   window.draw(sprite);
                   window.display();
               }
           }


    * SFML supports multi-threaded drawing
        - need to deactivate a window before using it in another thread, as OpenGL context
          cannot be active in more than one thread at the same time

        .. code-block:: cpp

           #include <SFML/Graphics.hpp>
   
           void renderingThread(sf::RenderWindow* window)
           {
               window->setActive(true); // activate the window's context
   
               // the rendering loop
               while (window->isOpen())
               {
                   // draw...
                   window->display(); // end the current frame
               }
           }
   
           int main()
           {
               sf::RenderWindow window(sf::VideoMode(800, 600), "OpenGL");
   
               window.setActive(false); // deactivate its OpenGL context
   
               // launch the rendering thread
               sf::Thread thread(&renderingThread, &window);
               thread.launch();
   
               while (window.isOpen())
               {
                   // events & logic
               }
           }



Sprites & Textures
------------------
    * texture: image that is loaded and mapped to a 2D entity
    * sprite: a textured rectangle
    * use ``sf::Texture`` to create a valid texture, which is required for a sprite
        - most functions of a texture are about loading and updating
        - make sure to check stdout as ``loadFromFile()`` can fail with no obvious reason
        - ``loadFromMemory()``: load image file from memory
        - ``loadFromStream()``: load image file from custom input stream
        - ``loadFromImage()``: load from already loaded image, which loads the texture from
          ``sf::Image``
        - pixels of ``sf::Image`` stay in system memory and operations are as fast as possible
        - pixels of texture in video memory are slow to retrieve but fast to draw

        .. code-block:: cpp

           sf::Texture texture;
           if (!texture.loadFromFile("image.png")) {
               // error
           }
   
           // load a 32x32 rectangle that starts at (10, 10)
           if (!texture.loadFromFile("image.png", sf::IntRect(10, 10, 32, 32))) {
               // error
           }


    * can create empty texture and update pixels later

        .. code-block:: cpp

           // create an empty 200x200 texture, need to update pixels
           if (!texture.create(200, 200)) {
               // error...
           }
           // update a texture from an array of pixels
           sf::Uint8* pixels = new sf::Uint8[width * height * 4]; // * 4 because pixels have 4 components (RGBA)
           texture.update(pixels);
   
           // update a texture from a sf::Image
           sf::Image image;
           texture.update(image);
   
           // update the texture from the current contents of the window
           sf::RenderWindow window;
           texture.update(window);


    * can also specify coordinates and update only a part of texture
    * texture has two properties that change how it is rendered
        - ``texture.setSmooth(true)``: smooth texture that makes pixel boundaries less visible,
          but blur the image a little
        - ``texture.setRepeated(true)``: allow texture to be repeatedly tiled within a single
          sprite, only works if sprite is configured to show a rectangle larger than the texture
    * after creating texture, can create a sprite
        - color of sprite is modulated/multiplied with its texture, and can be used to change
          global transparency, alpha, of sprite

        .. code-block:: cpp

           sf::Sprite sprite;
           sprite.setTexture(texture);
   
           window.draw(sprite); // draw a sprite
   
           sprite.setTextureRect(sf::IntRect(10, 10, 32, 32)); // not using entire texture
   
           // change color of sprite
           sprite.setColor(sf::Color(0, 255, 0)); // green
           sprite.setColor(sf::Color(255, 255, 255, 128)); // half transparent


    * sprites can be transformed, as they have position, orientation and scale
        - origin is the top left of the sprite, but can set it to a different point

        .. code-block:: cpp

           // position
           sprite.setPosition(sf::Vector2f(10.f, 50.f)); // absolute position
           sprite.move(sf::Vector2f(5.f, 10.f)); // offset relative to the current position
   
           // rotation
           sprite.setRotation(90.f); // absolute angle
           sprite.rotate(15.f); // offset relative to the current angle
   
           // scale
           sprite.setScale(sf::Vector2f(0.5f, 2.f)); // absolute scale factor
           sprite.scale(sf::Vector2f(1.5f, 3.f)); // factor relative to the current scale
   
           // set origin to different point of sprite
           sprite.setOrigin(sf::Vector2f(25.f, 25.f));


    * if texture is destroyed or move in memory, sprite ends up with invalid texture pointer
        - when texture of a sprite is set, it internally stores a pointer to the texture
          instance
        - invalid texture pointer causes to show only a white square
        - must correctly manage the lifetime of texture to be as long as the sprite using it

        .. code-block:: cpp

           sf::Sprite loadSprite(std::string filename)
           {
               sf::Texture texture;
               texture.loadFromFile(filename);
               return sf::Sprite(texture);
           } // error: the texture is destroyed here


    * use few textures as possible, as changing current texture is expensive for graphics card
    * drawing many sprites that use the same texture is optimal
    * use single texture that allows to group static geometry into single entity will be much
    faster, as can only use one texture per draw call
    * can use ``sf::Texture`` as wrapper around OpenGL texture

        .. code-block:: cpp

           sf::Texture texture;
           sf::Texture::bind(&texture); // bind texture
   
           // draw textured OpenGL entity...
   
           sf::Texture::bind(NULL); // bind no texture



Text & Fonts
------------
    * need a font to draw text
    * ``sf::Font`` provides loading a font, getting glyphs and reading its attributes
        - only need to load a font for most of the time
        - SFML will not load system fonts automatically
        - need to include the font file with application
        - make sure to check stdout as ``loadFromFile()`` can fail with no obvious reason
        - ``loadFromMemory()``: load font file from memory
        - ``loadFromStream()``: load font file from custom input stream

        .. code-block:: cpp

           sf::Font font;
           if (!font.loadFromFile("arial.ttf")) {
               // error
           }
   
           // can get font's other metrics
           int lineSpacing = font.getLineSpacing(characterSize);
           int kerning = font.getKerning(char1, char2, characterSize);


    * once font is loaded, text can be drawn by using ``sf::Text``

        .. code-block:: cpp

           sf::Text text;
   
           text.setFont(font); // select the font, font is a sf::Font
   
           text.setString("Hello world"); // set the string to display
   
           text.setCharacterSize(24); // set the character size in pixels, not points
   
           text.setFillColor(sf::Color::Red); // set the color
   
           text.setStyle(sf::Text::Bold | sf::Text::Underlined); // set the text style
   
           // inside the main loop, between window.clear() and window.display()
           window.draw(text);


    * text can be transformed, as they have position, orientation and scale
    * **Wide Strings**
        - handling non-ASCII characters is tricky, as it require understanding of encodings
        - use wide literal strings to avoid bothering with encodings
        - ``text.setString(L"יטאח")``, the prefix ``L`` tells the compiler to produce wide string
        - on most platforms, wide strings produce Unicode strings, which SFML can handle
          correctly
    * **Custom Text Class**
        - ``sf::Font`` provides feature to retrieve texture that contains pre-rendered glyphs of
          certain size
        - glyphs are added to the texture when they are requested
        - characters that can be generated when font is loaded are rendered on the fly when
          ``getGlyph()`` is called

        .. code-block:: cpp

           const sf::Texture& texture = font.getTexture(characterSize);
   
           // to do something with font texture, need to get texture coordinates of glyphs
           // character is UTF-32 code
           sf::Glyph glyph = font.getGlyph(character, characterSize, bold);


    * ``sf::Glyph`` members
        - ``textureRect``: contains texture coordinates of glyph within texture
        - ``bounds``: contains bounding rectangle of glyph, which helps position it relative to
          baseline of text
        - ``advance``: horizontal offset to apply to get starting position of the next glyph in
          the text

Shapes
------
    * each type of shape is separate class, but has same base class
    * all shapes has transformation properties of position, rotation, scale
    * can change the color of a shape

        .. code-block:: cpp

           sf::CircleShape shape(50.f);
           shape.setFillColor(sf::Color(100, 250, 50));
   
           window.draw(shape);


    * shapes can have outline
        - outline is extruded outwards by default, e.g. circle radius 10 and outline thickness
          5 will make total circle radius of 15
        - set negative thickness to extrude towards the center of the shape

        .. code-block:: cpp

           sf::CircleShape shape(50.f);
           shape.setFillColor(sf::Color(100, 250, 50));
           shape.setOutlineThickness(10.f); // outline extrude outwards
           shape.setOUtlineColor(sf::Color::Red);
   
           shape.setOutlineThickness(-10.f); // outline extrude inwards
   
           shape.setOutlineThickness(0.f); // remove outline
   
           // only show outline
           shape.setFillColor(sf::Color::Transparent);
           shape.setOutlineThickness(10.f);
           shape.setOUtlineColor(sf::Color::Red);


    * shapes can also be textured like sprites
        - ``setTextureRect()``: map the texture rectangle to the bounding rectangle of shape,
          not maximum flexibility, but easier than individually setting texture coordinates of
          each point of the shape
        - outline is not textured
        - texture is modulated/multiplied with shape's fill color
        - ``sf::Color::White`` will cause texture appear unmodified
        - use ``setTexture(NULL)`` to disable texturing

        .. code-block:: cpp

           // map 100x100 textured rectangle to shape
           shape.setTexture(&texture); // must be of 'sf::Texture'
           shape.setTextureRect(sf::IntRect(10, 10, 100, 100));


    * **Rectangle**
        - ``sf::RectangleShape``, has size attribute

        .. code-block:: cpp

           sf::RectangleShape rectangle(sf::Vector2f(120.f, 50.f)); // 120x50
           rectangle.setSize(sf::Vector2f(100.f, 100.f)); // change size to 100x100


    * **Circle**
        - ``sf::CircleShape``, has radius and number of sides attributes
        - number of sides: optional, to adjust quality of the circle as circles have to be
          approximated by polygons with many sides

        .. code-block:: cpp

           circle.setRadius(50.f); // change radius
           circle.setPointCount(100); // change number of sides


    * **Regular Ploygons**
        - no dedicated class
        - use ``sf::CircleShape`` with specific number of sides, as circles are approximated by
          polygons with many sides

        .. code-block:: cpp

           sf::CircleShape triangle(50.f, 3);
           sf::CircleShape square(50.f, 4);
           sf::CircleShape octagon(50.f, 8);


    * **Convex Shapes**
        - ``sf::ConvexShape``, allows to define any convex shape
        - need to set number points and define them
        - SFML cannot draw concave shapes
        - combine multiple convex polygons to draw a concave shape
        - convex shapes are auto constructed using triangle fans
        - can draw stars using ``sf::ConvexShape``

        .. code-block:: cpp

           sf::ConvexShape convex;
           convex.setPointCount(5);
           convex.setPoint(0, sf::Vector2f(0.f, 0.f));
           convex.setPoint(1, sf::Vector2f(150.f, 10.f));
           convex.setPoint(2, sf::Vector2f(120.f, 90.f));
           convex.setPoint(3, sf::Vector2f(30.f, 100.f));
           convex.setPoint(4, sf::Vector2f(0.f, 50.f));


    * **Lines**
        - no dedicated class
        - use ``sf::RectangleShape`` or line primitive

        .. code-block:: cpp

           sf::RectangleShape line(sf::Vector2f(150.f, 5.f)); // line with thickness
           line.rotate(45.f);
   
           // line without thickness using line primitive
           sf::Vertex line2[] = {sf::Vertex(sf::Vector2f(10.f, 10.f)),
                                 sf::Vertex(sf::Vector2f(150.f, 150.f))};


    * **Custom Shapes**
        - extend ``sf::Shape`` and override ``getPointCount()`` and ``getPoint()``
        - call protected ``update()`` whenever any point in the shape changes, so that the base
          class is informed and can update its internal geometry

        .. code-block:: cpp

           class EllipseShape : public sf::Shape {
           public:
               explicit EllipseShape(const sf::Vector2f& radius = sf::Vector2f(0.f, 0.f))
                   : m_radius(radius)
               {
                   update();
               }
   
               void setRadius(const sf::Vector2f& radius)
               {
                   m_radius = radius;
                   update();
               }
   
               const sf::Vector2f& getRadius() const { return m_radius; }
   
               virtual std::size_t getPointCount() const
               {
                   return 30; // fixed, but could be an attribute of the class if needed
               }
   
               virtual sf::Vector2f getPoint(std::size_t index) const
               {
                   static const float pi = 3.141592654f;
   
                   float angle           = index * 2 * pi / getPointCount() - pi / 2;
                   float x               = std::cos(angle) * m_radius.x;
                   float y               = std::sin(angle) * m_radius.y;
   
                   return sf::Vector2f(m_radius.x + x, m_radius.y + y);
               }
           private:
               sf::Vector2f m_radius;
           };


    * **Antialiasing**
        - cannot anti-alias, smooth edges, single shape
        - need to enable anti-aliasing globally when creating the window, with
          ``sf::ContextSettings``
        - anti-aliasing availability depends on GPU, or might be disabled in GPU settings

        .. code-block:: cpp

           sf::ContextSettings settings;
           settings.antialiasingLevel = 8;
           sf::RenderWindow window(sf::VideoMode(800, 600), "My Window",
                                   sf::Style::Default, settings);



Transforming
------------
    * all SFML classes use ``sf::Transformable`` for transformations
    * the class defines position, rotation, scale and origin
    * **Position**
        - entities are positioned relative to their top-left corner by default, but can change
          by setting origin

        .. code-block:: cpp

           entity.setPosition(10.f, 5.f); // set absolute position
           entity.move(5.f, 5.f); // move relative to current position
           sf::Vector2f position = entity.getPosition(); // get absolute position


    * **Rotation**
        - orientation defined in degrees
        - in clockwise order, as Y axis is pointing down
        - ``getRotation()`` returns angle in range [0, 360)
        - rotation is performed around the top-left corner by default, but can change by
          setting origin


        .. code-block:: cpp

           entity.setRotation(5.f); // set absolute rotation
           entity.rotate(5.f); // rotate relative to current orientation
           float rotation = entity.getRotation(); // get absolute rotation


    * **Scale**
        - resize entity, default scale is 1, smaller if less than 1, bigger if greater than 1
        - negative scale values are allowed, to mirror the entity

        .. code-block:: cpp

           entity.setScale(4.f, 1.6f); // set absolute scale
           entity.scale(0.5f, 0.5f); // scale relative to current scale
           sf::Vector2f rotation = entity.getScale(); // get absolute scale


    * **Origin**
        - center point of three other transformations
        - entity's position is the position of its origin, rotation is performed around the
          origin, scale is applied relative to the origin
        - origin is top-left corner by default, can set it to center or any other corner
        - only single origin for all three transformations, cannot position relative to
          top-left corner and rotate around the center
        - changing the origin also changes where the entity is drawn on screen

        .. code-block:: cpp

           entity.setOrigin(10.f, 20.f);
           sf::Vector2f origin = entity.getOrigin();


`back to top <#sfml>`_
