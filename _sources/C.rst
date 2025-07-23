=
C
=

1. `Basics`_
2. `Arrays`_
3. `Functions`_
4. `Bit Masking`_
5. `Terminal Modes`_
6. `GDB`_
7. `References & External Resources`_

`back to top <#c>`_

Basics
======

* `Variables`_, `Data Types`_, `Constants`_, `Arithmetic`_, `Loops`_, `Input & Output`_
* a C program contains functions and variables
* function contains statements to specify operations
* variables store values used during computation
* program execute at the beginning of ``main()``, and every program must have it
* escape sequence: ``\n``, ``\t``, ``\b``, ``\"``, ``\\``, provide mechanism to represent hard-to-type or
  invisible characters
* all variables must be declared before use
* compilers do not care about how the program looks, but proper indentation and spacing are
  critical for readability
* write only one statement per line, use blanks around operators to clarify grouping
* always indent statements controlled by the loop by one tab stop or four spaces
* pick a style and use it consistently
* do no mistake condition checking ``==`` with assign ``=``, as it will give no warning if misused

.. code-block:: c

   #include <stdio.h>
   
   int main ()
   {
       // this is single-line comment
       /* this is
       multi-line comment
       */
       printf("hello\n");
   }



Variables
---------
    * Declaration: where variable is stated but no storage is allocated
    * Definition: where the variable is created or assigned storage
    * **Naming Convention**
        - can use letters and digits, but first character must be a letter
        - only library routine names should start with underscore
        - cannot use keywords such as ``if``, ``else``, ``int``, etc.,
        - use lower case, and choose names that are related to the purpose of the variable
    * **Automatic Variables**
        - local variable in function is created only when the function is called, and
          disappear when the function exits
        - do not retain values from one call to the next
        - must be explicitly set on each entry, and will contain garbage if not set
    * **External Variables**
        - can be accessed by name by any function, globally accessible
        - can used instead of argument lists to communicate data between functions
        - retain values after the function has returned
        - must be defined exactly once, outside of any function
        - must be declared in each function to access it
        - declaration can be explicit ``extern`` or implicit
        - sometimes ``extern`` can be omitted, if the definition of variable occurs in the
          source file before its use in a function
        - common practice to place definitions of all external variables at the beginning of
          the source file, and omit all ``extern`` declarations
        - if variable is defined and used in separate files, ``extern`` is required
        - better to collect ``extern`` declarations in a header file
        - relying heavily on external variables is dangerous, as they can be changed
          unexpectedly, and make program hard to modify

        .. code-block:: c

           int max;
           char line[MAXLINE];
   
           int main()
           {
               // 'extern' can be omitted
               extern max;
               extern char longest[];
               return 0;
           }



Data Types
----------
    * ``char``: single byte, signed or unsigned plain chars are machine dependent, but printable
      chars are always positive
    * ``int``: 16 or 32 bits, machine dependent
    * ``float``: single-precision floating point
    * ``double``: double-precision floating point
    * **Qualifiers**
        - can be applied to basic types
        - ``short``: 16 bits, e.g. ``short int a`` where ``int`` can be omitted
        - ``long``: 32 bits
        - ``signed``: has negative values
        - ``unsigned``: always positive or zero
    * size of data types are machine dependent, and compilers can choose appropriate sizes for
      its own hardware
    * only ``short`` and ``int`` being at least 16 bits, and ``long`` at least 32 bits (size of
      ``short`` < ``int`` < ``long``)
    * float is usually 32 bit, with at least six significant digits and between 10<sup>-38</sup>
      and 10<sup>38</sup> magnitude
    * can use ``int`` instead of ``char``
    * ``<limits.h>`` and ``<float.h>`` has symbolic constant for all the sizes

