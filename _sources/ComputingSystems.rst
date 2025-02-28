=================
Computing Systems
=================

1. `Basics`_
2. `Boolean Logic`_
3. `Logic Gates`_
4. `Nand2Tetris HDL`_

`back to top <#computing-systems>`_

Basics
======

* `Compilation`_, `Hardware`_, `Abstraction`_
* a program is just a bunch of characters stored in a text file

Compilation
-----------
    * parse text, uncover semantics and express in low-level language understood by computer
    * analysed text is kept in a parse tree data structure
    * consist of a compiler, virtual machine, if necessary, and assembler
    * result is a text file containing machine-level code

Hardware
--------
    * a hardware architecture must know how to read machine-level code
    * architecture is implemented by certain chip set, such as registers, memory units, ALU
    * each hardware device is constructed from integrated elementary logic gates
    * each logic gate can be built from primitive gates like NAND and NOR
    * primitive gates consist of several switching devices, implemented by transistors

Abstraction
-----------
    * only define what the entity does, ignore the details of how it does it
    * description must contain all that need to be known to use the entity's services
    * all implementations must be concealed from client as they are irrelevant
    * top-down: high-level abstractions reduced or expressed by simpler ones
    * bottom-up: low-level abstractions used to construct more complex ones

`back to top <#computing-systems>`_

Boolean Logic
=============

* `Boolean Algebra`_, `Boolean Identities`_, `Binary Addition`_, `Negative Numbers`_
* all chips in digital devices are made from elementary logic gates
* logic gates can be physically implemented in different materials and fabrications, but
behaviour is  consistent across all components

Boolean Algebra
---------------
    * deals with binary values labelled as true/false, 1/0, yes/no or on/off
    * boolean function operates on binary inputs and return binary outputs
    * boolean functions are centre of specification, construction and optimization of hardware
    architectures
    * **Truth Table**
        - specify boolean function by enumerating all possible values of inputs and outputs
         | x | y | z | f(x, y, z) |
         |---|---|---|------------|
         | 0 | 0 | 0 |     0      |
         | 0 | 0 | 1 |     0      |
         | 0 | 1 | 0 |     1      |
         | 0 | 1 | 1 |     0      |
         | 1 | 0 | 0 |     1      |
         | 1 | 0 | 1 |     0      |
         | 1 | 1 | 0 |     1      |
         | 1 | 1 | 1 |     0      |
    * **Boolean Expression**
        - specify boolean function by operating over inputs
        - basic operators are AND (x.y), OR (x + y), NOT (!x)
    * **Canonical Representation**
        - every boolean function can be expressed using AND, OR, NOT
        - for each row with output 1, construct a term created by AND-ing together literals
        (variables or their negations) that fix the values of all row's inputs
        - x = 0, y = 1, z = 0 produce the term !xy!z
        - x = 1, y = 0, z = 0 produce the term x!y!z
        - x = 1, y = 1, z = 0 produce the term xy!z
        - apply OR on terms of rows with output 1 and the result expression is equivalent to
        truth table, (!xy!z + x!y!z + xy!z)
        - NP-hard problem to find the shortest expression equivalent to the given one
    * constant 0 = 0, AND = xy, x AND NOT y = x!y, x = x, NOT x AND y = !xy, y = y
    * XOR = x!y + !xy, OR = x + y, NOR = !(x + y), Equivalence = xy + !x!y, NOT y = !y
    * If y then x (y implies x) = x + !y, NOT x = !x, If x then y (x implies y) = !x + y
    * NAND = !(xy), constant 1 = 1

