===
ECS
===

1. `Basics`_
2. `Entity`_
3. `Component`_
4. `System`_

`back to top <#ecs>`_

Basics
======

* Entity Component System, software paradigm mostly used in video game development
* uses Composition-based design
* when building simple 2D games, using ECS might be overkill, and OOD might be more suitable
* Entity: any object in the game, e.g., player, tile, enemy
* Component: properties that can be attached to Entities, e.g., position, speed, health
* components can store only pure data or have functions implementing the logic of components
* Systems: code/logic that drives behavior, e.g., movement system that updates the position

`back to top <#ecs>`_

Entity
======

* `Declaring Entity`_, `Using Entities`_, `Entity Management`_
* any object in the game, usually any object with a position
* no unique functionality, only has a number of Components attached
* can only have at most one type of component, i.e., a player entity cannot have two position
  components
* how Components are stored and used within an Entity can differ and complex in some cases

Declaring Entity
----------------

    .. code-block:: cpp

       class Entity {
       public:
           std::shared_ptr<C_name> c_name;
           std::shared_ptr<C_transform> c_transform;
   
           Entity() {}
           ~Entity() { /* free memory if needed (RAII) */ }
       };
   
       int main()
       {
           std::vector<Entity> entities;
   
           Vec2 p(100, 200), v(10, 10);
   
           Entity e;
           e.c_transform = std::make_shared<C_transform> (p, v);
           e.c_name = std::make_shared<C_name> ("Hello");
   
           entities.push_back(e);
   
           s_movement(entities);
       }



Using Entities
--------------

    .. code-block:: cpp

       // pass by reference decreases memory usage and allows modification
       void s_movement(std::vector<Entity>& entities)
       {
           for (auto& e : entities) {
               e.c_transform->pos += e.c_transform->velocity;
           }
       }



Entity Management
-----------------
    * important to separate logic and data
    * create data structures to manage data, and abstract logic/algorithm from which data
      structure is used
    * Entity Manager uses Factory design pattern and handles creation, storage, and lifetime
      of all entity objects
    * need to ensure that user cannot create its own entities
    * group/tag entities by functionality, and Entity_manager should be able to get entities of
      given tag

        .. code-block:: cpp

           class Entity {
           public:
               Entity(size_t id, const std::string& tag): m_id{id}, m_tag{tag} {}
               const std::string& tag();
           private:
               const size_t      m_id    = 0;
               const std::string m_tag   = "Default";
               bool              m_alive = true; // to determine to destroy or not
           };


    * example Entity Manager

        .. code-block:: cpp

           typedef std::vector<std::shared_ptr<Entity>> Entity_vector;
           typedef std::map<std::string, Entity_vector> Entity_map;
   
           class Entity_manager {
           public:
               Entity_manager();
               std::shared_ptr<Entity> add_entity(const std::string& tag);
               Entity_vector&          get_entities();
               Entity_vector&          get_entities(const std::string& tag);
           private:
               Entity_vector m_entities;
               Entity_map    m_entity_map;
               size_t        m_total_entities = 0; // total entities created, not current total
           };


    * iterator invalidation can occur by modifying a collection while iterating through it
        - e.g., iterating through entities to check collisions, and removing when one dies
        - in Cpp, vector functions may cause reallocation and invalidates pointers and
          iterators

        .. code-block:: cpp

           for (auto& b : bullets) {
               for (auto& t : tiles) {
                   if (physics::check_collision(b, t)) {
                       // modifying a collection
                       bullets.erase(b); // can cause iterator invalidation
                   }
               }
           }


    * to avoid iterator invalidation, delay the effects of actions that modify collections
      until it is safe
        - with Entity Manager, it becomes easy to handle Entity creation and destruction
        - only add or remove entities at the beginning of a frame when it is safe

        .. code-block:: cpp

           class Entity_manager {
           public:
               void update(); // called at next frame when safe
           private:
               Entity_vector m_to_add; // to be added at next frame
           };
   
           void Entity_manager::update() // called at next frame when safe
           {
               for (auto e : m_to_add) {
                   m_entities.push_back(e);
                   m_entity_map[e->tag()].push_back(e);
               }
   
               m_to_add.clear();
           }


    * only allow Entity objects to be created by Entity Manager
        - make Entity constructor private and add Entity Manager as friend class
        - by having private constructor, ``std::make_shared<Entity>(args)`` cannot be used
        - for private constructor, need to use ``std::shared_ptr<Entity>(new Entity(args))``

        .. code-block:: cpp

           auto   e = std::make_shared<Entity>(args); // error
           auto   e = new Entity(args); // error
           Entity e(args); // error
   
           // must use Entity Manager to create entities
           auto e = entity_manager.add_entity(args);


`back to top <#ecs>`_

Component
=========

* `Storing Components`_, `Component Variables`_, `Component Container`_
* `C_transform`_, `C_shape`_
* just data, and maybe some logic in the constructor
* no helper functionality within components
* a Component class has some meaning to an Entity which contains it
    * e.g., entity containing Health component can live or die base on the health amount

Storing Components
------------------
    1. have a variable for each component type
    2. have a single container of Components, and have `add_component()` or `get_component()`

Component Variables
-------------------

    .. code-block:: cpp

       class Entity {
       public:
           C_name* c_name = nullptr;
           C_bbox* c_bbox = nullptr;
           // OR
           std::shared_ptr<C_name> c_name;
           std::shared_ptr<C_bbox> c_bbox;
   
           Entity() {}
           ~Entity() { /* free memory if needed (RAII) */ }
       };



Component Container
-------------------

    .. code-block:: cpp

       class Entity {
       public:
           Entity() {}
           ~Entity() { /* free memory if needed (RAII) */ }
           void add_component<T>(args);
           void get_component<T>(args);
       private:
           std::vector<Component> m_components;
           // OR
           std::tuple<C1, C2, C3, C4> m_components;
       };



C_transform
-----------
    * movement within a space

    .. code-block:: cpp

       class C_transform {
       public:
           Vec2 pos = {0, 0};
           Vec2 velocity = {0, 0};
           C_transform() {}
           C_transform(const Vec2& p, const Vec2& v): pos{p}, velocity{v} {}
       };



C_shape
-------
    * movement within a space

    .. code-block:: cpp

       class C_shape {
       public:
           // using SFML
           sf::CircleShape shape;
           C_shape() {}
       };


`back to top <#ecs>`_

System
======

* `Render System`_
* some systems only operate on entities with certain components
    * Movement system only operates on entities with C_transform
    * Physics system only operates on entities with C_bbox
* filter entities before passing them into system
* check entities for required components before using them

Render System
-------------
    * check components in the system function before rendering

        .. code-block:: cpp

           void s_render(std::vector<Entity>& entities)
           {
               for (auto& e : entities) {
                   if (e.c_shape && e.c_transform) {
                       e.c_shape->shape.set_position(e.c_transform->pos);
                   }
               }
           }


`back to top <#ecs>`_
