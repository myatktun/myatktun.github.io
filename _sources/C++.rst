===
C++
===

1. `Basics`_
2. `Built-in functions`_
3. `Arrays`_
4. `Pointers`_
5. `References`_
6. `Classes`_
7. `Object Oriented Programming`_
8. `Dynamic Memory`_
9. `Templates`_
10. `Preprocessor`_
11. `Signal Handling`_
12. `Multithreading`_
13. `Web Programming`_
14. `Graphics`_
15. `STL`_
16. `STL custom implementation`_
17. `Sample Project Structure`_

`back to top <#c>`_

Basics
======

* `Variables`_, `I/O`_, `Operators`_, `Literals`_, `Type Safety`_, `typedef`_, `Switch`_, `Loops`_, `Functions`_
* `Errors`_, `Exceptions`_, `Debugging`_, `Program Development`_, `Program Evaluation`_
* `Namespaces`_, `Struct`_, `Enumerations`_, `Overloading`_, `Lambda Expressions`_, `Files`_, `Date & Time`_
* process of developing a program: analysis, design, programming, testing
    * implementation: programming + testing
    * programming is part practical, part theoretical
* when approaching software, do it in a principled and serious manner, need to be part of the
solution, not add to the problems
* since ``main()`` is called by the system, the return value won't be used, but on systems like
Unix/Linux, it can be used to check program exit status
* source code or program text: what we read an write
* executable, object code or machine code: what the computer executes
    * object code files have suffix .obj (on Windows) or .o (Unix)
    * not portable among systems
* linker
    * links the compiled object code files of separate parts, translation units, to form an
    executable
* compile-time errors: found by the compiler
* link-time errors: found by the liker
* run-time or logic errors: not found until the program is run
* ``keep_window_open()``: needed on some Windows machines to prevent closing the window instantly

object
------

declaration
-----------
    * statement that gives a name to an object or introduces a name into a scope
    * can declare many times as long as consistent

        .. code-block:: cpp

           double sqrt(double);
           double sqrt(double);
           double sqrt(double);
   
           int sqrt(double); // error: inconsistent


    * functions and definitions consumes memory whereas declarations don't
    * reasons to declare before using
        - simplifies reading for both humans and compiler
        - having to know only the used declarations saves humans and compilers from looking
        through huge amounts of program text, especially when functions used are defined in
        other places

definition
----------
    * statement that introduces a new name and sets aside memory for a variable,
    every definition is also a declaration
    * ``const`` requires initializing
    * it is good to always initialize variables, uninitialized variables cause obscure bugs
    * preference for the ``{}`` initializer syntax
    * ``string`` and ``vector`` are initialized with default when not supplied explicitly
    * global variable is default initialized to 0, but should minimize use of globals
    * local and class members are uninitialized
* declaration/definition distinction allows to separate program into many parts that can be
compiled separately

header
------
    * collection of declarations, usually in another file
    *  to use headers, ``#include`` in (preprocess) source files, both in files that use its
    declarations and provide definitions
    * ``.h`` suffix is most common for headers, ``.cpp`` for source files
    * some compilers and most development environments insist on having suffixes although they
    can be omitted
    * should only contain declarations that can be duplicated in several files

Variables
---------
    * named objects
    * **types**
        - define a set of possible values and a set of operations
        - bool, char, int, float, double, void, wchar_t (wide char type)
        - definition without an initializer are initialized with NULL
        - ``extern`` tells the compiler that the variable is defined in another source, outside of current
        scope
        - omitting type with modifiers (signed, unsigned, long, short) auto implies int

        .. code-block:: cpp

           int i, j, k;
           char c;
           float f = 1.5, e = 2.2;
           extern q;
           unsigned x; // x is int


    * **type qualifiers**
        - *const*: cannot be changed during execution
        - *volatile*: value may be changed in ways not specified by the program
        - *restrict*: qualified pointer is initially the only means by which the object it points
        to can be accessed
    * **value**: a set of bits in memory interpreted according to a type
    * lvalue (variables)
        - expressions that refer to memory location
        - may appear in left or right side of assignment
        - "the object named by x"
    * rvalue (numeric literals)
        - data value stored in memory
        - cannot have a value assigned, only appear on the right
        - "values of object named by x"
    * **scopes**
        - global
           + defined outside of all functions, usually on top of the program
           + can be accessed by any function
        - local
           + declared inside a function or block
           + can be used only by statements that are inside the function
        - can have same name for local and global but value of local will take preference
    * **storage class**
        - defines the scope and life-time of variables and functions
        - *auto*
           + default class for all local variables
           + can only be used in functions/locals
        - *register*
           + for local variables to be stored in a register instead of RAM
           + variable max size is equal to register size usually one word
           + cannot have '&' operator applied to it as it does not have mem location
           + should only be used for quick access such as counters
           + not guaranteed to be stored in register depending on hardware restrictions
        - *static*
           + keep local variable instead of creating and destroying
           + maintain values between function calls
           + applying to global causes it's scope to be restricted to the declared file
           + using on class data member causes only one copy of that member to be shared by all
           objects of its class
        - *extern*
           + to give reference of global
           + variable cannot be initialized as it only points the variable name at location that has
           been defined
           + commonly used when tow or more files share the same globals
        - *mutable*
           + applies only to class objects
           + allows a member of object to override const member function
    * logically, assignment and initialization are different
    * **constexpr**
        - symbolic constant and must be given a value at compile time
    * **scope**
        - region of program text
        - global scope: area of text outside any other scope
        - namespace scope: named scope nested in global scope or in another namespace
        - class scope: within class
        - local scope: between {...} of a block or in a function argument list
        - statement scope: as in a for-statement
        - to keep names local as not to interfere with names declared elsewhere
        - clash: two incompatible declarations in the same scope
        - keep names as local as possible
        - larger the scope of a name is, the longer and more descriptive its name should be
        - functions within classes: member functions
        - classes within classes: member classes
        - classes within functions: local classes (avoid)
        - functions within functions: local/nested functions (not legal in C++)
        - blocks within functions and other blocks: nested blocks

I/O
---
    * occurs in streams, sequences of bytes
    * reading of strings is terminated by whitespace (space, newline, tab)
    * ``<iostream>``
        - cin (standard input)
           + instance of istream class
           + used with stream extraction operator, get from, '>>'
        - cout (standard output)
           + instance of ostream class
           + used with stream insertion operator '<<'
        - cerr (un-buffered standard error stream)
           + instance of ostream class
           + each stream insertion causes output to appear immediately
           + more resilient to errors as it is not optimized
        - clog (buffered standard error stream)
           + instance of ostream class
           + each stream insertion is held in a buffer till filled or flushed
    * ``<iomanip>``
        - services useful for formatted I/O with parameterized stream manipulators
        - setw, setprecision
    * ``<fstream>``
        - services for user-controlled file processing
    * reading a name consisting of two words

        .. code-block:: cpp

           string first, second;
           cin >> first >> second;


    * reading character array using ``cin.get()``, which reads a string with whitespace

        .. code-block:: cpp

           char ch[100];
           cin.get(ch, 50);


    * have a balance between program complexity and accommodation of users' personal tastes
    * hexadecimal
        - a digit exactly represents 4-bit value
        - popular for outputting hardware-related information
    * **integer output manipulators**: ``oct``, ``dec``, ``hex``, ``showbase``, ``noshowbase``

        .. code-block:: cpp

           cout << 123 << '\t' << hex << 1234 << '\t' << oct << 1234; // output: 123 4d2 2322


        - ``<< hex`` and ``<< oct`` informs any further integer outputs to be hex or oct
        - are called manipulators and are sticky until output format is changed

        .. code-block:: cpp

           cout << 123 << '\t' << hex << 1234 << "\thello\t" << 1234; // output: 123 4d2 hello 4d2
   
           // changing output back to decimal
           cout << hex << 1234 << '\t'<< dec << "hello\t" << 1234; // output: 4d2 hello 1234


        - can ask the ``ostream`` to show the base of each integer
        - decimals have no prefix, hexas have 0x and octals have 0 as prefix

        .. code-block:: cpp

           cout << showbase << 1234 << '\t' << hex << 1234 << '\t' << oct << 1234;
           // 1234 0x4d2 02322
           // showbase manipulator persists
   
           cout << '\t' << 1234 << '\t' << noshowbase << 1234; // 02322 2322


    * by default, ``>>`` assumes numbers use decimal notation
    * can tell it to read various formats and input manipulators also stick

        .. code-block:: cpp

           cin >> a >> hex >> b >> oct >> c >> d; // 1234 4d22 2322 2322
           cout << a << '\t' << b << '\t' << c << '\t' << d; // 1234 1234 1234 1234


        - can tell ``>>`` to accept prefixes
        - stream member function ``unsetf()`` clears the flags

        .. code-block:: cpp

           cin.unsetf(ios::dec);
           cin.unsetf(ios::oct);
           cin.unsetf(ios::hex);
   
           cin >> a >> b >> c >> d; // 1234 0x4d2 02322 02322
           cout << a << '\t' << b << '\t' << c << '\t' << d; // 1234 1234 1234 1234


    * **floating point manipulators**: ``fixed``, ``scientific``, ``defaultfloat``, ``setprecision()``
    * they also stick

        .. code-block:: cpp

           cout << 1234.56789 << '\t' << fixed << 1234.56789 << '\t' << scientific << 1234.56789
                << '\t' << 1234.56789;
           // 1234.57 1234.567890 1.2345678e+03 1.2345678e+03


        - by default, float values are printed using six total digits with defaultfloat
        - number is rounded for best approximation to be printed with six digits
        - floating-point format only applies to floating-point numbers

        .. code-block:: cpp

           cout << scientific << 1234.56789 << '\t' << 123456789 << '\t' << 1234567.0;
           // 1.2345678e+03 123456789 1.234567e+06
           // 1234567.0 prints in scientific because fix format cannot be used to be accurate
   
           cout << defaultfloat << 1234.56789 << 1234567.0;
           // 1234.57 1.23457e+06
           // defaultfloat chooses between scientific and fixed to present the most accurate


        - can set the precision with ``setprecision()``

        .. code-block:: cpp

           #include <iomanip>
           cout << 1234.56789 << '\t' << setprecision(8) << 1234.56789;
           // 1234.57 1234.5679


    * **fields** for integers: ``setw()``
        - using scientific and fixed formats, how much space a value takes up on output by
        floating-point numbers can be controlled, which is useful for printing table
        - same thing can be done for integers with *fields*
        - can specify exactly how many character positions an integer value or string value
        will occupy
        - field sizes don't stick

        .. code-block:: cpp

           cout << 12345 << '|' << setw(4) << 12345 << '|' << setw(8) << 12345;
           // 12345|12345|   12345
           // normal|doesn't fit in 4 field| three spaces in front
           // numbers will not be truncated to fit


        - bad formatting is almost always same as bad output data
        - overflows are noticeable and can be corrected
        - fields can also be used for floating-point and strings

        .. code-block:: cpp

           cout << 12345 << '|' << setw(8) << 12345 << '|' << "asdfg";
           // 12345| 12345.6|   asdfg