Boolean Identities
------------------
    * **Identity**
        - x AND 1 = x
        - x OR 0 = x
    * **Null/Domination**
        - x AND 0 = 0
        - x OR 1= 1
    * **Idempotence**
        - NOT(x) AND NOT(x) = NOT(x)
    * **Double Negation**
        - NOT(NOT(x)) = x
    * **Inverse**
        - x AND NOT(x) = 0, Zero property
        - x OR NOT(x) = 1, Unit Property
    * **Commutative**
        - x AND y = y AND x
        - x OR y = y OR x
    * **Associative**
        - (x AND (y AND z)) = ((x AND y) AND z)
        - (x OR (y OR z)) = ((x OR y) OR z)
    * **Distributive**
        - (x AND (y OR z)) = ((x AND y) OR (x AND z))
        - (x OR (y AND z)) = ((x OR y) AND (x OR z))
    * **Absorption**
        - x AND (x OR y) = x
        - x OR (x AND y) = x
    * **De Morgan's**
        - NOT(x AND y) = NOT(x) OR NOT(y)
        - NOT(x OR y) = NOT(x) AND NOT(y)
* maximum decimal number with k bits = 2<sup>k</sup> - 1
* binary -> decimal = &sum; b<sub>i</sub>2<sup>i</sup>

Binary Addition
---------------
    * 01 + 01 = 10, extra 1 is carried to the next position
    * 011+ 001 = 100, 011 + 011 = 110
    * **Overflow**
        - any carry bit that does not fit is ignored, 1 + 1 = 0
        - the result is not the true integer result of addition
        - only get the truncated result

Negative Numbers
----------------
    * 2's Complement, represent -x using positive number 2<sup>n</sup> - x = 1 + (2<sup>n</sup> - 1) - x
    * 2<sup>n</sup> - 1 in binary is all 1s
    * one's complement: flip the bits, two's complement: flip the bits from right to left, stop
    the first time 0 is flipped
    * positive numbers in range 0 to 2<sup>n-1</sup> - 1 and negative numbers in range -1 to -2<sup>n-1</sup>
    * **Example 2's Complement**


        input: 4, output: -4
        4 = 0100
        one's complement: 1111 - 0100 = 1011
        two's complement: 1011 + 1 = 1100


`back to top <#computing-systems>`_

Logic Gates
===========

* `Composite Gates`_, `NAND`_, `NOT`_, `AND`_, `OR`_, `XOR`_, `Multiplexor`_, `Demultiplexor`_
* `Multi-Bit NOT`_, `Multi-Bit AND`_, `Multi-Bit OR`_, `Multi-Bit Multiplexor`_
* `Multi-Way OR`_, `Multi-Way/Multi-Bit Multiplexor`_, `Multi-Way/Multi-Bit Demultiplexor`_
* `Implementing based on NAND`_, `Half Adder`_, `Full Adder`_, `ALU`_
* physical device that implements boolean function
* if function has n inputs and m outputs, the gate will have n input pins and m output pins
* simplest gates are made from tiny switching devices called transistors
* most gates are implemented as transistors etched in silicon, packaged as chips
* primitive gate can be viewed as black box device with elementary logical operation

Composite Gates
---------------
    - designed by interconnecting primitive ones
    - 3-way AND(a, b, c) = AND(AND(a, b), c)
    - XOR(a, b) = OR(AND(a, NOT(b)), AND(NOT(a), b))
* functional: gate implementation will realize its stated interface, in one way or another
* efficiency: do more with less, use as few gates as possible
* usually, given a gate specification/interface, find an efficient way to implement using
already implemented ones

NAND
----
    * one of AND, OR, NOT can be constructed from NAND
        - NOT(x) = x NAND x
        - x AND y = NOT(x NAND y)
        - x OR y = (x NAND x) NAND (y NAND y)
    * since every function can be constructed using the three, all functions can be constructed
    from NAND operations alone
    * **NAND Truth Table**

        | a | b | NAND(a, b) |
        |---|---|------------|
        | 0 | 0 |     1      |
        | 0 | 1 |     1      |
        | 1 | 0 |     1      |
        | 1 | 1 |     0      |

    * **NAND API**


        Chip name: Nand
        Inputs:    a, b
        Outputs:   out
        Function:  If a=b=1 then out=0 else out=1.
        Comment:   This gate is considered primitive and no need to implement it.



NOT
---
    * also called converter, negates its single input
    * **NOT API**


        Chip name: Not
        Inputs:    in
        Outputs:   out
        Function:  If in=0 then out=1 else out=0.



