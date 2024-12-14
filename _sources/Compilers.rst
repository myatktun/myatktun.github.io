=========
Compilers
=========

1. `Basics`_
2. `Structure`_
3. `Optimization`_

`back to top <#compilers>`_

Basics
======

* `Interpreter`_, `Preprocessor`_, `Assembler`_, `Linker`_, `Loader`_
* `Tools`_, `Language Evolution`_
* compilers translate a program into computer executable form
* also called a language processor
* read a source language and translate into a target language
* compilers must report errors during translation
* if a program is translated directly into machine executable form, it can be called by user
* machine program by compiler is faster than interpreter at mapping inputs to outputs

Interpreter
-----------
    * another kind of language processor
    * execute the source program directly, instead of translating
    * can give better error diagnostics than a compiler
    * e.g Java source code is first compiled into bytecodes, which is then interpreted by a
    virtual machine
    * bytecodes from one machine can be interpreted on another
    * some Java compilers, just-in-time compilers, compile bytecodes into machine language
    before running for faster processing of inputs to outputs

Preprocessor
------------
    * collect source program stored in separate files
    * may also expand macros
* after preprocessor, compiler may produce assembly-language program, which is easier to
produce and debug

Assembler
---------
    * process the assembly language from compiler
    * produce relocatable machine code

Linker
------
    * link relocatable object files and library files, as large programs are compiled in pieces
    * resolve external memory addresses, where code in one file refer to a location in another

Loader
------
    * put all executable object files into memory for execution

Tools
-----
    * can use modern software development environments
    * also have specialized tools to help implement phases of a compiler
    * parse generator: auto produce syntax analyzers from grammatical description of language
    * scanner generator: produce lexical analyzers from regex description of tokens of language
    * syntax-directed translation engine: produce collections of routines for walking a parse
    tree and generating intermediate code
    * code-generator generator: produce code generator from collection of rules for translating
    intermediate language into machine language
    * data-flow analysis engine: help gathering information about how values are transmitted
    from one part of program to another, key part of code optimization
    * compiler-construction toolkit: provide integrated set of routines for compiler phases
    construction

Language Evolution
------------------
    * 1st gen: machine languages
    * 2nd gen: assembly languages
    * 3rd gen: higher level languages, e.g. Fortran, C, Java
    * 4th gen: languages designed for specific applications, e.g. NOMAD, SQL, Postscript
    * 5th gen: logic and constraint based languages, e.g. Prolog, OPS5
    * Imperative language program specifies how computation is to be done, e.g. C, Java
    * Declarative language: program specifies what computation is to be done, e.g. Prolog
    * von Neumann language: computational model is based on von Neumann computer architecture
    * Object-oriented language: that support object-oriented programming, e.g. Java, Ruby
    * Scripting language: interpreted languages with high-level operators, programs are shorter
    than equivalent ones written in languages like C, e.g. Python, JavaScript
    * compiler writers have to track new language features and design optimal translation
    algorithms for new hardware
    * performance of a computer system is so dependent on compiler technology

`back to top <#compilers>`_

Structure
=========

* `Analysis`_, `Synthesis`_, `Phases`_, `Lexical Analysis`_, `Syntax Analysis`_, `Semantic Analysis`_
* `Intermediate Code Generation`_, `Code Optimization`_, `Code Generation`_, `Symbol-Table Management`_

Analysis
--------
    * often called front end of compiler
    * break the source program, apply grammatical structure and create intermediate
    representation
    * must inform syntactic or semantic errors
    * also collect information from source program to store in a symbol table
    * intermediate representation and symbol table are passed to synthesis part

Synthesis
---------
    * often called back end of compiler
    * constructs target program from intermediate representation and symbol table
* the symbol table is used by all phases of the compiler

