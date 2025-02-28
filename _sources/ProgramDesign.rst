==============
Program Design
==============

1. `Basics`_
2. `Designing Functions`_
3. `Designing Data`_

`back to top <#program-design>`_

Basics
======

* `Logical Operators`_
* design is the process of going from a poorly formed problem to a well structured solution
* making the problem more specific is part of the design process
* expression: either a value or of the form (<primitive> <expression>...), e.g. 2, (+ 2 3)
* value: numbers, strings
* generally, evaluation of an expression proceeds from left to right, inside to outside
    * (+ 2 (* 3 4)) -> (+ 2 12) -> 14
    * (+ 2 (* 3 4)) and (+ 2 12) are primitive calls
    * ``+`` and ``*`` are operators
    * 2, 3 and 4 are operands
* constant definitions make easy for other people to read and change
* readability and changeability are important properties of a program
* functions can be used repeatedly with different parameter values
    * using a function can make the code more concise
    * the code can have more meaning if a function is named well
    * can avoid redundancy and duplicate code as program gets bigger
* predicates are primitives or functions that produce boolean values

Logical Operators
-----------------
    * all expressions must evaluate to a boolean
    * **AND**
        - if first expression is true, keeps evaluating
        - all expressions must be true to produce true
        - if one expression is false, stops right away and produce false
        - short circuiting: does not keep evaluating if encounters one false
    * **OR**
        - if first expression is true, produce true
        - if first expression is false, keeps evaluating
        - produce true if there is one true in all expressions
    * **NOT**
        - reverses/negates the value of the expression

`back to top <#program-design>`_

Designing Functions
===================

* `Basics Steps`_, `Example`_
* design recipes make hard problems easier, by breaking it down and work through in systematic
fashion, but make easy problems cumbersome

Basic Steps
-----------
    * **Signature**
        - tells what type of data a function consumes and produces
        - capitalize the types, and be as specific as possible
        - helps to write later steps
    * **Purpose**
        - description of what function produces in terms of what it consumes
        - be as short as possible
    * **Stub**
        - scaffolding to be place for a short period of time
        - comment out or delete later
        - must have correct function name, parameters and produce dummy result of correct type
    * **Examples/Tests**
        - help understand what function must do, write multiple tests to illustrate behavior
        - run tests after the stub to see if they are well-formed, but do not consider the
        stub is correct if all tests pass
        - tests might fail before the whole function is completely and correctly designed
        - test coverage: given all the tests, how much of the code is being evaluated
        - also write tests for boundary/corner cases
    * **Inventory/Template**
        - correct function name and parameters
        - body of the template is the outline of function
        - comment out or delete later
    * use what is written in the previous steps to complete the function body
    * not a structured waterfall process, can skip a step and come back or correct later
    * run tests and debug as necessary, look for inconsistency between different steps
    * the sooner a mistake is found, the easier it is to fix
    * when tests fail, check both function definition and tests against signature and purpose
    * failing tests do not mean only function definition is incorrect

Example
-------

    .. code-block:: c

       // Design a function that consumes a number and produces twice that number.
   
       /* Signature */
       // Number -> Number
   
       /* Purpose */
       // produce 2 times the given number
   
       /* Stub */
       // comment or delete in later steps
       double twice_num(double n)
       {
           return 0;
       }
   
       /* Tests */
       // this is just example, real test functions might be implemented differently
       void test()
       {
           // do not write the expected result, write how it is computed
           assert(twice_num(2) == 4); // should use (2 * 2)
           assert(twice_num(3.2) == (2 * 3.2));
       }
   
       /* Template */
       // comment or delete in later steps
       double twice_num(double n)
       {
           (...n)
       }
   
       /* actual function written using previous steps */
       double twice_num(double n)
       {
           return 2 * n;
       }


`back to top <#program-design>`_

Designing Data
==============

* `Basic Steps`_, `Structure of Data`_, `Example`_

Basic Steps
-----------
    * **Data Definition**
        - explain how information is represented
        - effect on design of functions that operate on that data
        - simplify functions by restricting data consumed and produced
    * **Type Comment**
        - define a new type name
        - show how to form the data of that type
    * **Interpretation**
        - describe correspondence between information and data
    * **Example Data**
        - show one or more examples of data
    * **Template**
        - create template for one argument function on data
        - template is determined by the type of data it consumes

Structure of Data
-----------------
    * structure of information determines data definition, structure of templates, function
    examples and much of final program design
    * **Atomic Data**
        - information is itself in atomic form, does not derive from other data
        - one or two examples are sufficient
        - at least one function test case required
        - for boolean, at least test cases for true and false
        - atomic non-distinct: number, string, boolean, image, interval like number [0, 10)
        - atomic distinct: "red", false, empty

Example
-------

    .. code-block:: c

       /* Data Definition */
       // Time is Natural
   
       /* Interpretation */
       // interp. number of clock ticks since start of game
   
       /* Example Data */
       int START_TIME = 0;


`back to top <#program-design>`_