Operators
---------
    * **arithmetic**
        - +, -, &ast;, /, %, ++, --
        - modulo operator (%) cannot operate on floats, so use ``fmod()`` from ``<cmath>``
    * **relational**
        - ==, !=, >, <, >=, <=
    * **logical**
        - &&, ||, !
    * **bitwise**
        - perform bit-by-bit operation
        - &, |, ^ (XOR), ~ (complement), <<, >>
    * **assignment**
        - =, +=, -=, *=, /=, %=, <<=, >>=, &=, ^=, |=
    * **misc**
        - sizeof, conditional, comma, member (. & ->), cast, address (&), indirection (*)
        - ``sizeof()`` can be used on type name or expression
           + for type, gives size of an object
           + for expression, gives size of the type
           + gives number of bytes
           + size of a type can be different on various implementation of C++
    * precedence
        - left to right
           + postfix, multiplicative, additive, shift, relational, equality, bitwise (AND, OR,
           XOR), logical (AND, OR), comma
        - right to left
           + unary, conditional, assignment
    * ``*=`` & ``/=`` are referred to as scaling in many application domains
    * if an operator has an operand type of ``double``, floating-point arithmetic is used

Literals
--------
    * **integer**
        - prefix specifies the base or radix
           + 0x or 0X for hexa
           + 0 for octal
           + nothing for decimal
        - can have suffix combination of U/u and L/l for unsigned and long

        .. code-block:: cpp

           85 //decimal
           0213 // octal
           0x4b // hexa
           30 // int
           30u // unsigned int
           30l // long
           30UL // unsigned long


    * **float**
        - has int part, decimal point, fractional part and exponent part
        - can represent in decimal or exponential form

        .. code-block:: cpp

           3.141
           3141E-5L


    * **boolean**: true & false, should not consider as 1 or 0
    * **character**
        - enclosed in single quote
        - wide character begins with L and should be stored in ``wchar_t``
        - on a PC, range of char values is [-128:127], but only [0:127]l can be used portably
        as not every computer is a PC and different computers have different ranges, [0:255]
        - special meaning chars
           + \a       alert or bell
           + \b       backspace
           + \f       form feed
           + \r       carriage return
           + \t       horizontal tab
           + \v       vertical tab
           + \ooo     octal number of one to three digits
           + \xhh...  hexa number of one or more digits
    * **string**
        - enclosed in double quotes
        - representation of a string is a bit more complicated than that of an int as a string
        keeps track of the number of characters it holds
        - functions
           + strcpy, strcat, strlen, strcmp, strchr, strstr

        .. code-block:: cpp

           // C-style
           char myStr[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
           char myStr[] = "Hello"
   
           // string class
           string myStr = "Hello";
   
           // can break long lines
           "hello, \
           world"
   
           // adjacent string literals are concatenated by the compiler
           "line 1     "
           "still line 1"


    * **defining constants**
        - using **#define** preprocessor
        - using **const** keyword

        .. code-block:: cpp

           #define LINE 10
           const int LINE = 10;



Type Safety
-----------
    * when objects are used only according to the rules for their type
    * using a variable before it has been initialized is no type-safe
    * C++ compiler cannot guarantee complete type safety
    * **type conversions** are safe when no information is lost
        - bool to char
        - bool to int
        - bool to double
        - char to int
        - char to double
        - int to double
    * for really large int, some computers suffer loss of precision when converting to double
    * **unsafe/narrowing conversions** can implicitly turn a value to another type that does not
    equal the original value
    * accept by compiler even though unsafe
        - double to int
        - double to char
        - double to bool
        - int to char
        - int to bool
        - char to bool
    * universal and uniform initialization: {}-list-based notation to avoid accidents

        .. code-block:: cpp

           double x{2.7};
           int y{x}; // error: double->int might narrow


    * use explicit conversion if possible

        .. code-block:: cpp

           void g(double x)
           {
               int x1 = x;
               int x2 = int(x);
               int x3 = static_cast<int>(x);
           }


* a program usually contains some data structures or states
* arguments: inputs to a part of a program
* results: outputs from a part of a program
* breaking up big computation into smaller ones
    * **Abstraction**
        - programming and design technique that relies on separation of interface and implementation
        - use access labels to define abstract interface to classes (public, private)
        - class internals are protected from user-level errors, which might corrupt the state of object
        - class implementation may evolve without requiring change in user-level code
        - interface must be kept independent of the implementation
    * **Divide & Conquer**
        - divide larger problem into several little ones
        - each of the resulting problems is significantly smaller than the original
* ugly code is hard to read and hard to correct as it often hides logical errors
* always test programs with bad input
* don't demonstrate cleverness by writing the most complex program, demonstrate competence by
writing simplest code that does the job

typedef
-------
    * create new name for existing type

    .. code-block:: cpp

       typedef int hello;
       hello distance; // hello is of type int



Switch
------
    * values must be of an int, char or enumeration, cannot switch string
    * values in case labels must be constant expression, cannot use variable
    * cannot use same value for two ``case`` labels
    * can use several ``case`` labels for a single case
    * always end each ``case`` with ``break``, compiler will not warn if forgotten

Loops
-----
    * **while**

        .. code-block:: cpp

           int i{0};
           while (i<100) { ++i; }


        - the first program ever to run on a stored-program computer (the EDSAC), written and
        run by David Wheeler, was simple iteration of square numbers
        - loop variable must be defined and initialized outside (before) the statement
    * **for**

        .. code-block:: cpp

           for (int i=0; i<100;++i){}


        - never modify the loop variable inside the body of statement
        - conditional expression is assumed to be true if absent

        .. code-block:: cpp

           for(;;;)


    * range-for-loop

        .. code-block:: cpp

           for(int x:v) {} // for each int x in v



Functions
---------
    * parameter list: list of arguments required by the function
    * parameter names are not important in declaration, supplying information separate from
    the complete function definition
    * declaration is required to be called from another file

        .. code-block:: cpp

           int myFunc(int, int);


    * give ``void`` as return type to return nothing

        .. code-block:: cpp

           void noReturnFunction() {}


    * falling through the end of function
        - not returning a value as declared
        - only ``main()`` is special case as falling through is equal to ``return 0;``
    * acceptable to drop through bottom of ``void`` function which is same as ``return;``
    * programs are easier to write and understand if each function performs a single logical
    action
    * standard library provides ``swap()`` for every type that can be copied
    * **call/pass by value** (default)
        - copy the actual value of argument into formal parameter
        - argument is not affected
        - cost of copying the value
    * **call/pass by pointer**

        .. code-block:: cpp

           int myfunc(int *a, int *b);


        - copy the address of argument into formal parameter
        - argument is affected
    * **call/pass by reference**

        .. code-block:: cpp

           int myfunc(int &a, int &b);


        - copy the reference/address of argument into formal parameter
        - argument is affected

        .. code-block:: cpp

           vector<vector<double>>v;
           double& vector = v[f(x)][g(y)];
           var = var/2+sqrt(var);


        - references make both reading and writing of same element easy without repetition
    * **call/pass by const reference**

        .. code-block:: cpp

           int myfunc(const int &a);


        - ``const`` stops the argument being modified
    * pointer can be reassigned, reference must be bound at initialization and cannot be rebound
    * by-value vs by-reference
        - use non-const reference to change
        - by-value gives a copy
        - by-const-reference prevents from changing the value of the object
    * const reference doesn't need an lvalue

        .. code-block:: cpp

           void g(int a, int& r, const int& cr);
           g(x, y, z)
           g(1, 2, 3); // error, int& r needs variable
           g(1, y, 3); // OK

        - since cr is const, literal can be passed
        - compiler sets aside an int for cr to refer to
        - temporary: compiler-generated object
    * by-value for small objects, by-const-reference for non-modify large object, by-reference
    only when needed
    * return a result rather than modifying through a reference argument
    * non-const-reference are essential for manipulating containers, other large objects and
    for functions that change several objects as functions can have only one return value
    * best to avoid functions that modify several objects
    * recursive: function that directly or indirectly calls itself
    * function activation record
        - data structure containing a copy of called function's parameters and local variables
        - each function has its record
        - from implementation's point of view, a parameter is just another local variable
        - run-time cost of making function activation record doesn't depend on how big it is
        - record stack grows by one each time the function is called
        - the record is no longer used when the function returns
    * **constexpr functions**

        .. code-block:: cpp

           const int x = 1;
           constexpr test(int i)
           {
               return x + i;
           }


        - to avoid doing same calculation many times
        - evaluated by the compiler if given constant expressions as arguments
        - must be simple for compiler to evaluate, otherwise error
        - must have a body of single return
        - may not change the value of variables outside its body
    * do not return a pointer to a local variable
* characters that are not ordinary: Ctrl+Z (Windows), Ctrl+D (Unix) terminates an input stream

Compile-time errors
-------------------
    * found by compiler, which is the first line of defense against errors
    * **Syntax errors**
        - not always easy to be reported in understandable way by the compiler
    * **Type errors**
        - mismatches between the declared types
        - every function call must provide the expected number of arguments

Link-time errors
----------------
    * found by the linker when trying to combine object files into executable
    * every function must be declared with the same type in every translation unit (compiled
    parts)
    * every function must be defined exactly once
    * functions with the same name but different types will not match and will be ignored
    * misspelled function name doesn't usually give a linker error, but compiler does
    * compile-time errors are found earlier than link-time and easier to fix
    * exactly one definition of an entity but can be many declarations

Run-time errors
---------------
    * found by checks in a running program
    * detected by the computer, a library or user code
    * called function, the callee, must check its own arguments as checking can be in one place
    * but checking in function isn't always done when
        - cannot modify the function definition (using from library)
        - called function doesn't know what to do in case of error (library writer can detect
        errors, but only user know what to do with them)
        - called function doesn't know where it was called from
        - performance cost of a check can be more than the cost of calculating the result
    * letting the called function send errors and the caller handling them can have problems
        - both called function and caller must do tests
        - caller can forget to test
        - many functions do not have an extra return value to indicated an error

Logic errors
------------
    * found by the programmer
    * usually the most difficult to find and fix
    * check, estimate, that the result is plausible as it is not easy to know what is reasonable
* Sources of errors
    * poor or incomplete program specifications
    * unexpected arguments, input or state (data)
    * logical errors