AND
---
    * return 1 when both inputs are 1, 0 otherwise
    * **AND API**


        Chip name: And
        Inputs:    a, b
        Outputs:   out
        Function:  If a=b=1 then out=1 else out=0.



OR
--
    * return 1 when at least one of the inputs is 1, 0 otherwise
    * **OR API**


        Chip name: Or
        Inputs:    a, b
        Outputs:   out
        Function:  If a=b=0 then out=0 else out=1.



XOR
---
    * also called exclusive or, return 1 when two inputs are opposite, 0 when same or sum of
    input bits is even
    * **XOR API**


        Chip name: Xor
        Inputs:    a, b
        Outputs:   out
        Function:  If a!=b then out=1 else out=0.



Multiplexor
-----------
    * three-input gate, use one input, selection bit, to select and output one of the other
    two inputs, data bits
    * the name was adopted from communications systems, where similar devices are used to
    serialize/multiplex several input signals over single output wire
    * **Multiplexor Truth Table**

        | sel | out |
        |-----|-----|
        | 0   | a   |
        | 1   | b   |

    * **Multiplexor API**


        Chip name: Mux
        Inputs:    a, b, sel
        Outputs:   out
        Function:  If sel=0 then out=a else out=b.



Demultiplexor
-------------
    * opposite of multiplexor, take single input and channel it to one of two outputs according
    to selector bit
    * **Demultiplexor Truth Table**

        | sel | a  | b  |
        |-----|----|----|
        | 0   | in | 0  |
        | 1   | 0  | in |

    * **Demultiplexor API**


        Chip name: DMux
        Inputs:    in, sel
        Outputs:   a, b
        Function:  If sel=0 then {a=in, b=0} else {a=0, b=in}


* hardware is designed to operate on multi-bit arrays, buses
* basic requirement of 32-bit computer is to be able to compute AND on two 32-bit buses

Multi-Bit NOT
-------------
    * NOT to every bits in its n-bit input bus
    * **Multi-Bit NOT API**


        Chip name: Not16
        Inputs:    in[16]   // 16-bit pin
        Outputs:   out[16]
        Function:  For i=0..15 out[i]=Not(in[i]).



Multi-Bit AND
-------------
    * AND to every n bit-pairs in its two n-bit input buses
    * **Multi-Bit AND API**


        Chip name: And16
        Inputs:    a[16], b[16]
        Outputs:   out[16]
        Function:  For i=0..15 out[i]=And(a[i], b[i]).



Multi-Bit OR
------------
    * OR to every n bit-pairs in its two n-bit input buses
    * **Multi-Bit OR API**


        Chip name: Or16
        Inputs:    a[16], b[16]
        Outputs:   out[16]
        Function:  For i=0..15 out[i]=Or(a[i], b[i]).



Multi-Bit Multiplexor
---------------------
    * same as binary multiplexor, selector is single bit
    * except two inputs are each n-bit wide
    * **Multi-Bit Multiplexor API**


        Chip name: Mux16
        Inputs:    a[16], b[16], sel
        Outputs:   out[16]
        Funciton:  If sel=0 then for i=0..15 out[i]=a[i]
                   else for i=0..15 out[i]=b[i].


* two-input logic gates can be generalised to multi-way gates that accept arbitrary number of
inputs

Multi-Way OR
------------
    * outputs 1 when at least one of n bit inputs is 1, 0 otherwise
    * **8-Way OR API**


        Chip name: Or8Way
        Inputs:    in[8]
        Outputs:   out
        Function:  out=Or(in[0],in[1],...,in[7]);