Constants
---------
    * Integer: ``1234``, ``u`` or ``U`` for unsigned
    * Long: ``12345678l`` or ``12345678L``, ``ul`` or ``UL`` for unsigned
    * Double: ``123.4`` or ``1e-2``, ``l`` or ``L`` for ``long double``
    * Float: ``123.4f`` or ``123.4F``
    * Octal: ``037``, can have ``L`` and ``U``
    * Hexadecimal: ``0x1f``, can have ``L`` and ``U``
    * Character: an integer numeric value in the machine's character set, e.g. ASCII
      'A' = 65
    * integer too big to fit into an ``int`` will also be taken as ``long``
    * **Escape Sequences**
        - ``\a``: alert bell, ``\b``: backspace, ``\f``: formfeed
        - ``\n``: newline, ``\r``: carriage return
        - ``\t``: horizontal tab, ``\v``: vertical tab
        - ``\\``: backslash, ``\?``: question mark
        - ``\'``: single quote, ``\"``: double quote
        - ``\ooo``: octal number, ``\xhh``: hexadecimal number
        - ``\0``: null character
    * **Symbolic Constants or Constant Expressions**
        - bad practice to bury magic numbers, as they give little or no information
        - give meaningful names by defining as symbolic name or symbolic constant
        - ``#define name replacement``: any occurrence of ``name`` will be replaced with ``replacement``
        - symbolic constants are not variables and do not appear in declarations
        - may be evaluated during compilation
        - always write in upper case, and no semicolon at the end of the line

        .. code-block:: c

           #define MY_CONSTANT 99


    * **String Constants/Literals**
        - zero or more characters surrounded by double quotes, e.g. ``"String"`` or ``""`` for
          empty string
        - can be concatenated at compile time, e.g. ``"hello,"  " world"`` equals to
          ``"hello, world"``
        - long strings can be split into several lines
        - a string has ``\0`` at the end, so requires one more than the number of characters for
          storage
    * **Enumeration**
        - a list of constant integer values, and names must be distinct
        - first name has value 0, unless explicitly specified
        - unspecified values continue from the last specified value
        - compilers need not to check if the value is valid for enumeration

        .. code-block:: c

           enum boolean { NO, YES }; /* 0, 1 */
   
           enum months { JAN = 1, FEB, MAR }; /* 1, 2, 3 */



Arithmetic
----------
    * integer division truncates, fractional part is discarded
    * for integer operands, integer operation is performed
    * if operator has one integer and one float, integer will be converted to float

Loops
-----
    * **while**
        - condition in parentheses is tested
        - if true, body of the loop is executed, and loop ends when the test is false

        .. code-block:: c

           int i = 0;
           while (i++ < 10) {
               printf("hello world\n");
           }


    * **for**
        - has initialization, testing condition and increment step
        - initialization and increment can be any expressions
        - appropriate for loops in which initialization and increment are single statements
          and logically related
        - can have null statement as body

        .. code-block:: c

           for (int i = 0; i < 10; ++i) {
               printf("%d\n", i);
           }
   
           // with null statement
           for (nc = 0; getchar() != EOF; ++nc)
             ;



