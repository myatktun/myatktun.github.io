====
C# #
====

1. `What is C#?`_
2. `Basics`_

`back to top <#c>`_

What is C#?
===========

*

`back to top <#c>`_

Basics
======

* basic example program

    .. code-block:: csharp

       using System;
       using System.Collections.Generic;
       using System.Linq;
       using System.Text;
       using System.Threading.Tasks;
   
       namespace basics
       {
           class Program
           {
               /* code execution start from Main()*/
               static void Main(string[] args)
               {
                   Console.WriteLine("hello world");
                   Console.ReadLine();
               }
           }
       }



Output
------

    .. code-block:: csharp

       // print and stay on same line
       Console.Write();
   
       // print and go to new line
       Console.WriteLine();



Input
-----

    .. code-block:: csharp

       // store input as string
       Console.ReadLine();



Declare Variables
-----------------

    .. code-block:: csharp

       string x = "hello";
       char c = ' ';
       int y;
       y = 2;
       Console.WriteLine(x + c + y); // concatenation



Data Types
----------
    * char, string, int, bool
    * float, double, decimal (most accurate)

String
------

    .. code-block:: csharp

       string x = "hello world";
       // x.Length, x.ToUpper(), x.ToLower()
       // x.Contains('h'), x[2]
       // x.IndexOf('o'), x.IndexOf("llo") returns index of starting 'l', -1 if not found
       // x.Substring(2) start from index 2, inclusive, to end
       // x.Substring(2, 4) start from index 2 and 3 characters behind



Numbers
-------

    .. code-block:: csharp

       // Math.Abs(-40), Math.Pow(2.2, 3), Math.Sqrt(4)
       // Math.Max(4, 44), Math.Min(23, 12), Math.Round(4.12)
       // Math.Floor(4.9), Math.Ceiling(4.2)
       int x = Convert.ToInt32("13"); // convert string to int



Arrays
------

    .. code-block:: csharp

       // type[] name = {values};
       int[] x = { 1, 2, 3 };
       string[] y = new string[10]; // create empty array with specific number of elements


    * 2-dimensional array

        .. code-block:: csharp

           int[,] x = {
               {1, 2},
               {3, 4}
           }
   
           // declare empty 2D array
           int[,] y = new int[row, col]



Methods
-------

    .. code-block:: csharp

       namespace basics
       {
           class Program
           {
               // method is declared outside Main() and name is capitalized by convention
               // void: no return
               static void Hello(string x)
               {
                   Console.WriteLine("hello " + x);
               }
   
               // return an integer
               static int Number()
               {
                   return 2;
               }
   
               static void Main(string[] args)
               {
                   Hello("user");
                   Console.WriteLine(Number());
               }
           }
       }



Conditionals
------------
    * logical conditions: <, >, <=, >=, ==, !=, &&, ||

    .. code-block:: csharp

       if(condition){ /* code */}
       else if(condition){ /* code */ }
       else { /* code */ }
   
       ternary = (condition) ? True :  False;
   
       switch (expression)
       {
           case a:
               // code
               break;
           case b:
               // code
               break;
           default:
               // code
               break;
       }



Loops
-----
    * while

        .. code-block:: csharp

           while(condition)
           {
               // code
           }
   
           do
           {
               // code
           } while(condition);


    * for

        .. code-block:: csharp

           for (initial; condition; iterate)
           {
               // code
           }



Exceptions
----------

    .. code-block:: csharp

       try
       {
           // code that'll break
       }
       catch (exceptionType e)
       {
           // handle error
       }
       finally
       {
           // default handle
       }



Classes & Objects
-----------------
    * allow to create custom data types based on primitive ones
    * class names are capitalized by convention
    * Objects are instances of a Class

    .. code-block:: csharp

       // MyClass.cs
       class MyClass
       {
           // attributes
       }


    * **constructor**
        - special method, called whenever an object is created
        - same name as the class

        .. code-block:: csharp

           // MyClass.cs
           class MyClass
           {
               // attributes
   
               public MyClass(arguments)
               {
                   // code
               }
           }


    * **object methods**
        - methods defined in the class and used by it

        .. code-block:: csharp

           class MyClass
           {
               // attributes
   
               public MyClass() { }
   
               private void method2()
               {
                   // can only be called inside the class
               }
   
               public void method1()
               {
                   // can be called outside of the class
               }
   
           }


    * **getter & setter**
        - methods to control access to the attributes, make classes more secure
        - name is capitalized and same as attribute

        .. code-block:: csharp

           class MyClass
           {
               // attributes
   
               public MyClass(argument)
               {
                   // use setter to set attribute value
                   Attribute1 =  argument
               }
   
               public string Attribute1
               {
                   get { return attribute1; }
                   set {
                       // validate data, value will be any passed value
                       if(value == someValue)
                       {
                           attribute1 = value;
                       }
                       else
                       {
                           attribute1 = defaultValue;
                       }
                   }
               }
           }


    * **static attributes**
        - shared by all objects and instances of the class
        - contained on the class and specific to it, instead of each object
        - not unique from object to object
        - can be directly accessed through the class
        - use methods to be accessed by objects

        .. code-block:: csharp

           class MyClass
           {
               // every object's attribute1 will be 99
               // can be accessed with MyClass.attribute1
               public static int attribute1 = 99;
   
               public MyClass(argument)
               {
                   // can use constructor to modify value everytime an object is created
                   attribute1++;
               }
   
               // method to be accessed by objects
               public int getAttribute1()
               {
                   return attribute1;
               }
           }


    * **static methods**
        - belong to the class itself
        - do not need to create object to use the method, e.g ``Math.Sqrt()``
        - name is capitalized by convention
        - can be directly accessed through the class

        .. code-block:: csharp

           class MyClass
           {
               public MyClass(argument) { }
   
               public static void Method1()
               {
                   // code
               }
           }


    * **static classes**
        - prevent to create object of the class, e.g ``Math`` class

        .. code-block:: csharp

           static class MyClass {}
   
           // error
           MyClass class1 = new MyClass();


    * **inheritance**
        - inherit all attributes and methods of the parent class or classes
        - do not inherit private ones
        - child class can override the parent's

        .. code-block:: csharp

           class MyClass
           {
               // child can override
               public virtual void Method1() {}
           }
   
           class MyChildClass : MyClass
           {
               public override void Method1()
               {
                   // new code
               }
           }


`back to top <#c>`_
