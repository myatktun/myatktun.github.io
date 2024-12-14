==========
TypeScript
==========

1. `What is TypeScript?`_
2. `TSC`_
3. `Types`_
4. `Functions`_
5. `Objects`_
6. `Classes`_
7. `Structural Type System`_

What is TypeScript?
===================

* compiled language built on top of JS to address shortcomings (static typing, code completion,
  refactoring, shorthand notations)
* some drawbacks over JS: transpilation (have to compile to JS), discipline in coding
* can be used on both front-end and backend

`back to top <#typescript>`_

TSC
===
* to compile ``.ts`` files to ``.js``

    .. code-block:: sh

       npm i -g typescript
       tsc index.ts # output index.js in same directory as .ts file
       tsc --watch index.ts # continuously watch index.ts and compile on save



tsconfig.json
-------------
    * specifies compiler (tsc) options, placed at root directory
    * ``tsc --init`` to create

    .. code-block:: json

       {
           "compilerOptions": {
               "target": "es2015",
               "module": "commonjs",
               "rootDir": "./src",
               "sourceMap": true,
               "outDir": "./dist",
               "removeComments": true,
               "noEmitOnError": true,
               "esModuleInterop": true,
               "forceConsistentCasingInFileNames": true,
               "strict": true,
               "noUnusedParameters": true,
               "skipLibCheck": true
           }
       }


    .. code-block:: sh

       tsc # compile .ts files in /src and output .js files in /dist


`back to top <#typescript>`_

Types
=====

* core types
    * number, string, boolean, enum, unknown
    * object (not specific, properties can be any)
    * Array, Tuple
    * Function, never
* can use (_) to separate digits for large numbers

    .. code-block:: ts

       let x: number = 123_456_789


* does not always need to annotate variables

    .. code-block:: ts

       let x = 123_456_789 // x is type int
       x = 'hello' // error



any
---
    * default type when a variable is declared without value
    * can set variable to different type later
    * should avoid using

    .. code-block:: ts

       let x // has type 'any'
       x = 123
       x = 'hello'



arrays
------
    * has to annotate if empty array, or will default to any

    .. code-block:: ts

       let numbers: number[] = []



tuples
------
    * fixed length array with each element having particular type, although can use ``push()``
    * represented as regular JS arrays

    .. code-block:: ts

       let numbers: [number, string] = [1, '2']
       let numbers: [number, string] = [1, 2] // error
       let numbers: [number, string] = [1, '2', 3] // error
       numbers.push(3) // compiler will not show error
       numbers = [1, '2', 3] // error
   
       // tuples as array elements
       let numbers: [number,string][] = [
           [1, 'hello'],
           [2, 'world']
       ]



enum
----
    * numeric values by default

    .. code-block:: ts

       enum Numbers { a, b, c} // a=0, b=1, c=3
       enum Numbers { a = 3, b, c} // a=3, b=4, c=5
       enum Numbers { a = 'x', b = 'y', c = 'z'} // need to initialize if string values
       const enum Numbers { a, b, c} // using 'const' will optimize generated JS code



generics
--------
    * used to build reusable components

    .. code-block:: ts

       function getArray<T>(items: T[]): T[] {
           return new Array().concat(items)
       }
   
       let numArray = getArray<number>([1, 2, 3])
       let strArray = getArray<string>(['a', 'b', 'c'])
   
       numArray.push('d') // error
       strArray.push('d') // valid



union
-----
    * can be more than one type
    * might need runtime checks

    .. code-block:: ts

       // x is union type
       function myFunc(x: number | string): number {
           // Narrowing
           if(typeof x === 'number') return x
           return parseInt(x)
       }
   
       // both no error
       myFunc(10)
       myFunc('2')



intersection
------------
    * having two types at the same time

    .. code-block:: ts

       type Student = {
           name: string
       }
   
       type Tutor = {
           id: number
       }
   
       // Person is intersection type
       type Person = Student & Tutor
   
       // john have both properties
       let john: Person = {
           name: 'John',
           id: 123
       }


literals
--------
    * exact and specific values

    .. code-block:: ts

       let x: 10 | 20 = 10 // valid
       let x: 10 | 20 = 20 // valid
       let x: 10 | 20 = 32 // error, x can only be 10 or 20
   
       type MyLiteral = 10 | 20
   
       let x: MyLiteral = 10



null
----
    * assigning ``null`` is valid in JS
    * by default, TS is strict about using ``null``

    .. code-block:: ts

       function myFunc(x: number) {
           console.log(x)
       }
   
       myFunc(null) // error


    * allow ``null`` in tsconfig

        .. code-block:: json

           {
               "strictNullChecks": false
           }


    * telling tsc variable will never get ``null``

        .. code-block:: ts

           const input = document.getElementById("num1")!



unknown
-------
    * stricter than ``any``
    * different from ``any`` is that ``unknown`` type variable cannot be reassigned to other variable
      types
    * must be checked before reassigning to other variables
    * useful when know what to do with the variable after checking

    .. code-block:: ts

       let x: unknown
       let y: string
   
       x = "hello" // OK
       x = 10  // OK
       y = x   // error