* ``std::exception`` (provides ``what()`` method)
    * ``std::bad_alloc`` (can be thrown by new)
    * ``std::bad_cast`` (can be thrown by dynamic_cast)
    * ``std::bad_typeid`` (can be thrown by typeid)
    * ``std::bad_exception`` (useful to handle unexpected exceptions)
    * ``std::logic_error``
        - ``std::domain_error`` (mathematically invalid domain used)
        - ``std::invalid_argument``
        - ``std::length_error``
        - ``std::out_of_range`` (off-by-one error, bounds error)
           + subscript operation of vector knows its size and will check, if the check fails,
           the subscript operation throws 'out_of_range' exception
    * ``std::runtime_error``
        - ``std::overflow_error``
        - ``std::underflow_error``
        - ``std::range_error``
        - holds a string that can be used by an error handler
        - simply catch it to deal with, catching in ``main()`` is ideal for simple programs

Exceptions
----------
    * containers: collections of data
    * ``try``: followed by one or more catch blocks
    * ``catch``: catch exception with an exception handler
    * ``throw``: throws an exception when problem shows up
    * ``throw MyClass{}``: make an object of type 'MyClass' with default values and throw it
    * protected code: code within try/catch block

        .. code-block:: cpp

           try {
               //protected code
           }
           catch (ExceptionName e) {
               //code to handle exception
           }
           catch (...) {
               // catch any type of exception
           }


    * ``narrow_cast<int>(2.9)``
        - throws runtime_error exception as the type changes
        - 'cast' means 'type conversion'
        - 'cast' doesn't change its operand, but produces new value specified in '<...>'

Debugging
---------
    * need to know if the program actually worked correctly
    * complicated code is where bugs can most easily hide
    * add invariants, conditions that should always hold, in code sections suspected of bugs
    * assertion/assert: statement that states an invariant
    * pre-condition: requirement of a function upon its argument, always consider if a quick
    check of pre-conditions can be written
    * post-condition: what to do if pre-condition is violated, both provide sanity checks
* Testing: a run with a given set of inputs
* Terminology for **Program Development** <a id = "program-development"></a>
    * analysis: figure out a set of requirements or specification
    * design: create overall structure
    * implementation: write, debug and test the code
* only a combination of analysis and experimentation (design & implement) gives the solid
understanding to write a good program
* prototype: limited initial version aimed at experimentation
* grow a program from working parts rather than writing all at once
* token
    * sequence of characters that represents a unit
    * representing each token as a (kind, value) pair is conventional
    * parsing: reading a stream of tokens according to a grammar, which is done by parser or
    syntax analyzer
* for programs that accept user input, write a grammar defining the syntax of input and then
write a program that implements the rules of that grammar
* writing simple grammar
    * distinguish a rule from token
    * put one rule after another (sequencing)
    * express alternative patterns (alternation)
    * express a repeating pattern (repetition)
    * recognize the grammar rule to start with
* some call tokens terminals and rules non-terminals or productions
* it is not ideal to throw away input without determining what it is
* order of declaration is important, cannot use a name before it has been declared
* symbol table
    * mechanism to keep track of variables
    * can use ``map`` from standard library
* ``isalpha()``: check character for alphabetical letter ( ``isalpha('1')`` is false)
* ``isdigit()``: check character for digit
* interpreters: programs that immediately executes the expressions it has analyzed

Program Evaluation
------------------
    * variable is constructed when the execution reaches the definition, and destroyed
    when it goes out of scope
        - ``static`` local variable is initialized only the first time its function is called
    * global variables are constructed in the order in which they are defined and destroyed in
    reverse order in single translation unit
        - do not use global variables in everything
        - in different translation units, order of initialization may be different
    * compilers only allocate and deallocate memory necessary amount
    * never access the value of variable in an expression twice
        - ``v[i] = ++i``
        - not all compilers warn about the bad code and different compilers result different
         values
        - left or right hand side of the assignment may be evaluated first

Namespaces
----------
    * used as additional info to differentiate similar functions, classes, variables
    * defines a scope
    * fully qualified name: has namespace/class name and member name

        .. code-block:: cpp

           namespace first_space {
               int x = 1;
           }
   
           namespace second_space {
               int x = 2;
           }
   
           int main()
           {
               first_space::x; // fully qualified name
               second_space::x;
           }


    * ``using`` directive
        - avoid prepending of namespaces, tells the compiler that subsequent code is making
        use of names specified in namespace
        - no declaration for a name in the scope means it is likely to be in ``std``

        .. code-block:: cpp

           using namespace first_space;
           int main()
           {
               x; // use x from the first_space
           }


        - can be used to refer to particular item within a namespace

        .. code-block:: cpp

           using std::cout;
           int main()
           {
               cout << "hello" << std::endl;
           }


        - names introduced obey normal scope rules, entities with the same name defined in
        outer scope are hidden
        - good idea to avoid ``using`` for namespaces except for well known ones such as ``std``
        - may lose track to which names come from where
        - putting ``using`` in header file is bad habit
    * separate parts of a namespace can be spread over multiple files
    * namespaces can be nested
        - access members of nested by using resolution operators

        .. code-block:: cpp

           namespace first_space {
               int x = 1;
               namespace second_space {
                   int x = 2;
               }
           }
           using namespace first_space::second_space;
           int main()
           {
               x; // x from second_space
           }

* two kinds of user-defined types: classes & enumerations

Struct
------
    * class where members are public by default
    * primarily used for data structures where members can take any value

    .. code-block:: cpp

       struct Books {
           char title[50];
           char author[50];
       } book;


    * access with member access operator (.)

        .. code-block:: cpp

           struct Books Book1;
           strcpy(Book1.title, "Hello World");


    * can be passed as a function argument
    * pointers to structures

        .. code-block:: cpp

           struct Books *struct_pointer;
           struct_pointer = &Book1;
           struct_pointer->title;


    * can use with typedef

        .. code-block:: cpp

           typedef struct {
               char title[50];
               char author[50];
           } Books;
           Books Book1, Book2;



Enumerations
------------
    * simple user-defined type
    * declares an optional type name and a set of zero or more identifiers
    * each enumerator is a constant whose type is enumeration
    * by default, the value of first is 0, second is 1 and so on but can give a name value by
    adding an initializer
    * body of enumeration is a list of enumerators
    * ``class`` in ``enum class`` means that the enumerators are in the scope of enumeration and
    their values do not implicitly convert to other types

        .. code-block:: cpp

           enum class color { red, green, blue};
           Color c = Color::blue;
           int num = c; // error


    * ``enum class`` should be preferred
    * cannot define constructor for an enumeration to check initializer values
    * useful when a set of related named integer constants is needed
    * in plain ``enums``, enumerators are in the same scope as the enum and their values
    implicitly convert to integers and other types
        - less strict than ``enum classes``
        - can pollute the scope in which their enumerator is defined

        .. code-block:: cpp

           enum Color { red , green = 3, blue}; // blue will have value 4 as each will be one greater
           Color c = Color::blue; // c is of type color
           int num = c; // OK



Overloading
-----------
    * specifying more than one definition for function name or operator in the same scope
    * both declarations have different arguments and definition
    * overload resolution
        - compiler determines the most appropriate definition to use by comparing argument types used
        to call the function with parameter types specified in the definitions
    * **operator overloading**
        - functions starting with ``operator``
        - can define as ordinary non-member functions or as class member functions
        - overloaded operator must have at least one user-defined type as operand
        - define operators only with their conventional meaning
        - most interesting operators to overload (=, ==, !=, <, [] subscript, () call)

        .. code-block:: cpp

           MyClass operator+(const MyClass& c)
           {
               MyClass d;
               d.x = this->x + c.x;
               return d;
           }

        - **cannot overload** (::, .* , ., ?:, sizeof, typeid)
        - need to make I/O operator overloading a friend of the class as it would be called
        without creating an object

        .. code-block:: cpp

           friend ostream &operator<<(osstream& output, const MyClass& c)
           {
               output << c.x;
               return output;
           }
   
           friend istream &operator>>(istream& input, MyClass& c)
           {
               input >> c.x;
               return input
           }


        - overloading function call operator () is not creating new way to call a function,
        it's creating operator function that can be passed an arbitrary number of parameters

        .. code-block:: cpp

           MyClass operator()(int a, int b, int c)
           {
               MyClass C;
               C.x = a + b + c;
               return C;
           }
   
           MyClass Class1;
           MyClass Class2;
           Class2 = Class1(2, 2, 2,); // will call overloaded operator


        - in overloading class member operator (->), operator -> must be a member function and
        return type must be a pointer or an object of a class to which it can be applied
    * **function overloading**
        - can have multiple definitions for same function name in the same scope
        - definition of functions must differ by the types and/or number of arguments
        - cannot differ only return type
* separate how the program reads and writes from actual input and output devices
* directly addressing each kind of device will need to change the program for a new
screen or disk every time or limit users to certain screens and disks
* most modern OS separate the detailed handling of I/O devices into device drivers
* different kinds of I/O
    * streams of data items (files, network, display devices)
    * user interacting with keyboard or through GUI

Lambda Expressions
------------------
    * unnamed function defined as an argument
    * lambda introducer (``[]``), followed by argument list and function body
    * return type can be deduced from function body or specified explicitly
    * introducer can be used to gain access to local variables
    * lambda expressions should be kept simple, and only use for functions that fit on a line
    or two

    .. code-block:: cpp

       auto myLambda = [](int x) {return 2 * x};
   
       // specify return type
       auto myLambda = [](int x) -> int {return 2 * x};
   
       // access local variable
       int y = 2;
       auto myLambda = [y](int x) {return y * x};



buffer
------
    * data structure that ostream uses internally to store data given by user to OS
    * delay between ostream and characters appearing is usually because they are still in
    buffer
    * buffering is important for performance
    * istream uses buffer to communicate with the OS
    * with istream, buffering can be quite visible to the user