Multi-Way/Multi-Bit Multiplexor
-------------------------------
    * m-way n-bit multiplexor select one of m n-bit input buses and outputs it to a single
    n-bit output bus
    * selection is specified by a set of k control bits, where k = log<sub>2</sub>m
    * **4-Way 16-Bit Multiplexor API**


        Chip name: Mux4Way16
        Inputs:    a[16], b[16], c[16], d[16], sel[2]
        Outputs:   out[16]
        Funciton:  If sel=00 then out=a else if sel=01 then out=b
                   else if sel=10 then out=c else if sel=11 then out=d.
        Comment:   The assignment operations mentioned above are all 16-bit.
                   For example, "out=a" means "for i=0..15 out[i]=a[i]".


    * **8-Way 16-Bit Multiplexor API**


        Chip name: Mux8Way16
        Inputs:    a[16], b[16], c[16], d[16],
                   e[16], f[16], g[16], h[16], sel[3]
        Outputs:   out[16]
        Funciton:  If sel=000 then out=a else if sel=001 then out=b
                   else if sel=010 then out=c else if sel=011 then out=d
                   else if sel=100 then out=e else if sel=101 then out=f
                   else if sel=110 then out=g else if sel=111 then out=h.
        Comment:   The assignment operations mentioned above are all 16-bit.
                   For example, "out=a" means "for i=0..15 out[i]=a[i]".



Multi-Way/Multi-Bit Demultiplexor
---------------------------------
    * m-way n-bit demultiplexor channel single n-bit input into one of m possible n-bit outputs
    * selection is specified by a set of k control bits, where k = log<sub>2</sub>m
    * **4-Way 1-Bit Demultiplexor API**


        Chip name: DMux4Way
        Inputs:    in, sel[2]
        Outputs:   a, b, c, d
        Funciton:  If sel=00 then      {a=in, b=c=d=0}
                   else if sel=01 then {b=in, a=c=d=0}
                   else if sel=10 then {c=in, a=b=d=0}
                   else if sel=11 then {d=in, a=b=c=0}.


    * **8-Way 1-Bit Demultiplexor API**


        Chip name: DMux8Way
        Inputs:    in, sel[3]
        Outputs:   a, b, c, d, e, f, g, h
        Funciton:  If sel=000 then      {a=in, b=c=d=e=f=g=h=0}
                   else if sel=001 then {b=in, a=c=d=e=f=g=h=0}
                   else if sel=010 then {c=in, a=b=d=e=f=g=h=0}
                   else if sel=011 then {d=in, a=b=c=e=f=g=h=0}
                   else if sel=100 then {e=in, a=b=c=d=f=g=h=0}
                   else if sel=101 then {f=in, a=b=c=d=e=g=h=0}
                   else if sel=110 then {g=in, a=b=c=d=e=f=h=0}
                   else if sel=111 then {h=in, a=b=c=d=e=f=g=0}



Implementing based on NAND
--------------------------
    * primitive gates can be used to make other gates and chips without worrying about their
    internal design
    * each gate can be implemented in more than one way, the simpler the implementation, the
    better
    * use NAND as base of all hardware
    * ############################################ EDIT THIS #################################
    * NOT: think positive
    * AND: think negative
    * OR/XOR: use simple boolean manipulations
    * Multiplexor/Demultiplexor: use previously built
    * Multi-Bit NOT/AND/OR: construct arrays of n elementary gates, each gate operate
    separately on inputs
    * Multi-Bit Multiplexor: feed same selection bit to every one of n binary multiplexors
    * Multi-Way Gates: think forks
    * ############################################ EDIT THIS #################################

Half Adder
----------
    * add two bits, sum: XOR, carry: AND
    * **Half Adder Truth Table**

        | a | b | sum | carry |
        |---|---|-----|-------|
        | 0 | 0 |  0  |   0   |
        | 0 | 1 |  1  |   0   |
        | 1 | 0 |  1  |   0   |
        | 1 | 1 |  0  |   1   |


Full Adder
----------
    * add three bits, generally can be built using two half adders
    * when building an adder using full adders, use carry look ahead for optimization
    * **Full Adder Truth Table**

        | a | b | c | sum | carry |
        |---|---|---|-----|-------|
        | 0 | 0 | 0 |  0  |   0   |
        | 0 | 0 | 1 |  1  |   0   |
        | 0 | 1 | 0 |  1  |   0   |
        | 0 | 1 | 1 |  0  |   1   |
        | 1 | 0 | 0 |  1  |   0   |
        | 1 | 0 | 1 |  0  |   1   |
        | 1 | 1 | 0 |  0  |   1   |
        | 1 | 1 | 1 |  1  |   1   |