* type assertion
    * tell the compiler to treat an entity as different type
    * weakens type safety

    .. code-block:: ts

       let x: any = 1
       let y = <number>x
       // or
       let y = x as number
   
       y = 'hello' // error
   
       const input = document.getElementById("num1")! as HTMLInputElement


`back to top <#typescript>`_

Functions
=========

* all functions return types should be annotated
* all JS functions default return type is ``undefined``

.. code-block:: ts

   // 'y?: number' means y is optional when calling function
   function myFunc(x: number, y?: number): number {
       return 1; // returning other will give error
   }
   
   // no return
   function myFunc(x: number): void {
       console.log(x)
   }



Function
--------

    .. code-block:: ts

       function add(n1: number, n2: number) {
           return n1 + n2
       }
   
       function justPrint(n1: number): void {
           console.log(n1)
       }
   
       let MyFunc: (x: number, y: number) => number // MyFunc can be any function that accepts two
                                                    // numbers and return a number
       MyFunc = add // OK
       MyFunc = justPrint // error


    * using with ``interface``

        .. code-block:: ts

           interface MyFunc {
               (x: number, y: number): number
           }
   
           const f1: MyFunc = (x: number, y:number): number => x + y
           const f1: MyFunc = (x: number, y:string): number => x + y // error
           const f2: MyFunc = (x: number, y:number): number => x - y // f1 & f2 using same interface



never
-----
    * never produces return value
    * useful for utility functions

    .. code-block:: ts

       // omitting 'never' will set to 'void' but functions the same
       function error(msg: string): never {
           throw {message: msg}
       }
   
       error("ERROR")
       const result = error("ERROR")   // result doesn't have value


`back to top <#typescript>`_

Objects
=======

* can annotate each key,value types

    .. code-block:: ts

       let myObject: {
           readonly id: number
           name: string
           age?: number // age is optional when initializing
       } = { id: 1, name: 'hello'}
   
       myObject.id = 2 // error


* adding methods in object

    .. code-block:: ts

       let myObject: {
           objectFunc: (x: number) => void
       } = {
           objectFunc: (x: number) => {
               console.log(x)
           }
       }


* defining shape of the object separately
    * using ``type``
    * can be used with primitives, union

        .. code-block:: ts

           type MyObject = {
               readonly id: number
               name: string
               objectFunc: (x: number) => void
           }
   
           let myObject: MyObject = {
               id: 1,
               name: 'hello',
               objectFunc: (x: number) => {
                   console.log(x)
               }
           }
   
           type MyType = number | string // valid


    * using ``interface``

        .. code-block:: ts

           interface MyObjectInterface = {
               readonly id: number
               name: string
               objectFunc: (x: number) => void
           }
   
           let myObject: MyObjectInterface = {
               id: 1
               name: 'hello'
               objectFunc: (x: number) => {
                   console.log(x)
               }
           }
   
           interface MyType = number | string // error


    * should prefer ``interface``, only use ``type`` when specific features are needed

* optional access operators
    * property access

        .. code-block:: ts

           type MyObject = { name: string}
   
           function getObjectProperty(id: number): MyObject | null | undefined {
               return id === 0 ? null : { name: 'hello' }
           }
   
           let x = getObjectProperty(2)
   
           console.log(x.name) // error as return type can be null
   
           console.log(x?.name) // valid using optional property access
           console.log(x?.name?.toUpperCase()) // can chain multiple without error


    * also has **optional element access operator** and **optional call**

`back to top <#typescript>`_

Classes
=======

* data is public by default

.. code-block:: ts

   class MyClass {
       private id: number
       name: string
       protected age: number
   
       constructor(id: number, name: string) {
           this.id = id
           this.name = name
       }
   }


* using with ``interface``

    .. code-block:: ts

       interface MyInterface {
           id: number
           name: string
           method1(): string
       }
   
       class MyClass implements MyInterface {
           id: number
           name: string
   
           constructor(id: number, name: string) {
               this.id = id
               this.name = name
           }
   
           method1() {
               retunr 1 // error, only to return string
           }
       }


* extending classes

    .. code-block:: ts

       class MyClass2 extends MyClass {
           age: number
   
           constructor(id: number, name: string, age: number){
               super(id, name) // id & name are in super class
               this.age = age
           }
       }


`back to top <#typescript>`_

structural Type System
======================

* if two objects have the same shape, they are considered to be of same type
* only require a subset of the object's fields to match
* if object or class has all the required properties, both match

.. code-block:: ts

   interface Point {
       x: number
       y: number
   }
   
   // 'p' has same shape as 'Point'
   const point = { x: 1, y: 2 }
   
   // `newP` has the same shape as 'Point'
   const newP = { x: 1, y: 2, z: 3}
   
   class myP {
       x: number
       y: number
   
       constructor(x: number, y: number) {
           this.x = x
           this.y = y
       }
   }
   
   // 'mynewP' has the same shape as 'Point'
   const mynewP = new myP(1, 2)


`back to top <#typescript>`_