Files
-----
    * ``#include <fstream>``
    * a file is sequence of bytes numbered from 0 upward
    * file format has same role for files on disk as types for objects in main memory
    * ostream converts objects in main memory into streams of bytes and writes them to disk
    * istream takes a stream of bytes from disk and composes objects from them
    * ``ofstream``: data type for output file stream, ostream for writing to a file
    * ``ifstream``: data type for input file stream, istream for reading from a file
    * ``fstream``: type for general file stream, both ofstream and ifstream
    * file must be opened before read or write

        .. code-block:: cpp

           ifstream ist {filename}; // open file for reading
           ofstream ost {filename}; // open file for writing


    * when a file stream goes out of scope, associated file is closed and buffer if flushed
    * best to open files early in program before any computation
    * relying on scope minimizes the chances of file stream being used before attach
    * explicit open and close
        - all members are in ``std::ios_base``
        - ``ios::app``: append, useful for writing logs
        - ``ios::ate``: open file for output and move rw control to the end of the file
        - ``ios::binary``: binary mode, can have system-specific behavior
        - ``ios::in``: open for reading
        - ``ios::out``: open for writing
        - ``ios::trunc``: truncate contents before open if file already exists

        .. code-block:: cpp

           ifstream ifs;
           ifs.open(filename, ios_base::in);
           void open(const char* filename, ios::openmode mode);
   
           ofstream ofs {filename, ios::app};


    * can combine multiple modes

        .. code-block:: cpp

           ofstream outfile;
           // open in write mode and truncate if already exists
           outfile.open("file.dat", ios::out | ios::trunc);
           // open for read and write
           outfile.open("file.dat", ios::out | ios::in);


    * when using character representation, some character must be used to represent end of
    number in memory
    * distinction between storing fixed-size binary representation and variable-size character
    string representation also occurs in files
    * it is possible to request ``istream`` and ``ostream`` to copy bytes to and from files with
    ``ios::binary``

        .. code-block:: cpp

           template<class T> char* as_bytes(T& i)
           {
               void* addr = &i;
               return static_cast<char*>(addr);
           }
   
           int main()
           {
               ifstream ifs{"inputFile", ios::binary};
               ofstream ofs{"outputFile", ios::binary};
               vector<int> v;
               for(int x; ifs.read(as_bytes(x), sizeof(int));) v.push_back(x);
               for(int x: v) ofs.write(as_bytes(x), sizeof(int));
               return 0;
           }

    * when moving from character-oriented I/O to binary I/O, ``>>`` and ``<<`` must be given up
    as they turn values into character sequences
    * ``as_bytes()`` is needed to get the address of the first byte of an object's representation
    * default character I/O is portable, human readable and supported by type system
    * don't mess with binary I/O unless really needed
    * cannot open a file stream second time without closing first
    * program auto flushes all streams, release all allocated memory and close all files, but
    good practice to close all opened files before termination

        .. code-block:: cpp

           // member of fstream, ifstream, ofstream
           void close();


    * most common reason for failure to open a file for reading is that it doesn't exist
    * OS will create new file if nonexistent file is opened for output, will not for input
    * stick to reading from files opened as ``istreams`` and writing to files opened as ``ostreams``
    * istream states
        - ``good()``, ``eof()``, ``fail()``, ``bad()``
        - difference between fail and bad is not precisely defined
        - a stream that is ``bad()`` is also ``fail()``
        - when a stream fails, it can be recovered by taking it out of the ``fail()`` state with
        ``clear()``
        - stream state can be set back to ``fail()`` with ``ist.clear(ios_base::failbit)``, which
        sets ``iostream`` state to flags mentioned but clear flags that are not
        - character can be put back into ``ist`` using ``unget()``
        - getting more data from ``bad()`` state is unlikely, but to throw an exception with
        ``ist.exceptions(ist.exceptions()|ios_base::badbit)``
    * ``iostream`` can handle different character sets, implement different buffering strategies,
    and contain facilities for formatting monetary amounts in various languages
    * usually errors are much rarer for output than for input, only test each output operation
    of `ostream` if output devices have more chance of being unavailable or broken
    * use insertion operator (<<) to write to file, ofstream or fstream object instead of cout
    * use extraction operator (>>) to read file, ifstream or fstream object instead of cin
    * problems when reading
        - user typing out-of-range value
        - getting no value (eof)
        - user typing wrong type
    * terminators are useful when reading files with nested constructs
    * every file opened for reading has a read/get position and files for writing has write/put
    position
    * file position pointers
        - integer value that specifies the location in the file as a number of bytes from start
        - ``seekg``: seek get for istream
        - ``seekp``: seek put for ostream
        - both have argument of long int
        - seek directions: ``ios::beg`` (default), ``ios::cur``, ``ios::end``

        .. code-block:: cpp

           fileObject.seekg(n, ios::end);


    * there is next to no run-time error checking when positioning is used
    * undefined when seeking beyond end of a file and operating systems differ what happen
    * ``istringstream`` & ``ostringstream``
        - string can be used as the source of ``istream`` or ``ostream``
        - ``istringstream`` is useful for extracting numeric values from a string
        - will go into ``eof()`` state if read beyond the end of ``istringstream`` string
        - ``ostringstream`` can be useful for formatting output for a system that requires
        simple string argument such as GUI system

        .. code-block:: cpp

           istringstream is {s};
           ostringstream os;
           os.str().c_str();


        - ``str()`` member function of ``ostringstream`` returns the string composed by output
        operations to an ``ostringstream``
        - ``c_str()`` is member of ``string`` that returns C-style string
        - stringstreams are used to separate actual I/O from processing, usually to filter
        characters out of input
        - can use ``ostringstream`` to concatenate strings
    * ``istream`` library provides to read individual characters and whole lines

        .. code-block:: cpp

           string name;
           getline(cin, name);


        - usually parse the line after entered
        - reading individual characters gives full control
        - ``get()`` does not skip whitespace and returns a reference to ``istream`` like ``>>``
    * standard library functions for character classification
        - ``isspace()``, ``isalpha()``, ``isdigit()``, ``isxdigit()``, ``isupper()``, ``islower()``
        - ``isalnum()``, ``iscntrl()``, ``ispunct()``, ``isprint()``, ``isgraph()``
        - ``toupper()``, ``tolower(c)``
        - can be combined using ||
        - ``isalnum()``: ``isalpha()||isdigit()``
    * standard library ``iostream`` rely on concept called ``streambuf``

Date & Time
-----------
    * inherits date/time functions from C **<ctime>**
    * time-related types
        * clock_t, time_t, size_t
            - represent as integer
        * tm
            - in the form of C structure
            - tm_sec/min/hour/mday/mon/year/wday/yday/isdst

    .. code-block:: cpp

       time_t time(time_t *time); // time in seconds since Jan 1, 1970
       char *ctime(const time_t *time); // pointer to string of form 'day month year hr:min:s'
       struct tm *localtime(const time_t *time); // pointer to tm structure
       clock_t clock(void); // approx of running program time
       char *asctime(const struct tm *time); /* pointer to string with info stored in structure
                                               pointed to by time converted in the form
                                               'day month date hr:min:s year\n\0' */
       struct tm *gmtime(const time_t *time); // pointer to time in tm structure
       time_t mktime(struct tm *time); /* calendar-time equivalent of the time found in the structure
                                       pointed by time */
       double difftime(time_t time2, time_t time1); // calculates difference in seconds
       size_t strftime(); // use to format date and time


`back to top <#c>`_

Built-in functions
==================


<cmath>
-------
    * cos, sin, tan, log, pow, hypot, sqrt, abs, fabs, floor

<cstdlib>
---------
    * srand, rand

`back to top <#c>`_

Arrays
======

* consist of contiguous memory locations

    .. code-block:: cpp

       double myArr[100];
       int test[] = {1, 2, 3}; // will have initialized size
       int x[5] = {0}; // all elements will be 0
       int x[5] = {1}; // only first element will be 1 and others 0
   
       // initialize all elements to 0
       #include <cstring>
       int x[5];
       memset(x, 0, sizeof(x));
   
       // access by index
       x[2]; // third element
   
       // sometimes declared with extra memory for out of bounds error protection
       int x[n + 5];
   
       // int x[ROW][COLUMN], two dimensional array
       int x[3][4]; // 3 rows, 4 columns
       int x[3][4] = {{0}}; // initialized all elements to 0


* cannot use more than size limit of 10^8
* cannot return entire array from function, but can return a pointer to an array
    * have to define local variable as ``static`` to return address of local to outside function

    .. code-block:: cpp

       int * returnArray() {
           static int x[3];
           return x;
       }


`back to top <#c>`_

Pointers
========

* `Free Store`_, `NULL Pointer`_, `Void Pointer`_, `Explicit Type Conversion`_
* an object which holds address value of another
* 'address of' operator, unary ``&``, is used to get the address of an object
* 'contents of/dereference' operator, unary ``*``, is used to get the value of the object pointed to
* most pointer values/addresses use hexadecimal notation
* can do arithmetic operations and comparisons
* dereference operator can also be used for left-hand assignment

    .. code-block:: cpp

       int x = 10;
       int* p_x= &x;
       std::cout << *p_x; // get value pointed to by p_x, value of x
       *p_x = 9; // assign new value to the value pointed to by p_x, 'x' becomes 9


* pointer is not integer
    * pointer type provides operations for addresses
    * int provides operations for integers, arithmetic and logical
    * pointers and integers implicitly do not mix

    .. code-block:: cpp

       int* pi = &x;
       int i = pi; // error
       pi = 2; // error


* pointer to char, ``char*``, is not same as pointer to int, ``int*``

    .. code-block:: cpp

       int* pi = &i;
       char* pc = pi; // error
       pi = pc; // error


* if assigning different types of pointers is allowed, memory locations could be changed since
each type has different memory sizes
* pointer pointing to the start of array can access it by pointer arithmetic or array-style
indexing
* memory set aside by compiler
    * code/text storage: for the code
    * static storage: for global variables
    * stack/automatic storage: for functions, arguments and local variables

Free Store
----------
    * memory not touched by the compiler, also called the heap
    * can allocate in the heap with ``new``
    * ``new`` operator returns a pointer to the object it creates
    * can allocate individual elements or arrays

        .. code-block:: cpp

           int* p = new int[4]; // allocate 4 ints on the free store
           int x = *p; // first object pointed by p
           int y = ptr[1]; // second object pointed by p
           int z = *&p[2]; // third object pointed by p
   
           int* p1 = new int[4] {1, 2, 3, 4}; // allocate array on the free store and initialized it
           int* p1 = new int[] {1, 2, 3, 4}; // can omit number of elements when initialized


    * always return memory to free store after using
        - essential for long-running programs such as operating systems, embedded systems
        and libraries

        .. code-block:: cpp

           delete[] p; // free array of objects
           delete p; // free individual object


    * careful not to delete an object twice
        - deleting null pointer is harmless

        .. code-block:: cpp

           delete p; // first time deletion
           delete p; // error: p points to memory owned by free-store manager
   
           int* np = nullptr;
           delete np; // ok
           delete np; // ok


    * automatic garbage collection
        - recycle/free memory not needed without human intervention
        - can be costly and not ideal for all applications
    * programs under operating systems auto return memory to the system at the end
    * allow memory leak only when program will not use memory more than available and memory
    consumption estimate for a program should be correct