ALU
---
    * Arithmetic Logic Unit, computes a function on two inputs and outputs the result
    * **Trade-Offs**
        - has hardware/software trade-off when implementing ALU
        - deciding which operations are allowed to perform
        - as operations are abstracted from a programmer, to output the correct result is the
        only concern
        - when an operation is designed in hardware, it is faster, but complex and costly
    * **Example ALU Control Bits**
        - control bits are sequential
        - zx: if zx then x=0
        - nx: if nx then x=!x
        - zy: if zy then y=0
        - ny: if ny then y=!y
        - f : if f then out=x+y, else out=x&y
        - no: if no then out=!out
        - zr: if out=0 then zr=1, else zr=0
        - ng: if out<0 then ng=1, else ng=0

`back to top <#computing-systems>`_

Nand2Tetris HDL
===============

* `HDL`_, `Hardware Simulation`_, `Testing`_, `Multi-Bit Buses`_, `Built-In Chips`_, `Sequential Chips`_
* `Visualizing Chips`_, `XOR HDL`_, `Example HDL Programs`_

HDL
---
    * Hardware Description Language, famous ones are VHDL (Virtual HDL) and Verilog
    * functional and declarative specification language
    * can write HDL statements in any order, but conventional to describe from left to right
    * as long as the parts are connected correctly, the chip will function as stated
    * case sensitive, keywords are written in uppercase letters
    * space, newline and comments are ignored
    * used by hardware designers to plan and optimize chip architecture
    * designer specifies the chip structure by writing HDL program, and test it, which is carried
    out using simulation
    * hardware simulator take the HDL program as input and build an image of the modeled chip
    * designer can test the virtual chip on various sets of inputs and outputs are compared to the
    desired results
    * other parameters such as speed of computation, energy consumption and overall cost is also
    measured
    * final optimized HDL program is used as a blueprint for mass production through chip
    fabrication companies
    * **Naming Conventions**
        - names may be any sequence of letters and digits, but cannot start with a digit
        - can include uppercase letters, e.g FullAdder
        - programs are stored in .hdl files
        - chip name declared in HDL statement "CHIP Xxx" must be same as file name "Xxx.hdl"
    * **Statment**
        - specify each part of its name and connection to other parts
        - to write a statement, need to know a complete documentation of underlying parts'
        interfaces
    * **Pins**
        - by default single-bit, multi-bit bus pins can also be declared and used
        - internal pins: create and connect to describe inter-part connections, can be created
        and named at will
        - output pins: names cannot be controlled by programmer, supplied by chips' architects
        and documented in given API
        - pins have fan-in 1 and unlimited fan-out

        .. code-block:: vhdl

           // internal pin v simultaneously feeds three input
   
           chipPart1(..., out=v,...);
           chipPart2(..., in=v,...);
           chipPart3(..., in1=v, int2=v, ...);


    * **Program Structure**
        - interface: chip's API documentation, chip name and names of input and output pins
        - implementation: statements below PARTS keyword, describe names and topology of all
        lower-level parts from which the chip is constructed

        .. code-block:: vhdl

           // Comment to end of line
           /* Comment until closing */
           /** API documentation comment */
   
           CHIP ChipName {
               IN inputPin1, inputPin2,...;
               OUT outputPin1, outputPin2,...;
   
               PARTS:
               // Implementation statements
           }



Hardware Simulation
-------------------
    * process of writing HDL programs is similar to software development
    * instead of using a compiler, hardware simulator is used
    * hardware simulator can parse, interpret HDL code, convert into executable representation
    and test it
    * hardware simulators differ in cost, complexity and ease of use

Testing
-------
    * chips must be tested in specific, replicable, and well-documented fashion
    * hardware simulators can run test scripts, written in some scripting language
    * the script instruct the simulator to use certain inputs to compute outputs and record the
    test results in a file
    * for simple gates, it is easy to write test script to enumerate all possible input values