Phases
------


          character stream
                |
         * -----------------
        | Lexical Analyzer |
         * -----------------
                |
           token stream
                |
         * -----------------
        | Syntax Analyzer |
         * -----------------
                |
           syntax tree
                |
         * -----------------
        | Semantic Analyzer |
         * -----------------
                |
          syntax tree
                |
         * -----------------
        | Intermediate Code |                 front-end
        | Generator         | -------------------------------------------------
         * -----------------
                |
      intermediate representation
                |
         * --------------------
        | Machine-Independent | (only some compilers have this phase)
        | Code Optimizer      | (for backend to produce better target program)
         * --------------------
                |
      intermediate representation
                |
         * ---------------                     back-end
        | Code Generator | ----------------------------------------------------
         * ---------------
                |
        target-machine code
                |
         * ------------------
        | Machine-Dependent |
        | Code Optimizer    |
         * ------------------
                |
        target-machine code


    * optimization is optional and one of the optimization phases can be omitted
    * example translation


               a = b + c * 9
                    |
             * -----------------
            | Lexical Analyzer |
             * -----------------
                    |
        <id, 1> <=> <id, 2> <+> <id, 3> <*> <9>
                    |
             * -----------------
            | Syntax Analyzer |
             * -----------------
                    |
                 =
               /   \
        <id, 1>    <+>
                  /   \
            <id, 2>    <*>
                      /   \
                  <id, 3>  9
                    |
             * -----------------
            | Semantic Analyzer |
             * -----------------
                    |
                 =
               /   \
        <id, 1>    <+>
                  /   \
            <id, 2>    <*>
                      /   \
                  <id, 3>  inttofloat
                             |
                             9
                    |
             * -----------------
            | Intermediate Code |
            | Generator         |
             * -----------------
                    |
                t1  = inttotfloat(9)
                t2  = id3 * t1
                t3  = id2 + t2
                id1 = t3
                    |
             * --------------------
            | Code Optimizer      |
             * --------------------
                    |
                t1  = id3 * 9.0
                id1 = id2 + t1
                    |
             * ---------------
            | Code Generator |
             * ---------------
                    |
              LDF  R2, id3
              MULF R2, R2, #9.0
              LDF  R1, id2
              ADDF R1, R1, R2
              STF  id1, R1



Lexical Analysis
----------------
    * first phase of compiler
    * read stream of characters and group them into meaningful sequences called lexemes
    * **Lexeme**
        - a token, ``<token-name, attribute-value>``, is made from each lexeme
        - token-name: abstract symbol to use during syntax analysis
        - attribute-value: points to a symbol table entry, which is needed for semantic
          analysis and code generation
        - symbol table entry for identifier has information such as name and type
    * blanks separating the lexemes are discarded by lexical anaylzer


    This is just an example

    a = b + c * 9

    "lexeme -> token"

    a -> <id, 1>
    = -> < = >
    b -> <id, 2>
    + -> < + >
    c -> <id, 3>
    * -> < * >
    9 -> < 9 >



Syntax Analysis
---------------
    * second phase of compiler, also called parsing
    * **Syntax Tree**
        - tree-like intermediate representation created from the tokens made by lexical
          anaylzer
        - each node is an operation and the arguments of the operation as the children


    This is just an example

       a     =     b     +     c     *   9
    <id, 1> <=> <id, 2> <+> <id, 3> <*> <9>

    syntax tree

             =
           /   \
    <id, 1>    <+>
              /   \
        <id, 2>    <*>
                  /   \
              <id, 3>  9



Sematic Analysis
----------------
    * semantic analyzer use syntax tree and symbol table to check for semantic consistency with
    the language definition
    * gather type information and save it in syntax tree or symbol table to be used by later
    phases
    * important part is type checking, that each operator has matching operands
    * coercion: type conversion, e.g converting integer to float in binary operator

Intermediate Code Generation
----------------------------
    * compiler may generate one or more intermediate representations
    * most generate low-level or machine-like representation
    * an intermediate representation should be easy to produce and translate to target machine
    * **Three-address Code**
        - consist sequence of assembly-like instructions with three operands per instruction
        - each assignment instruction has at most one operator on right side, fixing the order
          of operations
        - compiler must generate temporary name to hold value computed by instruction
        - some instructions have fewer than three operands, e.g ``id1 = t3``

Code Optimization
-----------------
    * machine-independent optimization phase try to improve the intermediate code
    * different compilers perform different optimizations
    * optimizing compilers spend most time on this phase
    * simple optimizations improve running time of the target program without slowing down the
    compilation too much

Code Generation
---------------
    * generator take intermediate representation as input and map it into target language
    * for machine code target, registers or memory locations are selected for each variables
    and intermediate instructions are translated into machine instructions
    * assignment of registers is important part of code generation
    * storage organization at run-time depends on the language being compiled
    * storage-allocation decisions are made during intermediate code generation or during code
    generation

Symbol-Table Management
-----------------------
    * important for compiler to record variable names used in source program and collect
    information about various attributes of each
    * symbol table is a data structure with record for each variable name and its attributes
    * should be designed to allow the compiler to find records and store or retrieve
    data from records quickly
* activities from phases may be grouped together into a pass that reads input file and writes
output file
    * front-end phases might be grouped together as one pass
    * code optimization might be optional pass
    * back-end phases might be grouped together as one pass
* can use compiler collections to create various compilers
    * different front ends and one backend: different source languages, one target machine
    * one front end and different backends: one source language, different target machines

`back to top <#compilers>`_

Optimization
============

*

`back to top <#compilers>`_