* a pointer doesn't know how many elements it points to
    * out-of-range access, transient bugs, can affect unrelated parts of the program and are
    hard to find
* problems in some C-style programs are caused by access through uninitialized pointers and
out-of-range access
* optimizer, compiling on different machine or turning off debug features can cause a program
with uninitialized variables to run differently

NULL Pointer
------------
    * memory at address 0 is reserved by the OS
    * signals that the pointer is not intended to point to an accessible memory
    * use when no pointer to use for when initializing a pointer
    * can avoid accidental misuse of uninitialized pointer

    .. code-block:: cpp

       int* p0 = nullptr; // value zero is called when assigned to a pointer
   
       // before C++11
       int* p0 = 0;
       int* p0 = NULL;


* pointer to a pointer

    .. code-block:: cpp

       int *ptr;
       int **pptr;
       ptr = &x;
       pptr = &ptr;



Void Pointer
------------
    * pointer to memory that the compiler doesn't know the type of
    * use to transmit address between code that don't know each other's types, e.g address
    arguments of a callback function
    * can assign to pointer to any object type
    * ``static_cast`` can be used to convert between related pointer types, but use only when
    necessary

    .. code-block:: cpp

       void* pv1= new int; // ok
       void* pv2= new double[10]; // ok
   
   
       pv2 = pv1; // ok
       double* pd = pv1; // error
       pv[2] = 9; // error
       *pv1 = 2; // error: cannot dereference a void*
   
       int* pi = static_cast<int*>(pv1); // ok, also called explicit conversion


Explicit Type Conversion
------------------------
    * use only if really necessary
    * ``static_cast``
        - to convert between related pointer types
    * ``reinterpret_cast``
        - to convert one pointer type to another
        - not easily portable
    * ``const_cast``
        - cast away const
    * prefer ``static_cast`` if needed

    .. code-block:: cpp

       Register* in = reinterpret_cast<Register*> (0xff); // necessary when writing device drivers
   
       void f(const Buffer* p)
       {
           Buffer* b = const_cast<Buffer*> (p); // strip const from const Buffer*
       }


* assignment to pointer changes the pointer's value, not the pointed-to
* need to use ``new`` or ``&`` to get a pointer
* need to use ``*`` or ``[]`` to access an object pointed to
* assignment of pointers doesn't do deep copy, unlike references
    * assigns to the pointer object itself
* reference and pointer are both implemented by using a memory address
* using a pointer argument alerts the programmer something might be changed
* when to use
    * pass-by-value for tiny objects
    * use pointer parameter for functions where ``nullptr`` is a valid argument
    * use a reference in other cases
* pointer fiddling is tedious and error-prone, should be hidden in well-written and tested
functions

`back to top <#c>`_

References
==========

* an alias, cannot have NULL, cannot be changed to refer to another object, must be initialized
when created

.. code-block:: cpp

   int i = 1;
   int& r = i;


* usually used for function argument lists and function return values
* return by reference
    * returns an implicit pointer to return value
    * function can be used on the left side of an assignment

    .. code-block:: cpp

       int x[] = {1, 2, 3};
       int& setValue(int i) {
           return x[i]
       }
   
       // function on the left side of assignment
       setValue(1) = 10; // x = {1, 10, 3}


    * cannot return a reference to local var
* assignment to a reference changes the value of the object referred to
* cannot make a reference refer to a different object after initialization
* assignment of references does deep copy, unlike pointers
    * assigns to the referred-to object
* reference and pointer are both implemented by using a memory address
* when to use
    * pass-by-value for tiny objects
    * use pointer parameter for functions where ``nullptr`` is a valid argument
    * use a reference in other cases

`back to top <#c>`_

Classes
=======

* `Access Specifiers`_, `Member Functions`_, `Helper Functions`_
* `Constructor`_, `Destructor`_, `Copy Constructor`_
* `Const Functions`_, `Friend Functions`_, `Inline Functions`_, `this pointer`_, `Static Members`_
* used to specify the form of an object
* data and functions, parts used to define the class, are members of the class
* has zero or more members
* class definition states what the class can do
* access members using the ``object.member`` notation or ``object->member`` if given a pointer to
the object

.. code-block:: cpp

   class MyClass {
       public:
           int x;
           string s;
   };
   
   MyClass Class1;
   Class1.x = 1;
   Class1.s = "hello";


* keep classes complete and minimal
    * provide constructors
    * support copying or prohibit it
    * use types to provide good argument checking
    * identify non-modifying member functions
    * free all resources in the destructor
* implementation: part of the class that users access only indirectly

Access Specifiers
-----------------
    * default is private
    * a class can have multiple labeled sections
    * **public**
       - can set and get public variables without member functions
       - can be used by all functions
    * **private**
       - cannot be accessed from outside the class
       - only the class and friend functions can access members
    * **protected**
       - can be accessed in child classes
* private and protected members cannot be accessed directly with direct member access
operator

Member Functions
----------------
    - has definition or prototype within the class definition
    - can be defined within the class or separately using scope resolution operator (::)
    - function definitions are implementations that specify how things are done, and
    usually specified outside the class so that they don't distract
    - within member function, a member name refers to the member of that name in the
    object for which the member function was called

    .. code-block:: cpp

       // defined within class
       class MyClass {
           public:
               int myFunc(void) {
                   return 1 * 2;
               }
       };
   
       // defined outside
       class MyClass {
           public:
               void setX(int y);
       };
   
       void MyClass::setX(int y) {
           x = y;
       }



Helper Functions
----------------
    * also called convenience functions, auxiliary functions
    * a design concept, not programming language concept
    * often take arguments of the classes that they are helpers of
* usually put public first as it is what most people are interested in
* the rule that a name must be declared before it is used is relaxed within the limited
scope of a class
* the more public member functions are, the harder it is to find bugs
* effects of writing definition of member function within the class definition
    * compiler will try to generate code for function at each point of call, inline
    functions
    * all uses of the class will have to be recompiled when body of the function is changed
    * class definition gets larger
* never put member function bodies in class declaration unless there is performance boost
from inlining tiny functions
* create new types if it'll make the code clearer

Constructor
-----------
    * special member function that is executed whenever new objects are created
    * have same name as the class and does not return any, not even void
    * useful for setting initial values
    * default constructor does not have parameters

    .. code-block:: cpp

       class MyClass {
           public:
               MyClass(int y);
           private:
               int x, a, b;
       };
   
       MyClass::MyClass(int y) {
           x = y;
       }
   
       // using Initialization, same as above syntax
       MyClass::MyClass(int y): x{y} {}
   
   
       int main()
       {
           MyClass c1 {99}; // common style of initialization with constructor arguments
           MyClass c1(99); // C++98 style
           MyClass c1 = {99}; // OK
           MyClass c1 = MyClass{99}; // OK
       }


    - can use default arguments to provide several overloaded functions, but can only
    define default arguments for trailing parameters

    .. code-block:: cpp

       // 'a' & 'b' are default arguments, with values to be used if not supplied
       MyClass::MyClass(int y, int a = 2, int b = 3): x{y} {}
   
       // default 'b'
       MyClass::MyClass(int y, int a = 2): x{y} {}
       // default 'a' or 'b'
       MyClass::MyClass(int y): x{y} {}
   
       // error, all parameters must have default argument starting from 'a'
       MyClass::MyClass(int y, int a = 2, int b, int c): x{y} {}


    - for type T, T{} is the notation for the default value, as defined by the default
    constructor

    .. code-block:: cpp

       string s1 = string{}; // empty string ""
       vector<string> v1 = vector<string>{}; // empty vector, no elements
       int i = int{}; // 0
       double d = double{}; // 0.0
   
       // same as above
       string s1;
       vector<string> v1;
       int i;
       double d;


    * without constructor, an invariant cannot be established
* if a class doesn't have good invariant, use a ``struct`` as the data being dealt might be
plain data
* in-class initializer: an initializer for a class member specified as part of the member
declaration

Destructor
----------
    * executed whenever an object goes out of scope or delete expression is applied to a
    pointer to the object
    * exact same as the class prefixed with a tilde (~), does not return any and can't take
    parameters
    * useful for releasing resources like closing files or releasing memory

    .. code-block:: cpp

       class MyClass {
           public:
               ~MyClass();
       };
   
       MyClass::~MyClass(void) {
           cout << "Object deleted";
       }


    * destructors of the members are called when the object containing the member is destroyed

        .. code-block:: cpp

           struct MyStruct {
               string x;
               vector<string> y;
           }
   
           void f1()
           {
               MyStruct s1;
           }
   
           // destructors of x and y are called when s1 goes out of scope


    * class with a virtual function needs a virtual destructor
        - as the class is likely to be used as base class
        - derived classes are likely to be allocated using ``new`` and deleted through pointer
        to its base

Copy Constructor
----------------
    * creates an object by initializing with an object of the same class
    * used to initialize one object from another of same type, copy an object to pass it as
    argument to function, copy an object to return it from a function
    * compiler defines one if a class does not have it
    * a must to have if the class has pointer variables and dynamic memory allocations

    .. code-block:: cpp

       class MyClass {
           public:
               MyClass(const MyClass &obj);
       };
   
       MyClass::MyClass(const MyClass &obj) {
           ptr = new int;
           *ptr = *obj.ptr;
       }
   
       int main()
       {
           MyClass class1(10); // calls copy constructor
   
           MyClass class2 = class1; // also calls copy constructor
       }



Const Functions
---------------
    * reading of member variables is allowed inside the function, but not writing

    .. code-block:: cpp

       // ok
       int getX() const {
           return x;
       }
   
       // compile time error
       int getX() const {
           x++;
           return x;
       }



Friend Functions
----------------
    * defined outside the class' scope but can access all private and protected members
    * prototypes appear in class definition but are not member functions

    .. code-block:: cpp

       class MyClass {
           public:
               friend void printX(MyClass c);
               friend class MyClass2; // declare all member functions of MyClass2 as friends
       };
   
       void printX(MyClass c)
       {
           cout << c.x;
       }



Inline Functions
----------------
    * commonly used with classes
    * compiler places a copy at each point where the function is called
    * changes to inline function require all clients of function to be recompiled
    * compiler can ignore the inline qualifier if defined function has more than a line
    * function definition in a class definition is an inline function definition

    .. code-block:: cpp

       inline int myFunc(int x) {
           return x;
       }


* pointer to class, access with ->

    .. code-block:: cpp

       MyClass Class1;
       MyClass *ptrClass;
   
       ptrClass = &Class1;
       cout << ptrClass->x;