Multi-Bit Buses
---------------
    * **I/O Bus Pins**
        - bit-widths are specified during declaration, x[n]
    * **Internal Bus Pins**
        - bit-widths are deduced implicitly from the bindings in declaration
        - ``chipPart1(..., x[i]=u, ...);`` defines ``u`` as single-bit internal pin and has value
        x[i]
        - ``chipPart1(..., x[i..j]=v, ...);`` defines ``v`` as internal pin of width ``j-i+1`` bits
        and has values indexed ``i`` to ``j`` (inclusive) of bus-pin ``x``
        - internal pins cannot be subscripted, e.g. u[i] is not allowed
    * **True/False Buses**
        - constants true, 1, and false, 0, can be sued to define buses
        - if ``x`` is 8-bit bus-pin, ``chipPart(..., x[0..2]=true, ..., x[6..7]=true, ...);`` sets
        ``x`` to the value 11000111
        - unaffected bits are set to false, 0, by default
    * **Indexing Internal Bus Pins Workaround**

        .. code-block:: vhdl

           CHIP Foo {
               IN in[16];
               OUT out;
   
               PARTS:
               Not16  (in=in, out[4..11]=notIn);
               Or8Way (in=notIn, out=out);
   
               /* Not16  (in=in, out=notIn);
                  Or8Way (in=notIn[4..11], out=out); will Error */
           }


    * **Multiple Outputs**

        .. code-block:: vhdl

           // Splitting the out value
           CHIP Foo {
               IN in[16];
               OUT out[8];
   
               PARTS:
               Not16  (in=in, out[0..7]=low8, out[8..15]=high8);
               Bar8Bit(a=low8, b=high8, out=out);
           }
   
           // Feeding output to different Parts
           CHIP Foo {
               IN a, b, c;
               OUT out1, out2;
   
               PARTS:
               Bar(a=a, b=b, out=x, out=out1);
               Baz(a=x, b=c, out=out2);
           }



Built-In Chips
--------------
    * native implementation: written in HDL
    * built-in implementation: supplied by executable module written in high-level programming
    language

    .. code-block:: vhdl

       CHIP And16 {
           BUILTIN And16;
       }


    * **Foundation**
        - built-in chips provide supplied implementations of given or primitive chips
    * **Efficiency**
        - when using complex chips, hardware simulator has to evaluate all lower-level chips
        recursively, and it can be slow and inefficient
        - using built-in chip-parts instead of regular HDL-based chips can speed up the
        simulation
    * **Unit Testing**
        - when building a new chip, it is recommended to use built-in chip parts
        - improves efficiency and minimizes errors
    * **Visualization**
        - built-in chips that features a GUI will be displayed whenever it is loaded into the
        simulator
        - chips with GUI can be used just like any other chip
    * **Extension**
        - built-in chips can support to implement new I/O device or hardware platform