Input & Output
--------------
    * text input or output is dealt with as streams of characters
    * text stream: sequence of characters divided into lines
    * ``getchar()``
        - reads the next input single character from text stream and returns it
        - if no more input, return ``EOF``, end of file, integer defined in ``<stdio.h>``
        - variable must be big enough to hold ``EOF`` and any possible value returned, e.g.
          using ``int`` instead of ``char``

        .. code-block:: c

           // get a char, assign it to c, and test condition
           // precedence of != is higher than =
           while ((c = getchar()) != EOF) {
               putchar(c);
           }


    * ``putchar()``
        - prints/write one character each time it is called
    * ``printf()``
        - ``printf()`` is not part of C, as there is no input or output defined in C itself
        - a function from the standard library of functions
        - never auto supply newline character
        - each % indicate argument and its form to be substituted
        - ``%d``: integer, ``%f``: both float and double
        - ``%ld``: long int
        - ``%o``: octal, ``%x``: hexadecimal
        - ``%c``: character, ``%%``: % itself
        - can specify width and precision for better output
        - ``printf("%7d %3d", 10, 20);``, and ``printf("%7.2f", 10.12345673);`` with at least 7
          characters wide and 2 digits after decimal point
    * ``read()``
        - read from a file descriptor, up to given bytes count, into the buffer
        - ``read(STDIN_FILENO, &c, 1)``: read 1 byte from standard input into variable ``c``
    * **Characters**
        - control characters: non-printable, ASCII codes 0-31 and 127, can check with
          ``iscntrl()``
        - printable characters: ASCII 32-126
        - arrow keys input 3 or 4 bytes to terminal with escape sequences, which start with
          ASCII 27 byte
    * **Escape Sequences**
        - ``\x1b`` is the escape character or decimal 27
        - escape sequences always start with the escape character followed by ``[`` character
        - escape sequences instruct the terminal to do text formatting tasks
        - escape sequence format: ``<esc>[<argument><command>`` with multiple arguments
          separated by ``;``, e.g. ``\x1b[2J`` clear the entire
          screen
        - can use VT100 escape sequences or ncurses library
        - Erase In Display: ``J`` command to clear the screen
        - Cursor Position: ``H`` command to position the cursor at specific row and column
        - Cursor Forward: ``C`` command to move the cursor to the right, will not go past the
          screen edge
        - Cursor Down: ``B`` command to move the cursor down, will not go past the screen edge
        - Device Status Report: ``n`` command to query terminal status
    * **Append Buffer**
        - having one big ``write()`` is more optimal than a bunch of small ``write()``
        - have a buffer of characters or strings, and write the buffer
        - create dynamic buffer if necessary

`back to top <#c>`_

Arrays
======

* `Character Array`_

Character Array
---------------
    * the most common type of array

    .. code-block:: c

       /*
           while (another line)
               if (longer than previous longest)
                   (save it)
                   (save its length)
           print longest line
       */
       #include <stdio.h>
       #define MAXLINE 1000
   
       int get_line(char s[], int lim);
       void copy(char to[], char from[]);
   
       int main()
       {
           int len;
           int max;
           char line[MAXLINE];
           char longest[MAXLINE];
           max = 0;
           while ((len = get_line(line, MAXLINE)) > 0) {
               if (len > max) {
                   max = len;
                   copy(longest, line);
               }
           }
           if (max > 0)
               printf("%s", longest); // '%s' expect argument to be in "hello\n\0" form
           return 0;
       }
   
       int get_line(char s[], int lim)
       {
           int c, i;
           for (i = 0; i < lim - 1 && (c = getchar()) != EOF && c != '\n'; ++i)
               s[i] = c;
           if (c == '\n') {
               s[i] = c;
               ++i;
           }
           s[i] = '\0';
           return i;
       }
   
       // void return type, states that no value is returned
       void copy(char to[], char from[])
       {
           int i = 0;
           while ((to[i] = from[i]) != '\0')
               ++i;
       }


`back to top <#c>`_

Functions
=========

* `Function Prototype`_, `Call by Value`_, `Call by Reference`_, `Scopes`_
* provide convenient way to encapsulate computation
* can use a function without worrying about its implementation
* function definition can be in any order, in one source file or several
* parameter: in function definition, local to the function, not visible to others and they can
  use the same names
* argument: value used in a call of the function
* not necessary to return a value
* caller can ignore the return value
* ``main()`` return a value to its caller, the environment in which program was executed, 0
  for normal termination and non-zero for error condition

.. code-block:: c

   /* return-type function-name(parameter declarations) {
       declarations
       statements
   } */
   
   int hello() {
       printf("world\n");
       return 200;
   }



Function Prototype
------------------
    * declaration before definition, parameter names are optional
    * function definition must agree with its prototype

    .. code-block:: c

       void hello(int);
   
       int main() {
           hello(2);
       }
   
       void hello(int num) {
           printf("%d\n", num);
       }



Call by Value
-------------
    * all function arguments are passed by value
    * called function is given temporary variables, not the originals

    .. code-block:: c

       int power(int base, int n) {
           int p;
           /* 'n' is used as temporary, no need to use 'i' for loop, and 'n' is only modified
               inside the function
           */
           for (p = 1; n > 0; --n)
               p = p * base;
           return p;
       }



Call by Reference
-----------------
    * can make a function modify variable
    * caller must provide the address of the variable, a pointer
    * function must also declare the parameter to be a pointer, to access the variable
      indirectly through it
    * when array is used as argument, value passed to the function is the address of the
      beginning of the array, and there is no copying of elements

`back to top <#c>`_

Bit Masking
===========

* `Bit Shifting`_, `Extract Bits`_, `Set Bits`_, `Clear Bits`_, `Toggle Bits`_, `Flip Bits`_
* manipulate specific bits within a data structures, by using bitwise operations to extract,
  set, clear, or toggle individual bits or groups of bits

Bit Shifting
------------
    * **Shift Left (<<)**
        - shift all bits to the left by a specified number of positions, filling with zeros
          on the right
        - ``num << n``
        - left shifting a number by 1 bit is same as multiplying by 2, ``num << 1 == num * 2``
        - can use left shifting to calculate power of 2, e.g. ``1 << num == 2^num``
    * **Shift Right (>>)**
        - shift all bits to the right by a specified number of positions, filling with the
          sign bit or zeros on the left
        - ``num >> n``
        - right shifting a positive number by 1 bit is same as diving by 2, and same for
          negative number when using arithmetic shift, ``num >> 1 == num / 2``
        - can use right shift to divide the number by power of 2, ``num >> n == num / (2^n)`` or
          ``num >> n == num / (1 << n)``

Extract Bits
------------
    * extract specific bits by using AND bitwise operation with a mask with 1s in the position
      to extract
    * e.g. ``num & 0x0f`` extract the lower 4 bits, ``(num >> n) & 1`` extract the bit at (n + 1)
      position

Set Bits
--------
    * set specific bits to 1 by using OR bitwise operation with a mask with 1s in the positions
      to set
    * e.g. ``num | 0x0f`` set the lower 4 bits to 1

Clear Bits
----------
    * clear specific bits, set to 0, by using AND bitwise operation with a mask with 0s in the
      positions to clear
    * e.g. ``num & ~0x0f`` or ``num & 0xf0`` clear the lower 4 bits
    * to clear a specific bit, flip, bitwise OR with a mask with 1 at that position, and flip
      again
    * e.g. ``~(~num | (1 << (n - 1)))``, clear 3rd bit in 15, ``~(~15 | (1 << 2)) = 11``
    * can also use bitwise AND to clear a specific bit
    * e.g. ``num & ~(1 << (n - 1))``, clear 3rd bit in 15, ``15 & ~(1 << 2) = 11``

Toggle Bits
-----------
    * toggle/invert specific bits by using XOR bitwise operation with a mask with 1s in the
      positions to toggle
    * e.g. ``num ^ 0x0f`` toggle the lower 4 bits

Flip Bits
---------
    * flip all bits by using NOT bitwise operation, no mask required
    * e.g. ``~num``

`back to top <#c>`_

Terminal Modes
==============

* `Canonical Mode`_, `Raw Mode`_, `Terminal Size`_

Canonical Mode
--------------
    * also called Cooked Mode, default mode
    * input is only sent to the program when ``Enter`` is pressed

Raw Mode
--------
    * process each key press, need to turn off many flags in the terminal to enter this mode
    * can use functions provided in ``<termios.h>``
    * ``struct termios``: contain I/O, control and local modes, and special characters
    * ``tcgetattr()``: read current attributes in ``struct termios``
    * ``tcsetattr()``: set new terminal attributes

    .. code-block:: c

       void enable_raw_mode()
       {
           struct termios raw;
   
           /* read attributes into raw */
           tcgetattr(STDIN_FILENO, &raw);
   
           /* turn off ECHO feature, will not show what is being typed */
           raw.c_lflag &= ~(ECHO);
   
           /* apply modifications */
           /* TCSAFLUSH waits for all pending output to be written to terminal,
           and discard input that hasn't been read
           */
           tcsetattr(STDIN_FILENO, TCSAFLUSH, &raw);
       }



Terminal Size
-------------
    * **Using ioctl()**
        - defined in ``<sys/ioctl.h>``, use with ``TIOCGWINSZ`` to get terminal size on most
          systems
        - use ``struct winsize`` to save number of rows and columns
        - need to check if ``ioctl()`` returns -1 or gives 0 for row and column
    * **Using Escape Sequence**
        - put the cursor at the bottom-right of the screen, and use escape sequence Cursor
          Position Report to query the position of the cursor
        - read the cursor position report into buffer and parse it if necessary

`back to top <#c>`_

GDB
===

* `GDB Commands`_
* GNU Project Debugger
* break down a compiled program for details, e.g. step through lines, list variables and stack
* use ``-g`` flag when compiling to get debugging information, and ``gdb ./program`` to start
* code printed is not executed yet


GDB Commands
------------
    * commands can be shorten to the first few letters
    * **run**
        - ``run`` or ``r``, runs the program
        - stops if there are any current execution, and starts a new instance
        - like bash commands, can give arguments, input redirection, etc.
    * **break**
        - ``break LINE`` or ``br FUNC_NAME``, set breakpoint at specific line
    * **next**
        - ``next`` or ``n``, run the code, but will not go into the function line by line
    * **list**
        - ``list`` or ``l LINE_NUM``, print code around current or given line
    * **print**
        - ``print VAR`` or ``p VAR``, print the value of the given variable
        - can use C and C++ syntax to evaluate expressions, e.g. ``p (VAR * 10) + 5``, ``p *ptr``
    * **quit**
        - ``quit`` or ``q``, quit GDB instance
    * **up/down**
        - ``up``, ``down``, navigate through the call stack one at a time
    * **display/undisplay**
        - ``display VAR``, display the value of given variable at every command
        - ``undisplay DISPLAY_ID``, stop displaying at every command, need to give ID instead of
          variable name
    * **backtrace**
        - ``backtrace`` or ``bt``, print the current entire call stack
        - useful to isolate parts of the code
    * **step**
        - ``step`` or ``s``, execute one line of code
    * **continue**
        - ``continue`` or ``c``, run from current line until breakpoint
    * **finish**
        - ``finish`` or ``fin``, run the current function call and stop once finished
        - useful for checking only the return value, ignoring what the function does
    * **watch**
        - ``watch VAR``, set watchpoint on the given variable and report if it changes
    * **info**
        - ``info SUB_COMMAND``, display information on given subcommands
        - e.g. ``info br`` will show current breakpoints
    * **delete**
        - ``delete`` or ``d ID``, delete all or given breakpoints, watchpoints, tracepoints, and
          catchpoints
    * **whatis**
        - ``whatis VAR`` or ``what EXP``, print data type of given variable or expression
    * **target record-full**
        - ``target record-full``, record everything from current point and on
    * **reverse-next**
        - ``reverse-next`` or ``rn``, go back to the previous step
    * **set**
        - ``set var VAR=VALUE``, set variable value before executing
        - useful for testing behaviour changes

`back to top <#c>`_

References & External Resources
===============================

* Learn Learn Scratch Tutorials. (2021). Bitwise Operations & Bit Masking. Available at:
  https://youtu.be/ffPOA7UUDAs?si=0zu6dPhu34mjgdoZ
* CS 246. (2019). GDB Tutorial. Available at: https://youtu.be/svG6OPyKsrw?si=QwG4LyTX9zV2Qiqw

`back to top <#c>`_