this
----
    * every object has access to its own address through ``this`` pointer
    * points to the object for which a member function is called
    * implicit parameter to all member functions
    * friend functions do not have it

    .. code-block:: cpp

       class MyClass {
           int compare(MyClass c)
           {
               return this->x > c.x;
               return x > c.x; // no need to mention `this` to access a member
           }
   
           int x;
       };


    * cannot mutate ``this`` in a member function

        .. code-block:: cpp

           class MyClass {
               void compare(MyClass* c)
               {
                   this = c; // error
               }
           };



Static Members
--------------
    * only one copy no matter how many objects are created
    * shared by all objects
    * all static data is initialized to zero when first object is created
    * cannot put in class definition

    .. code-block:: cpp

       class MyClass {
           public:
               static int y;
       };
   
       int MyClass::y = 9;
   
       int main()
       {
           cout << MyClass::y;
       }


    * static function member is independent of objects and can be called even if no objects exist
    * accessed using only class name and scope resolution operator
    * can only access static data member, other static member functions and other outside
    functions
    * have a class scope and do not have access to the ``this`` pointer
    * can be used to determine if objects are created or not

    .. code-block:: cpp

       class MyClass {
           public:
               static int y;
   
               static int getY()
               {
                   return y;
               }
       };
   
       int MyClass::y = 9;
   
       int main()
       {
           cout << MyClass::getY();
       }


`back to top <#c>`_

Object Oriented Programming
===========================

* `Inheritance`_, `Data Encapsulation`_, `Interfaces`_, `Polymorphism`_, `Designing Classes`_
* usage of inheritance, polymorphism and encapsulation is the main definition of OOP

Inheritance
-----------
    * allow to reuse the code functionality and fast implementation time
    * a class can be derived from more than one base classes

    .. code-block:: cpp

       // base class
       class MyClass {
           public:
               void setX(int a)
               {
                   x = a;
               }
   
           protected:
               int x;
       };
   
       class TheClass {
           protected:
               int h;
       };
   
       // derived class
       class HisClass: public MyClass, public TheClass {
           public:
               int y;
       };
   
       int main()
       {
           HisClass HC;
   
           HC.setX(1);
       }


    * derived class can access all non-private members of base
    * cannot inherit constructors, destructors and copy constructors, overloaded operators and
    friend functions of the base class
    * rarely use **protected** and **private** inheritance
    * **public inheritance**
        - public and protected members of base become public and protected of derived
        - private members are only accessible through calls to public and protected members of
        base
    * **protected inheritance**
        - public and protected members of base become protected members of derived
        - public and protected member names can be used by members of the class and derived
        classes
    * **private inheritance**
        - public and protected member names can only be used by members of the class
    * **Virtual Table**
        - also called vtbl and its address is called virtual pointer, vptr
        - when inheritance is used, the data members of derived class are simply added after
        those of a base
        - the table tells which function is actually invoked when a function is called, with
        only two memory accesses for finding the right function
        - only one vtbl for each class with a virtual function, not each object
    * **Overriding**
        - explicit use of ``override`` is useful in large, complicated class hierarchies

        .. code-block:: cpp

           struct A {
               virtual void f() {}
               void g() {}
           }
   
           struc B:A {
               void f() override {}
               void g() override {} // error, no virtual A::g to override
           }


    * **Interface Inheritance**
        - using interface provided by a base class
        - without having to know about the derived classes
        - doesn't need to recompile base class every time the derived classes change
    * **Implementation Inheritance**
        - simplify the implementation of derived classes by using what the base offers
        - any change to the interface of the base require recompilation of all derived classes
        and their users
* programming by difference: program only the difference of derived class to the base class

Data Encapsulation
------------------
    * OOP concept that binds the data and functions and keeps both safe from interference
    * let to data hiding, important concept of OOP
    * encapsulation: bundling the data and functions that use them
    * abstraction: exposing only the interfaces and hiding implementation details from users
    * supports through creation of ``classes``
    * keep as many of the details of each class hidden from all other classes as possible

Interfaces
----------
    * describe the behavior or capabilities of a class without committing to particular
    implementation
    * implemented using abstract classes by declaring at least one of its functions as pure
    virtual function
    * abstract class (ABC) provides appropriate base class from which others can inherit
    * instantiating an object of ABC causes compilation error
    * subclass of ABC needs to implement each of the virtual functions

        .. code-block:: cpp

           /* abstract class defines interface and two other classes implemented same function
           but with different algorithm */
           class Shape {
           public:
               virtual int area() = 0; // pure virtual function
           };
   
           // Rectangle & Triangle must implement area()
           class Rectangle : public Shape {
               public:
                   int area()
                   {
                       return (width * height);
                   }
           };
   
           class Triangle : public Shape {
               public:
                   int area()
                   {
                       return (width * height / 2);
                   }
           };



Polymorphism
------------
    * occurs when there's a hierarchy of classes and they are related by inheritance
    * a call to member function will cause different function to be executed depending on the
    type of object that invokes the function

    .. code-block:: cpp

       class Shape {
       public:
           int area()
           {
               cout << "Parent class";
               return 0;
           }
       };
   
       class Rectangle: public Shape {
       public:
           int area()
           {
               cout << "Rectangle class";
               return (width * height);
           }
       };
   
       class Triangle: public Shape {
       public:
           int area()
           {
               cout << "Triangle class";
               return (width * height / 2);
           }
       };
   
       int main()
       {
           Shape* shape;
           Rectangle rec;
           Triangle tri;
   
           shape = &rec;
           shape->area(); // will call parent Shape area()
   
           shape = &tri;
           shape->area(); // will call parent Shape area()
   
       /*call of the function area() being set once by the compiler as the version defined in the
       base class */
   
           return 0;
       }


    * above output is caused by **static resolution** of the function call or **static linkage**
        - the function call being fixed before the program is executed
        - sometimes called **early binding** because the area() function is set during compilation

    .. code-block:: cpp

       class Shape {
       public:
           virtual int area()
           {
               cout << "Parent class";
               return 0;
           }
       };
   
       int main()
       {
           // will call respective area()
           shape = &rec;
           shape->area();
   
           shape = &tri;
           shape->area();
       }


    * compiler now looks at the contents of the pointer instead of it's type
        - since addresses of objects of tri and rec classes are stored in shape, respective
        area() function is called
    * **dynamic linkage** or **late binding**
        - function in the base class declared with ``virtual`` and signals the compiler not to have
        static linkage for the function
        - only call functions based on the kind of object for which it is called
    * **pure virtual function**
        - must be overridden
        - cannot create objects of classes with pure virtual functions
        - if all pure virtual functions are not overridden, derived class is still abstract
        - classes with pure virtual function are pure interfaces, have no data members and
        constructors

        .. code-block:: cpp

           class Shape {
           public:
               virtual int area() = 0; // tells the compiler that the function has no body
           };



Designing Classes
-----------------
    * try not to be too clever and keep coherent concept
    * trying to solve everything can lead to a failure
    * even a library only models its application domain from particular perspective
    * single class providing everything can make a user confuse
    * make even small details consistent for ease of use and to avoid run-time errors
    * always ensure that modification to the state of an object is done only by its own class
    * **Abstract Class**
        - can only be used as as base class
        - usually define interfaces to groups of related classes
        - state one ore more virtual functions need to be overridden in some derived class
    * **Concrete Class**
        - can be used to create objects
    * overriding: defining a function in a derived class to be used through interfaces provided
    by a base
    * prevent the default copy operations for a type if they can cause trouble
    * do not mix default copying and class hierarchies and pass by reference
    * when designing a base class, prevent copy constructor and copy assignment by using
    ``=delete``
    * write explicit functions to copy objects of types where default copy operations are
    disabled
    * **Derivation**
        - building one class from another and using the new class instead of the original
        - inheritance will allow the use of members from the base
    * **Virtual Functions**
        - defining the same function in derived class so that it is called when a user calls
        the base class function
        - must have exactly same name and type as the base class
        - also called as run-time polymorphism, dynamic dispatch, run-time dispatch as the
        function called is determined at run time based on the type of the object
        - must be declared ``virtual`` in the class declaration, but not when defining outside
    * **Private/Protected members**
        - keeping implementation details private to protect direct use
        - also called encapsulation

`back to top <#c>`_

Dynamic Memory
==============

* stack: store all variables declared inside the function
* heap: unused memory and can be used to allocate memory dynamically when program runs
* ``new``: allocate memory at run time, returns the address of the space allocated
* ``delete``: de-allocates memory used by 'new' operator
* any built-in or user defined data type can allocate memory dynamically

.. code-block:: cpp

   double* pvalue = NULL; // initialize pointer
   pvlaue = new double; // request memory


* memory may not be allocated if the free store had been used up

.. code-block:: cpp

   // good practice to check if new operator is returning NULL pointer
   if (!(pvalue = new double)) {
       cout << "Error: out of memory" << '\n';
       exit(1);
   }
   
   delete pvalue; // release memory


* ``malloc()`` from C still exists but not recommended to use
* ``new`` doesn't just allocate memory, it also constructs objects unlike ``malloc()``
* allocation for arrays
    * syntax to release memory for arrays are same
    .. code-block:: cpp

       char* pvalue = NULL;
       pvalue = new char[20];
       delete [] pvalue;
   
       // multi-dimensional array
       double** pvalue = NULL;
       pvalue = new char[3][4]; // allocate memory for 3x4 array
       delete [] pvalue;


* allocation of objects

    .. code-block:: cpp

       MyClass* myClassArray = new MyClass[4]; // allocate array of four MyClass objects
       delete[] myClassArray;
       // constructor and destructor, while deleting objects, will be called four times


`back to top <#c>`_

Templates
=========

* blueprint to create generic class or function
* library containers like iterators & algorithms have been developed using template concept

function template
-----------------
    * ``template<class type> ret-type func-name(parameter list) {}``

    .. code-block:: cpp

       template<typename T> inline T const& Max(T const& a, T const& b)
       {
           return a < b ? b : a;
       }
   
       int main()
       {
           cout << Max(1, 2);
       }



class template
--------------
    * ``template<class type> class class-name {};``
    * can define more than one generic data type with comma-separated list

`back to top <#c>`_

Preprocessor
============

* directives that give instructions to the compiler to preprocess the information before actual
compilation starts
* begin with **#**
* not C++ statements and do not end in a semicolon
* ``#define``
    * creates symbolic constants, a *macro*

    .. code-block:: cpp

       #define PI 3.14159265
       #define MIN(a,b) (((a) < (b)) ? a : b)


* conditional compilation
    * compiling selective portions of source code

    .. code-block:: cpp

       #define DEBUG
   
       int main()
       {
           #ifdef DEBUG
           cerr << "Only compiled if DEBUG has been defined before #ifdef DEBUG"
           #endif
   
           #if 0
           code prevented from compiling
           #endif
       }