Sequential Chips
----------------
    * combinational chips: time-independent, when value of input changes, output change
    instantaneously, all chips are combinational by default
    * sequential chips: time-dependent, also called clocked
    * when input is changed, output of chip may change only at the beginning of the next time
    unit, also called cycle
    * hardware simulator affects the progression of time unit using a simulated clock
    * **Clock**
        - simulator's 2-phase clock emits infinite series of values denoted 0, 0+, 1, 1+, etc.
        - progression of the series is controlled by simulator commands, tick and tock
        - tick moves the value from t to t+, and tock from t+ to t+1
        - simulated time can be fully controlled by the user or a test script
        - in first phase, tick, inputs of each sequential chip affect the chip's internal
        state
        - in second phase, tock, chip outputs are set to new values
    * **Controlling the clock**
        - when a sequential chip is loaded, GUI enables a clock-shaped button
        - one click on the button, a tick, end the first phase of the clock cycle, and a
        subsequent click, a tock, ends the second phase
        - scripting commands ``repeat n {tick, tock, output;}`` advance the clock ``n`` time units
        , and print some values
    * **Sequential Built-In Chips**
        - only built-in chip can depend on the clock explicitly, ``CLOCKED pin,..., pin;``
        - each pin is one of the chip's input or output pins
        - if input pin x is in ``CLOCKED`` list, changes to it should affect outputs only at the
        beginning of the next time unit
        - if output pin x is in ``CLOCKET`` list, changes to any inputs should affect only at the
        beginning of the next time unit
        - when only some of I/O pins are declared as clocked, changes in the non-clocked input
        pins affect the non-clocked output pins instantaneously, e.g. address pins in RAM
        units, addressing logic is combinational, and independent of the clock
        - if ``CLOCKED`` keyword is with empty list, the chip mah change its internal state
        depending on the clock, but I/O will be combinational and independent of the clock

        .. code-block:: vhdl

           /** D-Flip-Flop gate(DFF):
           out[t] = in[t - 1] where t is current cycle, or time-unit. */
   
           CHIP DFF {
               IN in;
               OUT out;
               BUILTIN DFF;
               CLOCKED in;
           }


    * **Sequential Regular Chips**
        - if the chip is not built-in, it is clocked when one or more of its chip-parts is
        clocked
        - clocked property is checked recursively
        - if a built-in chip is explicitly clocked, every chip that depends on it is clocked
        - if a chip is not built-in, there is no way to tell from its HDL code whether it is
        sequential or not
        - chip architect should provide the necessary information in the chip API
    * **Feedback Loops**
        - feedback loop: input of a chip feeds from one of the outputs, directly or through
        path of dependencies
        - data race: instantaneous and uncontrolled dependency between ``in`` and ``out``
        - for each loop, simulator checks if the loop goes through a clocked pin
        - if the loop does not go through a clocked pin, simulator stops processing and error
        to prevent uncontrolled data races

        .. code-block:: vhdl

           /** Not is combinational, DFF is sequential.
               loop1 creates data race.
               in-out dependency by loop2 is delayed by the clock, since in input of DFF
               is declared clocked. out(t) is a function of in(t-1). */
   
           Not(int=loop1, out=loop1); //Invalid feedback loop
           DFF(int=loop2, out=loop2); //Valid feedback loop



Visualizing Chips
-----------------
    * built-in chips with GUI can animate some operations, may include interactive elements
    * user can inspect or change the chip's current state
    * ALU: display Hack ALU's inputs, output and current computed function
    * Registers: display ARegister, DRegister and PC content, user can modify
    * RAM: display scrollable, array-like image showing contents of all memory locations, user
    can modify, GUI is updated if contents change
    * ROM: show array-like image of ROM32K, and icon to enable machine language program from
    external text file
    * Screen: show 256x512 window simulating physical screen, continuous refresh loop is
    embedded to show updated pixels
    * Keyboard: show keyboard icon to connect real keyboard, binary code for pressed key is
    shown in RAM_resident keyboard memory map, keyboard is restored if mouse focus is lost

    .. code-block:: vhdl

       CHIP GUIDemo {
           IN in[16], load, address[15];
           OUT out[16];
   
           PARTS:
           RAM16K  (in=in, load=load, address=address[0..13], out=a);
           Screen  (in=in, load=load, address=address[0..12], out=b);
           Keyboard(out=c);
       }



XOR HDL
-------
    * **HDL Program**

        .. code-block:: vhdl

           /* XOR gate:
              If a<>b out=1 else out=0. */
   
           CHIP Xor {
               IN a, b;
               OUT out;
   
               PARTS:
               Not(in=a, out=nota);
               Not(in=b, out=notb);
               And(a=a, b=notb, out=w1);
               And(a=nota, b=b, out=w2);
               Or(a=w1, b=w2, out=out);
           }


    * **Test Script**

        .. code-block:: tst

           load Xor.hdl
           output-list a, b, out;
           set a 0, set b 0,
           eval, output;
           set a 0, set b 1,
           eval, output;
           set a 1, set b 0,
           eval, output;
           set a 1, set b 1,
           eval, output;



Example HDL Programs
--------------------
    * **3-way Equality**

        .. code-block:: vhdl

           /** check three 1-bit variables
               if all three are equal, output is 1, otherwise 0 */
   
           CHIP Eq3 {
               IN a, b, c;
               OUT out;
   
               PARTS:
               Xor(a=a, b=b, out=neq1);
               Xor(b=b, b=c, out=neq2);
               Or (a=neq1, b=neq2, out=outOr);
               Not(in=outOr, out=out);
           }


`back to top <#computing-systems>`_