* ``#`` preprocessor operator
    * convert replacement-text token to string surrounded by quotes

    .. code-block:: cpp

       #define CONVERT(x) #x
   
       int main()
       {
           cout << CONVERT(hello world) << '\n';
           // cout << "hello world" << '\n';
       }


* ``##`` preprocessor operator
    * concatenate two tokens

    .. code-block:: cpp

       #define CONCAT(x, y) x ## y
   
       int main()
       {
           int xy = 100;
           cout << CONCAT(x, y);
           // cout << xy;
       }


* ``__LINE__``: current line number of program when it is being compiled
* ``__FILE__``: current file name of program when it is being compiled
* ``__DATE__``: string date of translation of source file into object code in month/day/year
* ``__TIME__``: time at which the program was compiled in hour:minute:second

`back to top <#c>`_

Signal Handling
===============

* signals that program can catch
    * SIGABRT: abnormal termination of program (e.g call to abort)
    * SIGFPE: arithmetic operation error (e.g divide by zero or overflow)
    * SIGILL: illegal instruction
    * SIGINT: external interrupt, usually by user
    * SIGSEGV: invalid access to storage
    * SIGTERM: termination request
* ``signal()``
    * to trap unexpected events
    * two arguments: int for signal number, pointer to the signal-handling function
    * always used to register signal to catch

    .. code-block:: cpp

       #include <csignal>
       void signalHandler(int signum)
       {
           cout << signum << '\n';
           exit(signum);
       }
   
       int main()
       {
           signal(SIGINT, signalHandler);
       }


* ``raise()``
    * to generate signals, int signal number as an argument

    .. code-block:: cpp

       if (i == 3) {
           raise(SIGINT); // will call signalHandler
       }


`back to top <#c>`_

Multithreading
==============

* specialized form of multitasking
* process-based multitasking
    * handles the concurrent execution of programs
* thread-based multitasking
    * deals with concurrent execution of pieces of the same program
* thread
    * each part of multithreaded program that can run concurrently
    * each thread defines separate path of execution
* before C++ 11, no built-in support for multithreading, instead rely on the OS for the feature
* creating POSIX thread

    .. code-block:: cpp

       #include <pthread.h>
       pthread_create (thread, attr, start_routine, arg)
       pthread_exit (status)


    * the routine can be called any number of times from any part of the code
    * thread: unique ID for new thread returned by the subroutine
    * attr: attribute object that may be used to set thread attributes, NULL for default values
    * start_routine: routine that the thread will execute once it is created
    * arg: single argument that may be passed to start_routine, must be passed by reference as
    pointer cast of type void, NULL may be used is no argument to be passed
    * ``pthread_exit()`` is called after a thread has completed its work
* max number of thread may be created is implementation dependent
* threads are peer and may create other threads
* no implied hierarchy or dependency between threads
* if ``main()`` finishes before the threads and exits with ``pthread_exit()``, other threads will
continue to execute
* otherwise, they will be terminated when ``main()`` finishes
* ``pthread_join(threadid, status)``, ``pthread_detach(threadid)``
    * blocks the calling thread until the specified threadid terminates
    * one of thread attributes defines whether it is joinable or detached
    * if a thread is created as detached, it can never be joined

`back to top <#c>`_

Web Programming
===============


CGI (common gateway interface)
------------------------------
    * set of standards that define how info is exchanged between web server and custom script
    * currently maintained by NCSA
    * CGI programs are written in Python, PERL, Shell, C or C++ etc.

var/www/cgi-bin
---------------
* CGI files have extension as **.cgi**
* by default, Apache server is configured to run CGI programs
* C++ CGI programs can interact with other external system, such as RDBMS, to exchange info

`back to top <#c>`_

Graphics
========

* `GUI`_
* subject that touch good software design and programming language facilities
* can embed color and 2D positions idea in a 1D stream of characters, like HTML and XML
* can use GUI toolkits, such as FLTK, and implement classes using them

GUI
---
    * separates the main logic of application from I/O, which allows to change program
    presentation
    * GUI programs have control inversion, the order of execution is determined by actions of
    the user, which is different from conventional programs
    * control inversion complicates both program organization and debugging
    * conventional programs
        - ``application -> input function -> user responds``
    * GUI programs
        - ``application <- system <- user action``
    * keep GUI of a program simple and build it incrementally by testing at each stage
    * **debugging GUI program**
        - check each program parts
        - simplify the code and check carefully
        - check linker settings
        - compare to a working code if possible
    * exceptions do not work well when using GUI library

`back to top <#c>`_

STL
===

* `Pair`_, `Vector`_, `List`_, `Stack`_, `Queue`_, `Deque`_, `Priority Queue`_, `Bounds`_, `Set`_, `Multi Set`_, `Unordered Set`_
* `Map`_, `Multi Map`_, `Unordered Map`_, `Sort`_, `Reverse`_, `PopCount`_, `Permutation`_, `Min/Max Element`_
* Standard Template Library, compilation of predefined algorithms, containers, functions and
  iterators

Pair
----
    * store two objects as single unit

    .. code-block:: cpp

       // pair<TYPE, TYPE>;
       pair<int, int> p;
   
       p = make_pair(1, 2);
       // OR
       p = {1, 2};
   
       p.first; // 1
       p.second; // 2



Vector
------
    * contiguous dynamic array
    * similar to an array in other languages, but doesn't need to specify the size of vector
      in advance
    * doesn't just store elements, also stores size
    * element type comes after ``vector`` in angle brackets (< >)
    * will only accept elements of declared type
    * can also define a vector of given size without specifying elements

    .. code-block:: cpp

       #include <vector>
       // vector<Type> v;
       vector<int> v(6); // vector of 6 int initialized to 0/garbage value, depend on compiler
       vector<int> v(6, 3); // vector of 6 int initialized to 3
       vector<string> s(4); // vector of 4 strings initialized to ""


    * range of vector v: [0:v.size())
        - notion of half-open sequences is used throughout C++ and the C++ standard library

    .. code-block:: cpp

       v.push_back(); // add new element at the end, copy objects
       v.emplace_back(); // dynamically increase the size and add element at the end
                         // faster than push_back(), no copying objects
       v.reserve(3); // increase and set vector capacity without creating objects
                     // no initializing values
   
       vector<pair<int, int>> v;
       v.push_back({1, 2});
       v.emplace_back(1, 2); // auto assume input is a pair
   
       vector<int> v1;
       vector<int> v2(v1); // copy vector
   
       v[1]; v.at(1); // accessing vector


    * getting input and adding to vector, using input operation as the condition for a for-loop
        - use character '|' to terminate the input
        - using for-loop limit the scope of input variable, x, to the loop, rather than 'while'

        .. code-block:: cpp

           vector<double> vs;
           for(double x; cin>>x)
               vs.push_back(x);


    * iterator: points to the memory address

    .. code-block:: cpp

       vector<int>::iterator it = v.begin(); // vector iterator
       *(it) // same as v[0]
       *(it + 2) // v[2]
   
       v.end(); // points to memory location after the last element
       v.rend(); // reverse the vector and points to memory location after the last element
       v.rbegin(); // reverse the vector and points to memory location of the first element
       v.back(); // value of last element
   
       // print values using iterator
       for(vector<int>::iterator it = v.begin(); it != v.end(); it++) {
           cout << *(it) << endl;
       }
       for(auto it = v.begin(); it != v.end(); it++) {
           cout << *(it) << endl;
       }
   
       // type of "it" = int
       for(auto it : v) {
           cout << it << endl;
       }
   
       // Delete
       // v.erase(ITERATOR) OR v.erase(START, END), END is exclusive
       v.erase(v.begin());
       v.erase(v.begin(), v.begin() + 3); // delete from begin to begin + 2
       v.pop_back(); // remove the last element
       v.clear(); // delete entire vector
   
       // Insert
       // v.insert(ITERATOR, OPTIONAL_COUNT, VALUE)
       v.insert(v.begin(), 2); // insert 2 at the start
       v.insert(v.begin() + 1, 3, 2); // insert three 2s at begin + 1
       v1.insert(v1.begin(), v2.begin(), v2.end()); // add v2 at the start of v1
   
       // Swap
       v1.swap(v2);
   
       v.empty(); // return 0 or 1
       v.size(); // number of elements
       v.capacity(); // storage space allocated, can be greater than the size
       v.shrink_to_fit(); // release unused storage space, capacity equal size after shrink
   
       // 2D vector
       vector<vector <int>> v;
       v.push_back(v1);
       v.push_back(v2);
       v.push_back(v3); // will have 3 rows, columns differ on sizes of v1, v2 and v3
       // vector<vector <int>> v(ROW, vector<int> (COLUMN));
       vector<vector <int>> v(3, vector<int> (4, 0)); // initialize elements to zero



List
----
    * non-contiguous doubly linked list
    * each link holds information and pointers to other links
    * operations are cheaper than in vector

    .. code-block:: cpp

       list<int> ls;
   
       ls.push_back(); // add new element at the end
       ls.emplace_back(); // dynamically increase the size and add element at the end
                         // faster than push_back()
       ls.emplace_front(); // dynamically increase the size and add element at the start
       ls.push_front(); // add element at the begin
   
       ls.end(); // points to memory location after the last element
       ls.rend(); // reverse the vector and points to memory location after the last element
       ls.rbegin(); // reverse the vector and points to memory location of the first element
       ls.back(); // value of last element
   
       // Delete
       // ls.erase(ITERATOR) OR ls.erase(START, END), END is exclusive
       ls.erase(ls.begin());
       ls.erase(ls.begin(), ls.begin() + 3); // delete from begin to begin + 2
       ls.pop_back(); // remove the last element
       ls.pop_front(); // remove the first element
       ls.clear(); // delete entire vector
   
       // Insert
       // ls.insert(ITERATOR, OPTIONAL_COUNT, VALUE)
       ls.insert(ls.begin(), 2); // insert 2 at the start
       ls1.insert(ls1.begin(), ls2.begin(), ls2.end()); // add ls2 at the begin of ls1
   
       // Swap
       ls1.swap(ls2);
   
       ls.empty(); // return 0 or 1
       ls.size(); // number of elements



Stack
-----
    * Last In, First Out
    * cannot access by index, no iterator
    * all operations are O(1)

    .. code-block:: cpp

       stack<int> st;
   
       st.push(1); // add existing element at the start of the stack
       st.emplace(2); // create new element and add at the start of the stack
       // st = {1, 2}
       st.top(); // last added element, 2
       st.pop(); // remove last added element
   
       st.empty(); // return 0 or 1
       st.size(); // number of elements
   
       // Swap
       st1.swap(st2);
   
       while(!st.empty()) {
           cout << st.top() << "\n";
           st.pop();
       }



Queue
-----
    * First In, First Out
    * cannot access by index, all operations are O(1)

    .. code-block:: cpp

       queue<int> q;
   
       q.push(1); // add existing element at the end of the queue
       q.emplace(2); // create new element and add at the end of the queue
       // q = {1, 2}
       q.front(); // first element
       q.back(); // last element
       q.pop(); // remove first element
   
       q.empty(); // return 0 or 1
       q.size(); // number of elements
   
       // Swap
       q1.swap(q2);
   
       while(!q.empty()) {
           cout << q.front() << "\n";
           q.pop();
       }



Deque
-----
    * dynamic double-ended queue, contiguous memory is not guaranteed
    * can insert and pop from both sides

    .. code-block:: cpp

       deque<int> dq;
   
       dq.push_back(); // add new element at the end
       dq.emplace_back(); // dynamically increase the size and add element at the end
                         // faster than push_back()
       dq.emplace_front(); // dynamically increase the size and add element at the start
       dq.push_front(); // add element at the begin
   
       dq.front(); // first element
       dq.back(); // last element
   
       dq.end(); // points to memory location after the last element
       dq.rend(); // reverse the vector and points to memory location after the last element
       dq.rbegin(); // reverse the vector and points to memory location of the first element
   
       // Delete
       // dq.erase(ITERATOR) OR dq.erase(START, END), END is exclusive
       dq.erase(dq.begin());
       dq.erase(dq.begin(), dq.begin() + 3); // delete from begin to begin + 2
       dq.pop_back(); // remove the last element
       dq.pop_front(); // remove the first element
       dq.clear(); // delete entire vector
   
       // Insert
       // ls.insert(ITERATOR, OPTIONAL_COUNT, VALUE)
       dq.insert(dq.begin(), 2); // insert 2 at the start
       dq1.insert(dq1.begin(), dq2.begin(), dq2.end()); // add ls2 at the begin of ls1
   
       // Swap
       dq1.swap(dq2);
   
       dq.empty(); // return 0 or 1
       dq.size(); // number of elements



Priority Queue
--------------
    * First In, First Out, max heap, largest value stay at the front, non-contiguous
    * cannot access by index
    * push: O(log n), top: O(1), pop: O(log n)

    .. code-block:: cpp

       priority_queue<int> pq;
   
       pq.push(4); // add existing element to the queue
       pq.emplace(3); // create new element and add to the queue
       pq.push(9);
       // pq = {9, 4, 3}
       pq.top(); // first element
       pq.pop(); // remove first element
   
       pq.empty(); // return 0 or 1
       pq.size(); // number of elements
   
       // Swap
       pq1.swap(pq2);
   
       // min heap, smallest value at the front
       priority_queue<int, vector<int>, greater<int>> pq;



Bounds
------
    * usually used for binary search
    * elements should be in sorted order
    * **Upper**
        - greater than

        .. code-block:: cpp

           vector<int> v = {1, 3, 9};
           // address of element greater than to 3
           auto it = upper_bound(v.begin(), v.end(), 3); // *it = 9
           auto it = lower_bound(v.begin(), v.end(), 4); // *it = 9
           auto it = upper_bound(v.begin(), v.end(), 3) - v.begin(); // it = 2, index of element


    * **Lower**
        - greater than or equal to

        .. code-block:: cpp

           vector<int> v = {1, 3, 9};
           // address of element greater than or equal to 3
           auto it = lower_bound(v.begin(), v.end(), 3); // *it = 3
           auto it = lower_bound(v.begin(), v.end(), 4); // *it = 9
           auto it = lower_bound(v.begin(), v.end(), 3) - v.begin(); // it = 1, index of element



Set
---
    * store elements in unique sorted order, default ascending
    * will not store existing element if added again
    * operations are in O(log n)

    .. code-block:: cpp

       set<int> s;
   
       s.insert(4); // add existing element
       s.emplace(3); // create new element and add
       // s = {3, 4}
   
       s.find(); // find element and return iterator
                 // finding non-existing element will return iterator after last element
       s.erase(); // find element/iterator and delete, maintain sorted order, erase(START, END)
       s.count(); // check element exist, return 0 or 1
       s.empty(); // return 0 or 1
       s.lower_bound();
       s.upper_bound();
   
       for(auto it = s.begin(); it != s.end(); it++) {
           cout << *(it) << endl;
       }
   
       // sorted in descending order
       set<int, greater<int>> s;



Unordered Set
-------------
    * elements have to be unique, stored in random order
    * operations are in O(1), rarely can be O(n)
    * no lower_bound and upper_bound functions

    .. code-block:: cpp

       unordered_set<int> us;
   
       us.insert(); // add existing element
       us.emplace(); // create new element and add
   
       us.find(); // find element and return iterator
                  // finding non-existing element will return iterator after last element
       us.erase(); // find element/iterator and delete
       us.erase(us.find()); // only first occurrence of the element will be deleted
       us.erase(us.find(), us.find()+2); // erase(START, END), END is exclusive
       us.count(); // return count



Multi Set
---------
    * store elements in sorted order, default ascending
    * elements not need to be unique

    .. code-block:: cpp

       multiset<int> ms;
   
       ms.insert(4); // add existing element
       ms.emplace(3); // create new element and add
       ms.emplace(4); // create new element and add
       // ms = {3, 4, 4}
   
       ms.find(); // find element and return iterator
                  // finding non-existing element will return iterator after last element
       ms.erase(); // find element/iterator and delete, maintain sorted order
                   // will delete all occurrence of the element
       ms.erase(ms.find()); // only first occurrence of the element will be deleted
       ms.erase(ms.find(), ms.find()+2); // erase(START, END), END is exclusive
       ms.count(); // return count
   
       // sorted in descending order
       multiset<int, greater<int>> ms;



Map
---
    * store key-value pairs in sorted order by key
    * keys must be unique and can be any data type
    * can have large values, unlike array
    * operations are in O(log n)

    .. code-block:: cpp

       map<int, int> mp;
       // map<pair<int, int>, int> mp;
   
       mp[KEY] = VALUE;
       mp.insert({KEY, VALUE});
       mp.emplace({KEY, VALUE});
   
       for(auto it : mp){
           // fist=key, second=value
           cout << it.first << " " << it.second << endl;
       }
   
       mp.find(); // return iterator



Multi Map
---------
    * store key-value pairs in sorted order by key
    * accept duplicate keys

    .. code-block:: cpp

       multimap<int, int> mp;
   
       mp[KEY] = VALUE;
       mp.insert({KEY, VALUE});
       mp.emplace({KEY, VALUE});
   
       for(auto it : mp){
           // fist=key, second=value
           cout << it.first << " " << it.second << endl;
       }
   
       mp.find(); // return iterator



Unordered Map
-------------
    * store key-value pairs, not sorted order
    * keys must be unique
    * operations are in O(1), rarely can be O(n)

    .. code-block:: cpp

       unordered_map<int, int> mp;
   
       mp[KEY] = VALUE;
       mp.insert({KEY, VALUE});
       mp.emplace({KEY, VALUE});
   
       for(auto it = mp.begin(); it != mp.end(), it++){
           cout << it->first << " " << it->second << endl;
       }
   
       mp.find(); // return iterator



Sort
----

    .. code-block:: cpp

       // sort(START_ITERATOR, END_ITERATOR) END_ITERATOR is exclusive, ascending by default
       sort(a, a + 4);
       sort(a + 2, a + 4); // sort only portion of array
       // sort(START, END, BOOL_COMP_FUNC)
       sort(a, a + 4, greater<int>()); // descending order
   
       // Example using comparator function
       vector<pair<int, int>, pair<int, int>> v;
   
       bool comp(pair<int, int>& a, pair<int, int>& b){
           return a.first < b.first; // if false, a and b will be swapped to sort
       }
   
       sort(v.begin(), v.end(), comp);



Reverse
-------

    .. code-block:: cpp

       // reverse(START_ITERATOR, END_ITERATOR) END_ITERATOR is exclusive
       reverse(a, a + 4);
       reverse(a + 2, a + 4); // reverse only portion of array



PopCount
--------
    * return number of 1 bits

    .. code-block:: cpp

       __builtin_popcount(x); // x is int
       __builtin_popcountll(x); // x is long long



Permutation
-----------
    * **Next Permutation**
        - return 0 or 1
        - string need to be sorted

        .. code-block:: cpp

           // next_permutation(s.begin(), s.end());
           string s = "123"; // will have 3! permutations
           string x = "231"; // only 3 permutations
           do {
               cout << s << endl;
           }
           while(next_permutation(s.begin(), s.end()));



Min/Max Element
---------------
    * return memory of min/max element in array

    .. code-block:: cpp

       int max = *min_element(START_ITERATOR, END_ITERATOR);
       int max = *max_element(START_ITERATOR, END_ITERATOR);


`back to top <#c>`_

STL Custom Implementation
=========================

* `Vector Custom`_, `List Custom`_

Vector Custom
-------------
    * need data members to hold the size and elements
        - functions such as ``push_back()`` cannot be implemented with fixed number of elements
        - need data member that points to different sets of elements if more space is required
        , such as a pointer to the first element

    .. code-block:: cpp

       class vector {
       public:
           vector(int);
   
           ~vector();
   
           int size() const { return sz; };
   
           double get(int);
   
           void set(int, double);
   
           int operator[](int);
       private:
           int sz;
           double* elements;
       };



List Custom
-----------

`back to top <#c>`_

Sample Project Structure
========================

* top of the project should be entry point for development
    * enable all development features such as dependency management, tests, docs etc.
* have src as separate cmake project
    * so consumers can use it as entry point
    * does not affect development environment
    * each sub folder is a module, can enable/disable easily
* every sub folder's structure looks similar to the parent's
* use a package manager
    * to have reproducible build for consumers
    * e.g Conan, vcpkg
* at least do these for CI
    * build on windows, linux, mac with gcc, clang, msvc
    * build debug and release versions
    * run cppcheck, clang-tidy
* use tools that makes source files uniform
    * formatters such as clang-format, cmake-format
    * linters such as clang-tidy, clazy, cppcheck, include-what-you-use
    * commercial linters such as pvs-studio, sonar are also available
    * pre-commit tools
* example structure


    PROJECT
       |--->cmake
       |--->src
       |      |--->app1
       |      |      |--->src
       |      |      |----CMakeLists.txt
       |      |--->cmake
       |      |--->module
       |      |      |--->include
       |      |      |--->src
       |      |      |----CMakeLists.txt
       |      |----CMakeLists.txt
       |
       |--->tests
       |----CMakeLists.txt
       |----CMakePresets.json


`back to top <#c>`_
