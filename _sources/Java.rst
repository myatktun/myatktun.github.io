====
Java
====

1. `Basics`_
2. `Classes`_
3. `Object Oriented Programming`_
4. `Collections Framework`_
5. `Generics`_
6. `Serialization`_
7. `Networking`_
8. `Threads`_
9. `Javadoc`_
10. `Java8`_
11. `JVM`_
12. `References & External Resources`_

`back to top <#java>`_

Basics
======

* `Conventions`_, `Modifiers`_, `Variables`_, `Data Types`_, `Enums`_, `Operators`_, `Loops`_, `Switch`_
* `Numbers`_, `Characters`_, `Strings`_, `Arrays`_, `Date/Time`_, `Regex`_
* `Methods`_, `I/O`_, `Exceptions`_, `Interfaces`_, `Packages`_
* write once, run anywhere, can run on all Java supported platforms without recompilation
* four platforms for Java: Standard Edition (Java SE), Enterprise Edition (Java EE), Micro
  Edition (Java ME), Java FX
* multi-threaded, interpreted, high performance (uses JIT compilers), distributed and dynamic
* object oriented: everything is object in Java
* first compiled into platform independent byte code, which is then interpreted by JVM
* object: has states and behaviors, is an instance of a class
* class: template/blueprint that describes state that objects of its type supports
* method: a behavior, a class can contain many behaviors


Conventions
-----------
    * identifiers are case sensitive and start with letter, $ or _
    * Upper camel case for classes and interfaces, lower camel case for methods and variables
    * name of the file should match the class name

Modifiers
---------
    * **Access modifiers**
        - default: visible to the package, no modifiers
        - private: visible to the class only
        - public: visible to the world
        - protected: visible to the package and all subclasses
    * **Non-access modifiers**
        - static: creating class methods and variables
        - final: finalizing the implementation of classes, methods and variables
        - abstract: for abstract classes and methods
        - strictfp: strict floating-point, to get same floating-point result on any platforms
        - synchronized: for threads, guarantees both atomicity and visibility, can be applied
          to blocks or methods
        - volatile: for threads, guarantees visibility, can only be applied to variables

Variables
---------
    * **Local**
        - declared in methods, constructors or blocks and visible only within
        - destroyed once it exits the block
        - cannot use access modifiers
        - implemented at stack level
        - no default value and must be initialized
    * **Instance/Non-static**
        - declared in a class, but outside a method, constructor or any block
        - created when object is created and destroyed as well
        - when object is created in the heap, space for each variable is also allocated in it
        - can be declared in class level before or after use
        - access modifiers can be given
        - visible for all methods, constructors and blocks in the class
        - have default values, but can be assigned during declaration or in constructor
        - can be accessed directly inside the class, but should called using ``Object.Variable``
          in static methods
    * **Class/Static**
        - declared with static keyword in class, but outside a method, constructor or block
        - only one copy per class, no matter how many objects are created
        - rarely used other than as constants
        - stored in static memory
        - created when program starts and destroyed when it stops
        - similar visibility to instance variables, but mostly declared public for users of the
          class
        - same default values as instance variables, and can be assigned in static initializer
          blocks
        - can be called using ``ClassName.VariableName``
        - should be all upper case when declaring as ``public static final``

Data Types
----------
    * **Primitive**
        - byte: 8-bit signed
        - short: 16-bit signed
        - int: 32-bit signed
        - long: 64-bit signed
        - float: single-precision 32-bit
        - double: double-precision 64-bit
        - boolean: 1-bit true/false
        - char: single 16-bit Unicode
    * **Reference/Object**
        - created using constructors of classes, to access objects
        - class objects and various type of array variables are reference data type
        - default value is null
    * **Literals**
        - decimal, hexadecimal, octal
        - string

Enums
-----
    * restrict a variable to have one of only predefined values
    * enums are classes and should follow the conventions for classes

    .. code-block:: java

       enum Level {LOW, MEDIUM, HIGH}
       Level l; // l can be only one of the 3 values



Operators
---------
    * **Arithmetic**
        - +, -, &ast;, /, %, ++, --
    * **Relational**
        - ==, !=, >, <, >=, <=
    * **Bitwise**
        - &, \|, ^, ~ (complement), <<, >>, >>> (zero fill right shift)
    * **Logical**
        - &&, ||, !
    * **Assignment**
        - =, +=, -=, \*=, /=, %=, <<=, >>=, &=, ^=, \|=
    * **Misc**
        - ?: (conditional)
        - ``instance of`` (only for object reference variables, check object is of or compatible
          with particular type)

Loops
-----
    * **while**

        .. code-block:: java

           while (true) {
               // do this
           }


    * **for**

        .. code-block:: java

           // update statement can be left blank
           for (int i = 0; i < 9; ++i) {
               if (i == 2)
                   continue; // skip the body
   
               // do this
   
               if ( i == 4)
                   break; // break out of loop
           }
   
           // foreach loop, since Java 5
           for (int x : intArray) {
               // do this
           }


    * **do...while**

        .. code-block:: java

           // execute at least once
           do {
               // do this
           } while (true);


    * loop control statements: ``break``, ``continue``

Switch
------
    * values must be of an int, byte, short, char, strings and enums
    * value for a case must be same data type as the one in the switch, must be constant or
      literal
    * not every case needs to contain ``break``

    .. code-block:: java

       int a = 2;
   
       switch (a) {
           case 1:
               // do this
               break;
           case 2:
               // do this
               break;
           default : // optional
               // do this
       }



Numbers
-------
    * wrapper classes such as Integer, Long, Byte, Double, Float, Short are subclasses of the
      abstract class Number
    * boxing: converting primitive data types into object
    * unboxing: converting wrapper object back to primitive data type
    * the compiler takes care of boxing and unboxing
    * ``Number`` class is part of ``java.lang`` package

    .. code-block:: java

       Integer x = 5; // box int to Integer object
       x = x + 10; // unbox Integer to int


    * ``xxxValue()``
        - convert value of the Number object to primitive data type and return
        - byte, short, int, long, float, double
        - ``x.byteValue()``
    * ``compareTo()``
        - compare Number object to the argument
        - two different types cannot be compared
        - return 1 if greater, 0 if equal, -1 if less than the argument
        - ``x.compareTo(3)``
    * ``equals()``
        - determine if Number object is equal to the argument object
        - argument can be of any object
        - return true if argument is not null and is an object of same type with same numeric
          value
        - extra requirements for Double and Float
        - ``x.equals(y)``
    * ``valueOf()``: return relevant Number Object holding the value of the argument passed,
      argument can be primitive data type, String, etc.

        .. code-block:: java

           Integer.valueOf(9); // 9
           Double.valueOf(9); // 9.0
           Float.valueOf("80"); // 80.0
   
           // 16 is called radix, to decide the value of returned Integer based on the String
           Integer.valueOf("444", 16); // 1092


    * ``toString()``: to get String object with value of Number object, take primitive data type
      as an argument and return String object, `x.toString()` or `Integer.toString(12)`
    * ``parseXxx()``: to get primitive data type of certain String, is a static method and can
      have one argument or two

        .. code-block:: java

           Integer.parseInt("9"); // 9
           Double.parseDouble("5") // 5.0
           Integer.parseInt("444,", 16) // 1092


    * ``abs()``: return absolute value of argument, that can be int, float, long, double, short,
      byte, `Math.abs(-8)` return `8`
    * ``ceil()``: return smallest integer greater than or equal to the argument,
      ``Math.ceil(100.82)`` return ``101.0``
    * ``floor()``: return largest integer less than or equal to the argument, ``Math.floor(100.82)``
      return `100.0`
    * ``rint()``: return integer that is closest in value to the argument, ``Math.rint(100.82)``
      return `101.0` and `Math.rint(100.20)` return `100.0`
    * ``round()``: return closest long or int, ``Math.round(100.5)`` return ``101.0``
    * ``min()``: return smaller of two arguments, which can be int, float, long, double,
      ``Math.min(1.3, 2)`` return ``1.3``
    * ``max()``: return maximum of two arguments, which can be int, float, long, double,
      ``Math.max(1.3, 2)`` return ``2.0``
    * ``exp()``: return e to the power of the argument, ``Math.exp(2)`` is e<sup>2</sup> and
      ``Math.E`` return Euler's number
    * ``log()``: return natural logarithm of argument, ``Math.log(Math.E)`` return ``1.0``
    * ``pow()``: return value of first argument raised to the power of second, ``Math.pow(2, 2)``
      return `4.0`
    * ``sqrt()``: return square root of argument, ``Math.sqrt(2)``
    * ``sin()``: return sine of specified double value, ``Math.sin(2.0)``
    * ``cos()``: return cosine of specified double value: ``Math.cos(2.0)``
    * ``tan()``: return tangent of specified double value, ``Math.tan(2.0)``
    * ``asin()``: return arcsine of specified double value, ``Math.asin(Math.sin(2.0))``
    * ``acos()``: return arccosine of specified double value, ``Math.acos(Math.cos(2.0))``
    * ``atan()``: return arctangent of specified double value, ``Math.atan(Math.tan(2.0))``
    * ``atan2()``: convert rectangular coordinates ``(x, y)`` to polar coordinate ``(r, theta)``,
      ``Math.atan2(1, 2)``
    * ``toDegrees()``: convert argument to degrees, ``Math.toDegrees(45.0)``
    * ``toRadians()``: convert argument to radians, ``Math.toRadians(45.0)``
    * ``random()``: to generate random number between ``0.0`` and ``1.0`` (exclusive), ``Math.random()``

Characters
----------
    * wrapper class for primitive data type char
    * has methods to manipulate characters
    * autoboxing: compiler auto convert to object if necessary

    .. code-block:: java

       Character ch = 'a';
   
       char c = test('x'); // primitive 'x' is autoboxed and return is unboxed


    * **escape sequences**
        - \t (tab), \b (backspace), \n, \r (carriage return), \f (form feed), \', \", \\

    * ``isLetter()``: true if char is a letter, ``Character.isLetter('a')``
    * ``isDigit()``: true if char is a digit, ``Character.isDigit('5')``
    * ``isWhitespace()``: true if char is space, tab or new line, ``Character.isWhitespace('\t')``
    * ``isUpperCase()``: true if char is uppercase, ``Character.isUpperCase('A')``
    * ``isLowerCase()``: true if char is lowercase, ``Character.isUpperCase('a')``
    * ``toUpperCase()``: return uppercase form, ``Character.toUpperCase('a')``
    * ``toLowerCase()``: return lowercase form, ``Character.toLowerCase('A')``
    * ``toString()``: return one-character String object, ``Character.toString('a')``

Strings
-------
    * to create and manipulate strings
    * has 11 constructors to provide initial value using different sources, such as array of
      chars
    * immutable, created string object cannot be changed, use String Buffer and String Builder
      classes if needed

    .. code-block:: java

       String s = "hello";
   
       // create using char array
       char[] charArray = { 'h', 'e', 'l', 'l', 'o'};
       String s = new String(charArray);


    * ``length()``: return length of string, ``s.length()``
    * ``concat()``: concatenate two strings and return new string, ``s1.concat(s2)`` or
      ``"hello".concat("world")``
    * ``format()``: create reusable formatted string

        .. code-block:: java

           String s;
           s = String.format("%.3f, %d, %s", floatVar, intVar, stringVar);


    * ``charAt()``: return char at index, ``s.charAt(8)``
    * ``compareTo()``: compare string to another object/string, return 0 if equal, < 0 if
      argument is greater, > 0 if argument is less than, `s1.compareTo(s2)`
    * ``compareToIgnoreCase()``: compare two strings, ignoring case, ``s1.compareToIgnoreCase(s2)``
    * ``contentEquals()``: return true if and only if this String represents same sequence of
      chars as specified in StringBuffer, `s.contentEquals(new StringBuffer())`
    * ``copyValueOf()``: return string as in the argument array, ``s1.copyValueOf(char[] s2)`` or
      ``s1.copyValueOf(char[] s2, startIndex, length)``
    * ``endsWith()``: check if string ends with specified suffix, ``s.endsWith("some string")``
    * ``equals()``: return true if equal, ``s1.equals(s2)``
    * ``equalsIgnoreCase()``: return true if equal ignoring case, ``s1.equalsIgnoreCase(s2)``
    * ``getBytes()``: encodes string into byte array, ``s.getBytes("UTF-8")`` or
      ``s.getBytes("ISO-8859-1")``
    * ``getChars()``: copy chars from string to char array, ``s.getChars(start, end, dst, dstBegin)``
    * ``hashCode()``: return hash code for string, ``s.hashCode()``
    * ``indexOf()``: return index of first occurrence of char or substring, -1 if not found,
      ``s.indexOf('a')`` or ``s.indexOf("abc", startIndex)``
    * ``intern()``: return canonical representation, ``s.intern()``, ``s.intern() == t.intern()`` if
      and only if `s.equals(t)`, interning ensure all strings having same contents share same
      memory
    * ``lastIndexOf()``: return index of last occurrence of char or substring, -1 if not found,
      ``s.lastIndexOf('a')`` or ``s.lastIndexOf("abc", startIndex)``
    * ``matches()``: return if string match regex or not, ``s.matches("*abc*")``, same as
      ``Pattern.matches(regex, str)``
    * ``regionMatches()``: check if two string regions are equal,
      ``s1.regionMatches(boolean ignoreCase, startIndex, s2, startIndexIns2, numOfCharToCompare)``
    * ``replace()``: return new string after replacing all occurrences of char,
      ``s.replace(old, new)``
    * ``replaceAll()``: return new string after replacing each substring that matches regex,
      ``s.replaceAll(regex, replaceWithThis)``
    * ``replaceFirst()``: return new string after replacing first substring that matches regex,
      ``s.replaceFirst(regex, replaceWithThis)``
    * ``split()``: return array of strings after splitting that matches regex, ``s.split(",")`` or
      ``s.split(",", limitToReturn)``
    * ``starsWith()``: check if string starts with specified prefix, ``s.starsWith("abc")`` or
      ``s.starsWith("abc", startIndex)``
    * ``subSequence()``: return new character sequence, ``s.subSequence(startIndex, endIndex)``
    * ``subString()``: return new substring, ``s.subString(start)`` or ``s.subString(start, end)``
    * ``toCharArray()``: return new char array, ``s.toCharArray()``
    * ``toLowerCase()``: convert all chars to lower, ``s.toLowerCase()``, which is same as
      ``s.toLowerCase(Locale.getDefault())``
    * ``toString()``: return itself a string, ``s.toString()``
    * ``toUpperCase()``: converts all chars to upper, ``s.toUpperCase()``, which is same as
      ``s.toUpperCase(Locale.getDefault())``
    * ``trim()``: return copy string after removing leading and trailing whitespace, ``s.trim()``
    * ``valueOf()``: return string representation of argument, ``String.valueOf(new long(123))``

Arrays
------
    * as arrays are reference types and can only be dynamically allocated, they are objects on
      the heap

    .. code-block:: java

       int[] myArray = new int[9]; // preferred way
       int[] myArray = {1, 2, 3}; // initialized
       int myArray[];


    * passing arrays to methods

        .. code-block:: java

           public void printArray(int[] myArray);


    * returning arrays from methods

        .. code-block:: java

           public int[] printArray(int[] myArray);


    * ``binarySearch()``: find number and return index of sorted array,
      ``Arrays.binarySearch(myArray, numToSearch)``
    * ``equals()``: true if two arrays have same number of elements, ``Arrays.equals(a1, a2)``
    * ``fill()``: set specified value to each element, ``Arrays.fill(myArray, 1)``
    * ``sort()``: sort the array in ascending order, ``Arrays.sort(myArray)``

Date/Time
---------
    * in ``java.util`` package, use [Java8 Date/Time](#java8-date-time) for updated API
    * ``Date()``: initialize object with current date and time
    * ``Date(long ms)``: accept argument of number of milliseconds since midnight Jan 1, 1970
    * ``after()``: true if this Date object is later than argument, ``d1.after(d2)``
    * ``before()``: true if this Date object is earlier than argument, ``d1.before(d2)``
    * ``clone()``: shallow copy of this Date object, ``Object d2 = d1.clone()``
    * ``compareTo()``: 0 if equal, < 0 if this Date is earlier, > 0 if this Date is later,
      ``d1.compareTo(d2)``
    * ``equals()``: true if same time and date, ``d1.equals(d2)``
    * ``getTime()``: return number of ms since Jan 1, 1970, ``d.getTime()``
    * ``hashCode()``: return hash code for this Date, ``d.hashCode()``
    * ``setTime()``: set time and date specified by in ms from Jan 1, 1970, ``d.setTime(long time)``
    * ``toString()``: convert and return this Date as string, ``d.toString()``
    * **SimpleDateFormat**
        - concrete class to format and parse dates
        - allow user-defined patterns for date-time formatting using date format codes

        .. code-block:: java

           Date d = new Date();
           SimpleDateFormat ft = new SimpleDateFormat("E yyyy.MM.dd 'at' hh:mm:ss a zzz");
           System.out.println(ft.format(d));
   
           // parse(), parse string according to the format stored in SimpleDateFormat object
           Date t = ft.parse(input);


    * can use ``printf`` to format date-time using date-time conversion characters

        .. code-block:: java

           String s = String.format("%tc", d);
           System.out.printf(s);
   
           // can use index to be formatted
           System.out.printf("%1$s %2$tB", "Date: ", d);
   
           // can use < flag
           System.out.printf("%s %tB %<te", "Date: " d);


    * ``Thread.sleep()``: sleep for any period, ``Thead.sleep(time in ms)``
    * ``System.currentTimeMillis()``: get current time in ms, used for measuring elapsed time
    * **GregorianCalendar**
        - concrete implementation of Calendar class in Gregorian
        - ``GregorianCalendar()``: initialize default GregorianCalendar using current time in
          default time zone and default locale or ``GregorianCalendar(int yr, int month, int date)``
          or ``GregorianCalendar(int yr, int month, int date, int hr, int min, int second)`` or
          ``GregorianCalendar(Locale l)`` or ``GregorianCalendar(TimeZone zone)`` or
          ``GregorianCalendar(TimeZone z, Locale l)``
        - ``add(int field, int amnt)``: add amount of time to given field,
          ``c.add(GregorianCalendar.MONTH, 2)``
        - ``computeFields()``: converts UTC as ms to time field values
        - ``computeTime()``: Overrides Calendar Converts time field values to UTC as ms
        - ``equals()``: true if equal, ``c1.equals(c2)``
        - ``get()``: get value of given time field, ``c.get(Calendar.YEAR)``
        - ``getActualMaximum()``: get max value a field can have,
          ``c.getActualMaximum(Calendar.YEAR)``
        - ``getActualMinimum()``: get minimum value a field can have,
          ``c.getActualMinimum(Calendar.YEAR)``
        - ``getGreatestMinimum()``: get highest minimum value of a field,
          ``c.getGreatestMinimum(Calendar.AM_PM)``
        - ``getGregorianChange()``: get date change from Julian Calendar to Gregorian
        - ``setGregorianChange()``: set Gregorian Calendar change date,
          ``c.setGregorianChange(new Date())``
        - ``getLeastMaximum()``: get lowest max value of a field,
          ``c.getLeastMaximum(Calendar.PM)``
        - ``getMaximum()``: get max value for a field, ``c.getMaximum(Calendar.YEAR)``
        - ``getTime()``: get this Calendar current time, ``c.getTime()``
        - ``getTimeInMillis()``: get this Calendar current time in ms, ``c.getTimeInMillis()``
        - ``getTimeZone()``: return TimeZone object, ``TimeZone t = c.getTimezone()``
        - ``hashCode()``: get hash code, ``c.hashCode()``
        - ``isLeapYear()``: true if argument is leap year, ``c.isLeapYear(2000)``
        - ``roll()``: add/subtract single unit of time on given field,
          ``c.roll(Calendar.YEAR, true)`` increase year by one, ``c.roll(Calendar.YEAR, false)``
          decrease year by one
        - ``set()``: set time field with given value, ``c.set(Calendar.YEAR, 22)`` or
          ``c.set(yr, month, date)`` or ``c.set(yr, month, date, hr, min)`` or
          ``c.set(yr, month, date, hr, min, second)``
        - ``setTime()``: set this Calendar current time with given Date object, ``c.setTime(Date())``
        - ``setTimeinMillis()``: set this Calendar current time in given long value
        - ``setTimeZone()``: set time zone with given TimeZone object, ``c.setTimeZone(TimeZone)``
        - ``toString()``: return string representation of this Calendar

Regex
-----
    * in ``java.util.regex`` package
    * **Pattern Class**
        - compiled representation of regex, no public constructors
        - must invoke ``compile()``, which returns Pattern object
    * **Matcher Class**
        - interpret pattern and preform match operations on input string
        - no public constructors, must invoke ``matcher()`` on Pattern object
        - ``groupCount()``: return number of capturing groups, does not include group 0,
          ``m.groupCount()``
        - capturing group 0 represents entire expression
        - ``start()``: return start index of previous match, ``m.start()`` or ``m.start(int group)``,
          which returns start index of subsequence captured by given group
        - ``end()``: return offset after the last char matched, ``m.end()`` or ``m.end(int group)``,
          which returns offset after the last char of subsequence captured by given group
        - ``lookingAt()``: true if pattern is matched, starting at the beginning, ``m.lookingAt()``
        - ``find()``: true if next subsequence of matched pattern is found, ``m.find()`` or
          ``m.find(int start)``, which find next subsequence at given index
        - ``matches()``: true if entire region matches the pattern
        - ``appendReplacement(StringBuffer, String)``: non-terminal append and replace return
          Matcher object, ``m.appendReplacement()``
        - ``appendTail(StringBuffer)``: terminal append and replace, return StringBuffer object
        - ``replaceAll(String)``: replace every subsequence that matches with given string,
          return String object
        - ``replaceFirst(String)``: replace first subsequence, ``m.replaceFirst()``, return String
        - ``quoteReplacement(String)``: return literal replacement String for specified String,
          act as intermediate in replace methods, ``m.quoteReplacement()``
    * **PatternSyntaxException**
        - unchecked exception that indicates syntax error in regex
        - ``getDescription()``: return description of error
        - ``getIndex()``: return error index
        - ``getPattern()``: return incorrect regex pattern
        - ``getMessage()``: return description of syntax error and index, incorrect regex
          pattern and visual indication of error index within pattern

    .. code-block:: java

       String line = "hello abc hello";
       String pattern = "(.*)(abc)(.*)";
       Pattern p = Pattern.compile(pattern);
       Matcher m = p.matcher(line);
   
       if (m.find()) {
           // pattern found
       }
       else {
           // pattern not found
       }



Methods
-------
    * modifier: define access type of the method, optional
    * ``void``: method does not return any value

    .. code-block:: java

       // modifier returnType methodName (ParameterList)
       public static int myMethod (int a) {}


    * **pass by value**

        .. code-block:: java

           public static void swap(int a, int b) {
               int tmp = a;
               a = b;
               b= tmp;
           }
   
           public static void main(String[] args) {
               int x = 1, y = 2;
               swap(x, y); // calling swap does not change x and y values
           }


    * **method overloading**
        - a class having two or more methods with same name but different parameters
        - not same as overriding, which has same name, type, number of parameters, etc.
        - overloading make program more readable

        .. code-block:: java

           public static int myFunc(int a) {}
           public static double myFunc(double a) {}


    * **command-line arguments**
        - stored as strings in String array passed to ``main()``

        .. code-block:: java

           public static void main(String[] args) {
               System.out.println(args.length);
           }


    * **this**
        - used as reference to the object of current class, only within instance method or
          constructor
        - can refer the members of class such as constructors, variables and methods
        - to differentiate instance variables from local variables that have same names

        .. code-block:: java

           class MyClass {
               int x;
   
           /* explicit constructor invocation: calling one type of constructor, such as
           parameterized constructor or default from other in a class */
               MyClass() {
                   this(2); // invoke MyClass(int x)
               }
   
               MyClass(int x) {
                   this.x = x;
               }
           }


    * **var-args**
        - JDK 1.5 enables to pass variable number of same type arguments to a method

        .. code-block:: java

           public static void MyFunc(int... numbers) {
               System.out.println(numbers.length);
           }
   
           // both valid
           MyFunc(1, 2, 3, 4);
           MyFunc(new int[] {1, 2, 3, 4});



I/O
---
    * in ``java.io`` package
    * InputStream: read data from source
    * OutputStream: write data to destination
    * classes of streams: File, ByteArray, Filter (Buffered, Data), Object
    * **byte streams**
        - for I/O of 8-bit bytes

        .. code-block:: java

           FileInputStream in = null;
           FileOutputStream out = null;
   
           try {
               in = new FileInputStream("input.txt");
               out = new FileOutputStream("output.txt");
   
               int c;
               while ((c = in.read()) != -1) {
                   out.write(c);
               }
           } finally {
               if (in != null) {
                   in.close();
               }
               if (out != null) {
                   out.close();
               }
           }


    * **character streams**
        - for I/O of 16-bit unicode
        - ``FileReader`` and ``FileWriter`` use ``FileInputStream`` and ``FileOutputStream``
          internally, but read and write 2 bytes at a time

        .. code-block:: java

           FileReader in = null;
           FileWriter out = null;


    * **standard streams**
        - standard input: input from user, ``System.in``
        - standard ouput: output from program to user, ``System.out``
        - standard error: output error from program to user, ``System.err``

        .. code-block:: java

           InputStreamReader cin = null;
   
           try {
               cin = new InputStreamReader(System.in);
               System.out.println("Enter input, 'q' to quit.");
               char c;
               do {
                   c = (char) cin.read();
                   System.out.print(c);
               } while (c != 'q');
           } finally {
               if (cin != null)
                   cin.close();
           }


    * **FileInputStream**
        - for reading data from files
        - objects can be created, and several types of constructors available
        - all methods throw IOException
        - ``close()``: close file input stream, ``in.close()``
        - ``finalize()``: protected method, clean the connection to the file, ensure ``close()``
          is called when there are no more references to the stream
        - ``read(int r)``: read specified byte of data from InputStream, return the next byte of
          data or -1 if end of file
        - ``read(byte[] r)``: read r.length bytes from InputStream into array, return total
          number of bytes read or -1 if end of file
        - ``available()``: return number of bytes that can be read from the file input stream

        .. code-block:: java

           InputStream in = new FileInputStream("filename");
   
           // using File object
           File f = new File("filename");
           InputStream in = new FileInputStream(f);


    * **FileOutputStream**
        - to create file and write data into it
        - will create new file if not exist, before opening it for output
        - all methods throw IOException
        - ``close()``: file file output stream, ``out.close()``
        - ``finalize()``: protected method, clean the connection to the file, ensure ``close()``
          is called when there are no more references to the stream
        - ``write(int w)``: write specified byte to output stream
        - ``write(byte[] w)``: write w.length bytes from byte array to OutputStream

        .. code-block:: java

           OutputStream out = new FileOutputStream("filename")
   
           // using file object
           File f = new File("filename")
           OutputStream out = new FileOutputStream(f);


    * **Directories**
        - can use File object to create directories and list files in a directory
        - ``mkdir()``: create a directory, return true on success and false on failure, which
          means path specified already exists or entire path does not exist yet, ``d.mkdir(/foo)``
        - ``mkdirs()``: create both directory and parents of the directory, ``d.mkdirs(/foo/bar)``
        - path separators of UNIX and Windows are resolved correctly by Java
        - ``list()``: list all files

        .. code-block:: java

           File d = new File("/foo/bar");
           d.mkdirs();
   
           String[] paths = d.list();
           for (String p : paths) {
               System.out.println(p);
           }


    * **ByteArrayInputStream**
        - allow buffer in memory to be used as InputStream, byte array as input source
        - ``ByteArrayInputStream(byte[] a)`` or ``ByteArrayInputStream(byte[] a, int off, int len)``:
          constructor take byte array, first byte to be read and number of bytes to be read
        - ``read()``: read next byte from InputStream, return int as next byte of data, -1 if
          end of file
        - ``read(byte[] r, int off, int len)``: read from input stream starting from off till
          len into an array, return total number of bytes read or -1 if end of file
        - ``available()``: return number of readable bytes from input stream
        - ``mark(int r)``: set current marked position in the stream, max limit of readable
          bytes as argument
        - ``skip(long n)``: skip n numbers of bytes from stream, return actual number of bytes
          skipped

        .. code-block:: java

           ByteArrayInputStream bInput = new ByteArrayInputStream(byte[] b);
           for (int i = 0; i < 1; ++i) {
               while ((c = bInput.read()) != -1)
                   System.out.println(Character.toUpperCase((char) c));
   
               bInput.reset();
           }


    * **ByteArrayOutputStream**
        - create buffer in memory, all data sent to the stream is stored in the buffer
        - ``ByteArrayOutputStream()``: create ByteArrayOutputStream having buffer of 32 bytes
        - ``ByteArrayOutputStream(int s)``: having buffer of given size
        - ``reset()``: reset number of valid bytes of the stream to zero, all output in the
          stream is discarded
        - ``toByteArray()``: return newly allocated byte array, with size and content of current
          output stream
        - ``toString()``: return buffer content as string
        - ``write(byte[] b)``: write given array to output stream
        - ``write(byte[] b, int off, int lent)``: write len of bytes starting from off
        - ``writeTo(OutputStream o)``: write entire content of this Stream to given stream

        .. code-block:: java

           ByteArrayOutputStream bOutput = new ByteArrayOutputStream(12);
   
           while (bOutput.size() != 10)
               bOutput.write("hello".getBytes());
   
           byte[] b = bOutput.toByteArray();


    * **DataInputStream**
        - to read primitives
        - ``DataInputStream(InputStream in)``: create InputStream object
        - all methods throw IOException
        - ``read(byte[] b)``: read bytes from input stream into the byte array, return total
          number of bytes read or -1 if end of file
        - ``read(byte[] b, int off, int len)``: read len of bytes starting from off
        - ``readBoolean()``, ``readByte()``, ``readShort()``, ``readInt()``: read bytes from the
          contained InputStream, return next two bytes of InputStream as specific primitive
          type
        - ``readLine()``: read next line of text from InputStream, read successive bytes by
          converting each into char, until line terminator or end of file, return chars read
          as String

        .. code-block:: java

           DataInputStream dataIn = new DataInputStream(new FileInputStream("filename"));
   
           while (dataIn.available() > 0) {
               System.out.print((char) dataIn.read());
           }


    * **DataOutputStream**
        - write primitives to output source
        - ``DataOutputStream(OutputStream out)``: create OutputStream object
        - all methods throw IOException
        - ``write(byte[] w)``: write number of bytes to output stream, return number bytes
          written to buffer
        - ``write(byte[] w, int off, int len)``: write len bytes from byte array at starting
          point off
        - ``writeBoolean()``, ``writeByte()``, ``writeShort()``, ``writeInt()``: write specific
          primitive type data into output stream as bytes
        - ``flush()``: flush data output stream
        - ``wrtieBytes(String s)``: write the string to output stream as sequence of bytes, by
          discarding each char high eight bits

        .. code-block:: java

           DataOutputStream dataOut = new DataOutputStream(new FileOutputStream("filename"));
           dataOut.writeUTF("hello");


    * **File**
        - class to create files and directories, file searching, file deletion, etc.
        - File object represents actual file/dir on the disk
        - ``File(File parent, String child)``: create File instance from parent abstract
          pathname and child pathname
        - ``File(String pathname)``: create File instance by converting given pathname into
          abstract pathname
        - ``File(String parent, String child)``: create File instance from parent and child
          pathname string
        - ``File(URI uri)``: create File instance by converting given URI into abstract pathname
        - ``getName()``: return name of file or dir
        - ``getParent()``: return pathname's parent or null if parent dir not exist
        - ``getParentFile()``: return abstract pathname of pathname's parent, null if parent
          dir does not exist
        - ``getPath()``: return pathname string
        - ``isAbsolute()``: true if pathname is absolute
        - ``getAbsolutePath()``: return absolute pathname string
        - ``canRead()``: true if and only if file exists and can be read
        - ``canWrite()``:true if and only if file exists and is allowed to write
        - ``exists()``: true if file or dir exists
        - ``isDirectory()``: true if and only if pathname exists and is a dir
        - ``isFile()``: true if and only if file exists and is normal file, which is not a dir
          and satisfy other system-dependent criteria
        - ``lastModified()``: return last modified time in ms since epoch (Jan 1, 1970)
        - ``length()``: return length of file, return unspecified value if pathname is dir
        - ``createNewFile()``: auto create new, empty file only if it does not exist, return
          true if file not exist and created successfully, throw IOException
        - ``delete()``: delete file or dir, dir must be empty, return true if success
        - ``deleteOnExit()``: request file or dir be deleted when the VM terminates
        - ``list()``: return array of strings of files and dirs
        - ``list(FilenameFilter f)``: return array of strings of files and dirs that satisfy
          given filter
        - ``listFiles()`` or ``listFiles(FileFilter)``: return array of File objects
        - ``mkdir()``: create dir, return true if dir is created
        - ``mkdirs()``: create dir, with necessary parent dirs if not exist, return true if
          dir with parent dirs is created
        - ``renameTo(File f)``: rename the file, return true if success
        - ``setLastModified(long time)``: set last modified time of file or dir, return true if
          success
        - ``setReadOnly()``: mark file or dir read only, return true if success
        - ``createTempFile(String prefix, String suffix)`` or
          ``createTempFile(String prefix, String suffix, File dir)``: create empty file in default
          tmp or specified directory, using prefix and suffix to generate name, return
          abstract >pathname of created empty file
        - ``compareTo(File)``: compare two abstract pathnames lexicographically, return 0 if
          equal, < 0 if argument is greater, > 0 if argument is less
        - ``compareTo(Object)``: compare abstract pathname to another object, return 0 if equal,
          < 0 if argument is greater, > 0 if argument is less
        - ``equals(Object)``: true if argument is not null and same file or dir
        - ``toString()``: return pathname string, which is just the string returned by ``getPath()``

        .. code-block:: java

           File f = null;
           f = new File("filename");
           boolean bool = f.canExecute();
           String s = f.getAbsolutePath();


    * **FileReader**
        - inherits from InputStreamReader, used to read streams of characters
        - ``FileReader(File)``, ``FileReader(FileDescriptor)``, ``FileReader(String)``: create
          FileReader with given argument to read from
        - ``read()``: read single char, return int of char read, throws IOException
        - ``read(char[] c, int off, int len)``: read chars into array, return number of chars
          read

        .. code-block:: java

           FileReader fr = new FileReader(new File("filename"));
           char[] a = new char[50];
           fr.read(a); // read content to the array
           fr.close();


    * **FileWriter**
        - inherits form OutputStreamWriter, used to write streams of characters
        - ``FileWriter(File)``, ``FileWriter(File,boolean append)``, ``FileWriter(FileDescriptor)``,
          ``FileWriter(String)``, ``FileWriter(String, boolean append)``: create FileWriter object,
          accept boolean to append data or not
        - ``write(int c)``: write single char, throw IOException
        - ``write(char[] c, int off, int len)``: write len of array of chars starting from off
        - ``write(String s, int off, int len)``: write len of String starting from off

        .. code-block:: java

           FileWriter writer = new FileWriter(new File("filename"));
           writer.write("hello");
           writer.flush();
           writer.close();



Exceptions
----------
    * in ``java.lang.Exception`` package
    * exceptions should be handled not to let programs terminate abnormally
    * exceptions can be caused by users, programmer or other resources errors
    * **checked exceptions**
        - checked by the compiler at compilation time, aka compile time exceptions
        - cannot be ignored and must be taken care of immediately
        - e.g ``FileNotFoundException`` when creating ``FileReader`` object and the file doesn't
          exist
        - ``ClassNotFoundException``, ``CloneNOtSupportedException``, ``IllegalAccessException``
        - ``InstantiationException``, ``InterruptedException``, ``NoSuchFieldException``
        - ``NoSuchMethodException``
    * **unchecked exceptions**
        - occurs at the time of program execution, aka runtime exceptions
        - ignored at compile time, such as bugs and logic errors
        - e.g ``ArrayIndexOutOfBoundsException`` only shows up when running the program
        - ``ArithmeticException``, ``ArrayIndexOutOfBoundsException``, ``ArrayStoreException``
        - ``ClassCastException``, ``IllegalArgumentException``, ``IllegalMonitorStateException``
        - ``IllegalStateException``, ``IllegalThreadStateException``, ``IndexOutOfBoundsException``
        - ``NegativeArraySizeException``, ``NullPointerException``, ``NumberFormatException``
        - ``SecurityException``, ``StringIndexOutOfBounds``, ``UnsupportedOperationException``
    * **errors**
        - not exceptions, but beyond control of user or programmer
        - ignored at compile time, generated by runtime environment
        - e.g a stack overflow, JVM out of memory
    * JVM exceptions
        - thrown by the JVM
        - e.g ``NullPointerException``, ``ArrayIndexOutOfBoundsException``, ``ClassCastException``
    * programmatic exceptions
        - thrown by the application or API
        - e.g ``IllegalArgumentException``, ``IllegalStateException``
    * ``Exception`` and ``Error`` classes are subclasses of ``Throwable`` class
    * ``IOException`` and ``RuntimeException`` are two main subclasses of ``Exception`` class
    * **Throwable**
        - ``getMessage()``: return detail message about exception
        - ``getCause()``: return cause of exception as Throwable object
        - ``toString()``: return name of the class from ``getMessage()``
        - ``printStackTrace()``: print result of ``toString()`` with stack trace to ``System.err``
        - ``getStackTrace()``: return array with elements on stack trace, index 0 being the top
          of the call stack
        - ``fillInStackTrace()``: fill the stack trace of this Throwable object with current
          stack trace, return Throwable object
    * **try/catch**
        - placed around the code that might generate exception
        - code within the block is called protected code
        - every ``try`` block should be immediately followed by ``catch`` or ``finally``, which is
          optional
        - one ``try`` can have multiple ``catch`` blocks
        - code in ``finally`` block always execute, even if exception does not occur
        - can use ``finally`` to do cleanup, no matter what happens in the protected code, e.g
          closing a file

        .. code-block:: java

           try {
               // protected code
           } catch (Exception e) {
               System.out.println(e);
           } catch (ExceptionType1 | ExceptionType2 e) {
               // can handle multiple exception in single catch block since Java 7
               System.out.println(e);
           } finally {
               // at the end of catch blocks
               // always execute
           }


    * **throws/throw**
        - used when a method does no handle a checked exception
        - ``throws``: used to postpone the handling of checked exception
        - ``throw``: used to invoked exception explicitly

        .. code-block:: java

           // can declare to throw more than one exception
           public void MyFunc() throws Exception1, Exception2 {
               throw new Exception3();
           }


    * **try-with-resources**
        - automatic resource management, introduced in Java 7
        - auto close the resources used within try/catch block
        - a class should implement ``AutoCloseable`` interface to be used with
        - can have multiple classes in ``try`` statement, which will be closed in reverse order
        - resources declared in ``try`` statement are instantiated before the start of ``try`` block,
          and are implicitly declared as ``final``

        .. code-block:: java

           try (FileReader fr = new FileReader("filename")) {
               // no need to invoke fr.close();
           } catch (IOException e) {
               e.printStackTrace();
           }


    * **custom exceptions**
        - must be a child of ``Throwable``
        - need to extend ``Exception`` class for a checked exception that is auto enforced by
          the Handle or Declare Rule
        - need to extend ``RuntimeException`` for runtime exception

        .. code-block:: java

           public class MyException extends Exception {}



Interfaces
----------
    * contract between objects on how to communicate with each other, a reference type
    * define methods a subclass should use, but implementation is up to the subclass
    * can contain constants, default and static methods and nested types
    * only default and static methods can have method body
    * all methods need to be defined in the class unless the class is abstract
    * cannot be instantiated, no constructors, all methods are abstract
    * cannot contain instance fields, except declared ``static`` and ``final``
    * interface is implicitly abstract, methods in it are implicitly abstract and public, so
      the keywords can be omitted
    * cannot declare checked exceptions other than the ones declared by the interface
    * must maintain signature of interface method and same return type or subtype

    .. code-block:: java

       interface MyInterface {
           void foo();
       }
   
       interface OtherInterface extends MyInterface {
           // inherits 'foo()'
           void bar();
       }
   
       class A implements OtherInterface {
           // 'A' must implement both 'foo()' and 'bar()'
   
           // must be public
           public void foo() {}
           public void bar() {}
       }
   
       // no need to implement 'foo()'
       abstract class B implements MyInterface {}


    * a class can extend only one class but implement more than one interface

        .. code-block:: java

           interface MyInterface {
               void foo();
           }
   
           interface OtherInterface {
               void bar();
           }
   
           class A implements MyInterface, OtherInterface {
               public void foo() {}
               public void bar() {}
           }


    * an interface can extend multiple interfaces

        .. code-block:: java

           interface MyInterface {
               void foo();
           }
   
           interface OtherInterface {
               void bar();
           }
   
           interface MultipleInterface extends MyInterface, OtherInterface {}


    * **tagging interface**
        - interface with no methods in it
        - to create common parent among group of interfaces
        - to add data type to a class: implementing class does not need to define any methods,
          but becomes an interface type through polymorphism

Packages
--------
    * categorizing the classes, interfaces, enumerations and annotation types for easy search
      and usage
    * to prevent naming conflicts and to control access
    * a grouping of related types for access protection and namespace management
    * e.g ``java.lang``, ``java.io``
    * package statement should be the first line in source file
    * only one package statement per source file
    * if no package statement is used, will be placed in current default package
    * use ``javac -d DEST file.java`` to compile programs with package statements

    .. code-block:: java

       // MyPackage.java
       package MyPackage;
       interface MyInterface {
           void foo();
       }
   
       // MyClass.java
       package MyPackage;
       public class MyClass implements MyInterface {
           public void foo() {}
   
           public static void main(String[] args) {}
       }


    * classes in the same package find each other without any special syntax
    * if different packages, use ``import`` or ``packageName.Class``

        .. code-block:: java

           // A.java
           package MyPackage;
           public class A {}
   
           // B.java
           package OtherPackage;
           public class B {}
   
           // MyClass.java
           package MyPackage;
           import OtherPackage.*;
           public class MyClass implements MyInterface {
               public static void main(String[] args) {
                   A a = new A(); // can use 'A' class since same package
                   B b = new B(); // ok, since import statement is used
   
                   // or
                   OtherPackage.B b = new B(); // if import statement is not used
               }
           }


    * the name of the package must match the directory structure of bytecode
    * when compiled there will be separate files for each class, interface and enumerations

        .. code-block:: java

           // A.java
           package foo.bar.MyPackage;
           public class A {}
           class B {}
   
           // './foo/bar/MyPackage/A.class'
           // './foo/bar/MyPackage/B.class'


    * compiled ``.class`` files and ``.java`` source files do not need to have same path
    * can give access to others without revealing source files
    * **CLASSPATH**
        - full path of the classes directory can be set with ``CLASSPATH`` system variable
        - if class path is ``path/classes`` and package is ``foo.MyPackage``, compiler and JVM will
          look for ``.class`` files in ``path/classes/foo/MyPackage``
        - can have several class paths separated by semicolon (Windows) or colon (Unix)
        - by default, compiler and JVM will search current directory and JAR files containing
          Java platform classes

`back to top <#java>`_

Classes
=======

* `Class Variables`_, `Constructors`_, `Singleton Class`_, `Class Rules`_, `Import`_
* `Abstract Class`_, `Non-static Nested Class`_, `Static Nested Class`_
* blueprint to create objects
* use ``new`` keyword to create new objects
* a class cannot be associated with ``private`` access modifier
* variables of a class can have another class as its member
* nested class: class written within a class, Non-static nested class and Static nested class
* outer class: the class that holds the inner class


.. code-block:: java

   public class MyClass {
       int x;
   
       public int getX() {
           return x;
       }
   
       public static void main (String []args) {
           MyClass c1 = new MyClass();
           System.out.println(c1.x); // accessing instance variable
       }
   }



Class Variables
---------------
    * local: defined inside methods, constructors or blocks
    * instance: within a class, outside any method, can be accessed from any method
    * class: declared within a class, outside any method, with static keyword

    .. code-block:: java

       public static void main (String []args) {
           MyClass c1 = new MyClass();
           System.out.println(c1.x); // accessing instance variable
       }


Constructors
------------
    * at least one constructor is invoked each time a new object is created
    * should have the same name as the class
    * a class can have more than one constructor
    * no explicit return type
    * compiler builds a default one if not defined explicitly, initializing member variables
      to zero

    .. code-block:: java

       public class MyClass {
           public MyClass () {} // constructor
   
           public MyClass (int y) {} // constructor with one parameter
       }



Singleton Class
---------------
    * can only create one instance of the class

Class Rules
-----------
    * only one public class per source file, but can have multiple non-public classes
    * public class name should be the same as file name
    * package statement should be the first in the source file if the class is declared inside
      a package
    * import statements must be written between package statements and class declaration
    * import and package statements will imply to all classes present in the source file
    * cannot declare different import or package statements to different classes in the file

Import
------
    * to give the compiler the location to find a particular class

    .. code-block:: java

       import java.io.*; // load all classes in directory java_installation/java/io



Abstract Class
--------------
    * contains ``abstract`` keyword in declaration
    * may or may not have abstract methods, methods without body
    * class must be abstract if it has at least one abstract method
    * abstract classes cannot be instantiated
    * must inherit from another class with implementations of abstract methods to use abstract
      class
    * all abstract methods must be implemented once inherited

    .. code-block:: java

       abstract class A {
           public abstract void hello();
       }
       class B extends A {
           public void hello() { } // 'B' must implement 'hello()' or itself must be abstract
       }
   
       A a = new A(); // error
       A a = new B(); // ok



Non-static Nested Class
-----------------------
    * also called inner class, used for security mechanism
    * inner class can be made ``private`` and used to access private members of a class
    * if ``private``, it cannot be accessed from object outside the class

    .. code-block:: java

       class Outer {
           private class Inner {
               public void innerHello() {
                   System.out.println("Hello from Inner Class");
               }
           }
   
           void callInner() {
               Inner i = new Inner();
               i.innerHello();
           }
       }
   
       public class MyClass {
           public static void main(String[] args) {
               Outer o = new Outer();
               o.callInner();
           }
       }


    * can use inner class methods to access private members of a class

        .. code-block:: java

           class Outer {
               private int outerNum = 9;
   
               public class Inner {
                   public int getOuterNum() {
                       return outerNum;
                   }
               }
           }
   
           public class MyClass {
               public static void main(String[] args) {
                   Outer o = new Outer();
                   Outer.Inner i = o.new Inner();
                   System.out.println(i.getOuterNum());
               }
           }


    * **Method-local Inner Class**
        - class within a method, scope of the class is restricted within the method
        - can only be instantiated within the method, where it is defined

        .. code-block:: java

           public class MyClass {
               void myFunc() {
                   class MethodInner {
                       public void helloFromMethodInner() {
                           System.out.println("Hello from method inner class");
                       }
                   }
   
                   MethodInner mi = new MethodInner();
                   mi.helloFromMethodInner();
               }
   
               public static void main(String[] args) {
                   MyClass m = new MyClass();
                   m.myFunc();
               }
           }

    * **Anonymous Inner Class**
        - inner class without class name, declare and instantiate at the same time
        - used to override the method of a class or interface

        .. code-block:: java

           abstract class AnonymousInner {
               public abstract void anonymousInnerMethod();
           }
   
           public class MyClass {
   
               public static void main(String[] args) {
                   AnonymousInner i = new AnonymousInner() {
                       public void anonymousInnerMethod() {
                           System.out.println("Hello from AnonymousInner");
                       }
                   };
   
                   i.anonymousInnerMethod();
               }
           }


        - can pass anonymous inner class as argument to a method that accepts object of an
          interface, abstract class or a concrete class

        .. code-block:: java

           interface MyInterface {
               void foo();
           }
   
           public class MyClass {
               public void bar(MyInterface i) {
                   i.foo();
               }
   
               public static void main(String[] args) {
                   MyClass m = new MyClass();
   
                   m.bar(new MyInterface() {
                       public void foo() {
                           System.out.println("hello from foo");
                       }
                   });
               }
           }



Static Nested Class
-------------------
    * static member of outer class
    * can be accessed without instantiating the outer class
    * does not have access to instance variables and methods of outer class
    * can access static members of outer class, but must use outer object to access non-static
      members

    .. code-block:: java

       public class Outer {
           int outer = 1;
           static staticOuter = 2;
   
           static class StaticNested {
               public void foo(Outer o) {
                   System.out.println("hello from static inner");
                   // can access static member of outer class normally
                   System.out.println(staticOuter);
   
                   // need outer object to access non-static members
                   System.out.println(o.outer);
               }
           }
   
           public static void main(String[] args) {
               Outer o = new Outer();
               StaticNested i = new StaticNested();
               // can also instantiate with 'Outer.StaticNested i = new Outer.StaticNested()';
               i.foo(o);
           }
       }


`back to top <#java>`_

Object Oriented Programming
===========================

* `Inheritance`_, `Polymorphism`_, `Abstraction`_, `Encapsulation`_
* four fundamentals of OOP: abstraction, encapsulation, inheritance and polymorphism


Inheritance
-----------
    * one class acquiring properties of another
    * subclass: class that inherits properties of other, aka derived class, child class
    * superclass: class whose properties are inherited, aka base class, parent class
    * ``extends``: to inherit properties of a class, except private ones

    .. code-block:: java

       class Super {
           int superX = 9;
           public void superMethod() {}
       }
   
       // Sub inherits all of Super's methods and fields
       class Sub extends Super {
           public void subMethod() {}
       }
   
       Sub s = new Sub();
       s.superMethod();
       s.superX;
       s.subMethod();


    * when an object of subclass is created, a copy of contents of superclass is made within it
    * can instantiate using superclass reference variable, but can't access subclass properties

        .. code-block:: java

           Super s = new Sub();
           s.superMethod(); // ok
           s.superX; // ok
           s.subMethod(); // error


    * subclass inherits all members, such as fields, methods and nested class from its
      superclass, but not constructors, as they are not members
    * ``super``: to invoke constructor of superclass, similar to ``this`` keyword, use to
      differentiate members of superclass from members of subclass if same names

        .. code-block:: java

           class Super {
               int i = 9;
               public void hello() {
                   System.out.println("hello from super");
               }
           }
   
           class Sub extends Super {
               int i = 8;
               public void hello() {
                   System.out.println("hello from sub");
               }
   
               public void subMethod() {
                   Sub s = new Sub();
                   s.hello(); // print "hello from sub"
                   super.hello(); // print "hello from super"
                   System.out.println("i from sub: " + s.i); // print 8
                   System.out.println("i from super: " + super.i); // print 9
               }
           }
   
           // can use to call parameterized constructor of superclass
           class Super {
               int i;
   
               Super(int i) {
                   this.i = i;
               }
           }
   
           class Sub extends Super {
               Sub(int i) {
                   super(i); // can now call Super parameterized constructor
               }
           }
   
           Sub s = new Sub(8); // ok


    * ``instanceof``: to check an object is is an instance of specified type

        .. code-block:: java

           class A {}
           class B extends A {}
           class C extends B {}
   
           B b = new B();
           C c = new c();
           System.out.println(b instanceof A); // true
           System.out.println(c instanceof B); // true
           System.out.println(c instanceof A); // true


    * ``implements``: to inherit properties of an interface, interface can't be extended by class

        .. code-block:: java

           interface A {}
           class B implements A {}
           class C extends B {}
   
           System.out.println(b instanceof A); // true
           System.out.println(c instanceof B); // true
           System.out.println(c instanceof A); // true


    * single inheritance: ``B extends A``
    * multi level inheritance: ``B extends A``, ``C extends B``
    * hierarchical inheritance: ``B extends A``, ``C extends A``
    * multiple inheritance: ``C extends A, B`` (not supported by Java, as it can lead to diamond
      problem)
    * a class can implement more than one interfaces: ``C implements A, B``
    * **Overriding**
        - overriding function of existing method
        - allow subclass to implement parent class method based on its requirements
        - argument list must be same, return type must be same or subtype of the one declared
          in original overridden method
        - access level cannot be more restrictive than original, e.g if original method is
          ``public``, overriding method cannot be ``private`` or ``protected``
        - can only override instance methods if subclass inherits them
        - method with ``final`` cannot be overridden
        - method with ``static`` cannot be overridden, but can be re-declared
        - cannot override if method cannot be inherited
        - if same package as superclass, can override any method, unless ``private`` or ``final``
        - if different package as superclass, can only override ``public`` or ``protected``
          non-final methods
        - overriding method can throw unchecked exceptions, even if the original method does
          not
        - overriding method should not throw checked exceptions that are new or broader than
          the ones in original method
        - constructors cannot be overridden

        .. code-block:: java

           class A {
               public void hello() {
                   System.out.println("hello from A");
               }
           }
   
           class B extends A {
               public void hello() {
                   System.out.println("hello from B");
               }
   
               public void onlyB() {
                   System.out.println("B specific method");
               }
           }
   
           A a = new A();
           B b = new B();
           a.hello(); // A's hello()
           b.hello(); // B's hello()
           b.onlyB(); // ok
   
           // A as b's reference type does not have methods in B
           A b = new B();
           b.hello(); // B's hello()
           b.onlyB(); // error



Polymorphism
------------
    * object having many forms, occur when parent class reference is used to refer child class
      object
    * all objects are polymorphic in Java as any object will pass ``instanceof`` test for their
      own type and class Object
    * reference variable can be of only one type and cannot be changed once declared
    * reference variable can be reassigned to other objects if it not declared ``final``
    * reference variable can be declared as class or interface

    .. code-block:: java

       public interface A {}
       public class B {}
       public class C extends B implements A {} // C is polymorphic
   
       C c = new C();
       c instanceof A // true
       c instanceof B // true
       c instanceof C // true
       c instanceof Object // true
   
       A a = c; // ok
       B b = c; // ok
       Object o = c; // ok


    * **Virtual Methods**: overridden methods being invoked at run time, no matter what
      reference type is used at compile time

        .. code-block:: java

           class B {
               public void hello() {
                   System.out.println("hello from B");
               }
           }
   
           class C extends B {
               public void hello() {
                   System.out.println("hello from C");
               }
           }
   
           C c = new C();
           // 'C' object instantiated using 'B' reference 'b'
           B b = new C();
           c.hello(); // print "hello from C"
           // compiler use 'hello()' from 'B', but JVM invoke 'hello()' from 'C' during run time
           b.hello(); // print "hello from C"



Abstraction
-----------
    * hiding implementation from the user, except functionality
    * user will have information about what the object does, not how it does it
    * achieved using [Abstract Class](#abstract-class) and [Interface](#interfaces)

Encapsulation
-------------
    * wrapping variables and methods as single unit, aka data hiding
    * variables are declared ``private``, define getter and setter methods to access variables
    * by encapsulating, can set fields of a class read/write only and a class can have total
      control over what is stored in its fields

    .. code-block:: java

       class A {
           private int num = 1;
   
           public int getNum() {
               return num;
           }
   
           public void setNum(int n) {
               num = n;
           }
       }
   
       a.getNum(); // 1
       a.setNum(9);
       a.getNum(); // 9


`back to top <#java>`_

Collections Framework
=====================

* `Collection`_, `List`_, `LinkedList`_, `ArrayList`_
* `Set`_, `Sorted Set`_, `HashSet`_, `LinkedHashSet`_, `TreeSet`_
* `Map`_, `Map.Entry`_, `SortedMap`_, `HashMap`_, `TreeMap`_, `WeakHashMap`_, `LinkedHashMap`_, `IdentityHashMap`_
* `Enumeration`_, `Algorithms`_, `Iterator`_, `Comparator`_
* in ``java.util`` package
* unified architecture for representing and manipulating collections
* contain interfaces, implementations/classes and algorithms


Collection
----------
    * ``AbstractCollection``: implement most of the Collection interface
    * foundation interface on which collections framework is built
    * all collections implement Collection interface
    * several methods can throw ``UnsupportedOperationException``
    * ``add(Object)``: adds argument to this collection, return true if added, false if object is
      already member or if the collection doesn't allow duplicates
    * ``addAll(Collection)``: add all elements of argument to this collection, true if added
    * ``clear()``: remove all elements from this collection
    * ``contains(Object)``: true if object is element of this collection
    * ``containsAll(Collection)``: true if this collection contain all elements of argument
    * ``equals(Object)``: true if this collection and argument are equal
    * ``hashCode()``: return hash code for this collection
    * ``isEmpty()``: true if this collection is empty
    * ``iterator()``: return iterator for this collection
    * ``remove(Object)``: remove one instance of object from this collection, true if removed
    * ``removeAll(Collection)``: remove all elements of argument from this collection, return
      true if removed
    * ``retainAll(Collection)``: remove all elements from this collection except those in
      argument, true if removed
    * ``size()``: return number of elements of this collection
    * ``toArray()``: return array of all elements of this collection, elements are copies
    * ``toArray(Object[])``: return array with elements whose type match argument

List
----
    * ``AbstractList``: extend AbstractCollection and implement most of the List interface
    * ``AbstractSequentialList``: extend AbstractList for collection of sequential access
    * stores sequence of elements
    * can insert or access elements by position, zero-based index can contain duplicate element
    * has methods define by Collection interface and other own methods
    * methods throw ``UnsupportedOperationException`` if not modifiable, ``ClassCastException`` if
      one object is incompatible with another
    * ``add(int, Object)``: insert object into this list at given index, existing elements at or
      beyond are shifted and not overwritten
    * ``addAll(int, Collection)``: insert all elements of argument at given index, existing
      elements are shifted and not overwritten, return true if this list changes
    * ``get(int)``: return object stored at given index
    * ``indexOf(Object)``: return index of last instance of given object in this list, return 1
      if not found
    * ``listIterator()``: return iterator to the start of this list
    * ``listIterator(int)``: return iterator of this list that start at given index
    * ``remove(int)``: remove element at given index, return removed element, list is compacted,
      indexes of subsequent elements are decremented by one
    * ``set(int, Object)``: assign given object at given index
    * ``subList(int start, int end)``: return list from start to end (exclusive), returned
      elements are also referenced by the invoking object

    .. code-block:: java

       List<Integer> l1 = new ArrayList<Integer>();
       l1.add(1);
       l1.add(2);
       System.out.println(l1);
   
       List<String> l2 = new LinkedList<String>();
       l2.add("hello");
       l2.add("world");
       System.out.println(l2);



LinkedList
----------
    * implemented by extending AbstractSequentialList
    * ``LinkedList()``, ``LinkedList(Collection)``: create empty linked list or initialized with
      elements of argument
    * ``add(int, Object)``: add object at given index, can throw ``IndexOutOfBoundsException``
    * ``add(Object)``: add object at end of the list, return true on success
    * ``addAll(Collection)``: add all elements of argument at the end of the list, throw
      ``NullPointerException`` if given collection is null
    * ``addAll(int, Collection)``: add all elements of argument at given index
    * ``addFirst(Object)``: add element at the start of the list
    * ``addLast(Object)``: add element at the end of the list
    * ``clear()``: remove all elements
    * ``clone()``:return shallow copy of this linked list
    * ``contains(Object)``: true if list contain given argument
    * ``get(int)``: return element at given index, throw ``IndexOutOfBoundsException``
    * ``getFirst()``: return first element, throw ``NoSuchElementException``
    * ``getLast()``: return last element, throw ``NoSuchElementException``
    * ``indexOf(Object)``: return index of first occurrence of given argument, -1 if not found
    * ``lastIndexOf(Object)``: return index of last occurrence of given argument, -1 if not found
    * ``listIterator(int)``: return list iterator at given position, throw
      ``IndexOutOfBoundsException``
    * ``remove(int)``: remove and return element at given index, throw ``NoSuchElementException``
    * ``remove(Object)``: remove first occurrence of argument and return true if success, throw
      ``IndexOutOfBoundsException``
    * ``removeFirst()``: remove and return first element
    * ``removeLast()``: remove and return last element
    * ``set(int, Object)``: replace element at given index with given object
    * ``size()``: return size of the list
    * ``toArray()``: return an array with elements of the list, throw ``NullPointerException``
    * ``toArray(Object[])``: return an array with elements of the list, with runtime type as
      specified

    .. code-block:: java

       LinkedList<Integer> l = new LinkedList<Integer>();
       l.add(1);
       l.add(2);
       int x = l.remove(1); // x = 2



ArrayList
---------
    * extend AbstractList and implement List interface, support dynamic array
    * ``ArrayList()``, ``ArrayList(Collection)``, ``ArrayList(int)``: create empty array list or
      initialized with elements or specific capacity
    * ``add(int, Object)``: add given element at given index
    * ``add(Object)``: add element at end, return true on success
    * ``addAll(Collection)``: add all elements of argument at end
    * ``addAll(int, Collection)``: add all elements of given collection at specific index
    * ``clear()``: remove all elements
    * ``clone()``: return shallow copy
    * ``contains(Object)``: return true if list contain argument
    * ``ensureCapacity(int)``: increase capacity to have at least specified capacity
    * ``get(int)``: return element at given index
    * ``indexOf(Object)``: return index of first occurrence of argument, -1 if not found
    * ``lastIndexOf(Object)``: return index of last occurrence of given argument, -1 if not found
    * ``remove(int)``: remove and return element at given index
    * ``removeRange(int start, int end)``: remove elements from start to end, exclusive
    * ``set(int, Object)``: replace element at given index with given argument
    * ``size()``: return size
    * ``toArray()``: return an array with elements of the list
    * ``toArray(Object[])``: return an array with elements of the list, with runtime type as
    * ``trimToSize()``: trim the capacity to current size

    .. code-block:: java

       ArrayList<Integer> l = new ArrayList<Integer>();
       l.add(1);
       l.add(2);
       int x = l.remove(1); // x = 2



Set
---
    * ``AbstractSet``: extend AbstractCollection and implement most of the Set interface
    * no duplicate elements, only methods inherited from Collection
    * ``add(Object)``: add object to the collection
    * ``clear()``: remove all objects
    * ``contains(Object)``: true if object exists
    * ``isEmpty()``: true if no elements
    * ``iterator()``: return iterator object for the collection
    * ``remove(Object)``: remove specific object
    * ``size()``: return number of elements

    .. code-block:: java

       Set<Integer> s = new HashSet<Integer>();
       s.add(1);
       s.add(1);
       s.add(2);
       s.add(3);
       s.size(); // 3



Sorted Set
----------
    * extend Set, elements in ascending order
    * has implementations in various classes like TreeSet
    * most methods throw ``NoSuchElementException`` when no items in the set
    * throw ``ClassCastException`` for incompatible objects
    * null is not allowed and throw ``NullPointerException``
    * ``comparator()``: return this set's comparator, return null for natural ordering
    * ``first()``: return first element
    * ``headSet(Object)``: return sorted set with elements less than argument, returned elements
      are also referenced by invoking object
    * ``last()``: return last element
    * ``subSet(Object start, Object end)``: return sorted set with elements between start and
      end, exclusive, returned elements are also referenced by invoking object
    * ``tailSet(Object)``: return sorted set with elements greater than argument, returned
      elements are also referenced by invoking object

    .. code-block:: java

       SortedSet<Integer> s = new TreeSet<Integer>();
       s.add(3);
       s.add(2);
       s.add(9);
       s.add(1);
   
       java.util.Iterator<Integer> it = s.iterator();
   
       while (it.hasNext()) {
           System.out.println(it.next());
       }



HashSet
-------
    * extend AbstractSet and implement Set interface
    * ``HashSet()``, ``HashSet(Collection)``, ``HashSet(int)``: create default hash set or initialize
      with elements or specific capacity
    * ``HashSet(int, float)``: create hash set with given capacity and fill ratio/load capacity,
      fill ratio must be between 0.0 and 1.0, which determine how full the set can be before
      resized
    * ``add(Object)``: add element, return true if success and element is not present
    * ``clear()``: remove all elements
    * ``clone()``: return shallow copy, elements are not cloned
    * ``contains(Object)``: return true if contain argument
    * ``isEmpty()``: return true if empty
    * ``iterator()``: return iterator
    * ``remove(Object)``: remove given argument, return true if success and element is present
    * ``size()``: return number of elements

    .. code-block:: java

       HashSet<Integer> h = new HashSet<>();
       System.out.println(h.add(1));
       System.out.println(h.add(1)); // false



LinkedHashSet
-------------
    * extend HashSet, but no new member is added
    * maintain linked list of entries, allow insertion-order iteration
    * ``HashSet()``, ``HashSet(Collection)``: create default hash set
    * ``LinkedHashSet(int)``: create linked hash set with specific capacity
    * ``LinkedHashSet(int, float)``: create linked hash set with specific capacity and fill ratio

    .. code-block:: java

       LinkedHashSet<Integer> h = new LinkedHashSet<>();
       System.out.println(h.add(1));
       System.out.println(h.add(1)); // false



TreeSet
-------
    * implement Set interface that uses a tree for storage
    * objects are sorted and ascending order
    * access time is fast, suited for large amount of information that must be found quickly
    * ``TreeSet()``, ``TreeSet(Collection)``: create empty tree set or initialize with argument
    * ``TreeSet(Comparator)``: create empty tree set that uses specific comparator
    * ``TreeSet(SortedSet)``: create tree set with elements of argument
    * ``add(Object)``: add given element
    * ``addAll(Collection)``: add all elements of given collection
    * ``clear()``: remove all elements
    * ``clone()``: return shallow copy
    * ``comparator()``: return comparator used, null if natural ordering
    * ``contains(Object)``: return true if element present
    * ``first()``: return first element
    * ``headSet(Object)``: return sorted set with elements less than argument
    * ``isEmpty()``: return true if empty
    * ``iterator()``: return iterator
    * ``last()``: return last element
    * ``remove(Object)``: remove specific element if present, return true on success
    * ``size()``: return size
    * ``subSet(Object start, Object end)``: return sorted set of elements range from start to
      end, exclusive
    * ``tailSet(Object)``: return sorted set of elements greater than or equal to argument

    .. code-block:: java

       TreeSet<Integer> h = new TreeSet<>();
       System.out.println(h.add(1));
       System.out.println(h.remove(1));
       System.out.println(h.remove(1)); // false



Map
---
    * ``AbstractMap``: implement Map interface
    * maps unique keys to values
    * has implementations in various classes like HashMap
    * most methods throw ``NoSuchElementException`` when no items in the map
    * throw ``ClassCastException`` for incompatible objects
    * null is not allowed and throw ``NullPointerException``
    * ``UnsupportedOperationException`` for changing unmodifiable map
    * ``clear()``: remove all key-value pairs
    * ``containsKey(Object)``: true if contain argument as key
    * ``containsValue(Object)``: true if contain argument as value
    * ``entrySet()``: return a Set with entries of the map, set-view of the map
    * ``equals(Object)``: true if given object is a map and contain same entries
    * ``get(Object)``: return value of given key argument
    * ``hashCode()``: return hash code for this map
    * ``isEmpty()``: true if map is empty
    * ``keySet()``: return a Set with keys of this map, set-view of keys of the map
    * ``put(Object k, Object v)``: add key-value pair, existing value is overwritten, return null
      if key does not already exist, otherwise previous value is returned
    * ``putAll(Map)``: put all entries from given map
    * ``remove(Object)``: remove entry whose key equals argument
    * ``size()``: return size of map
    * ``values()``: return a collection with values in the map, collection-view of values of map

    .. code-block:: java

       Map<String, Integer> m = new HashMap<String, Integer>();
       m.put("hello", 1);
       m.put("world", 2);
       System.out.println(m);



Map.Entry
---------
    * interface to enable working with a map entry
    * ``Map.entrySet()`` return a set, which is a Map.Entry object
    * ``equals(Object)``: true if argument is Map.Entry and key-value pairs are equal to
      this object
    * ``getKey()``: return key for this map entry
    * ``getValue()``: return value for this map entry
    * ``hashCode()``: return hash code for this map entry
    * ``setValue(Object)``: set value for this map entry to argument, ``ClassCastException``
      if argument is not correct type, `NullPointerException` if argument is null and map
      does not allow null keys, `UnsupportedOperationException` if map cannot be changed

    .. code-block:: java

       Map<String, Integer> m = new HashMap<String, Integer>();
       m.put("hello", 1);
       m.put("world", 2);
   
       Set<Map.Entry<String, Integer>> s = m.entrySet();
       System.out.println(s);



SortedMap
---------
    * extends Map, entries are in ascending key order
    * has implementations in various classes like TreeMap
    * most methods throw ``NoSuchElementException``
    * ``ClassCastException`` for incompatible object
    * ``NullPointerException`` when null object is used and map does not allow
    * ``comparator()``: return this map's comparator, null if natural ordering is used
    * ``firstKey()``: return first key in this map
    * ``headMap(Object)``: return sorted map with keys that are less than argument
    * ``lastKey()``: return last key of this map
    * ``subMap(Object start, Object end)``: return sorted map with keys greater than or equal to
      start and less than end
    * ``tailMap(Object)``: return sorted map with keys greater than or equal to argument

    .. code-block:: java

       TreeMap<Integer, String> m = new TreeMap<Integer, String>();
       m.put(1, "hello");
       m.put(4, "bar");
       m.put(2, "world");
       m.put(3, "foo");
   
       Set<Map.Entry<Integer, String>> s = m.entrySet();
   
       Iterator<Map.Entry<Integer, String>> i = s.iterator();
   
       while (i.hasNext()) {
           System.out.println(i.next());
       }



HashMap
-------
    * use hashtable, constant time for basic operations
    * ``HashMap()``, ``HashMap(Map)``, ``HashMap(int)``: create default hash map or initialize with
      given map or capacity
    * ``HashMap(int, float)``: create hash map with specific capacity and fill ratio
    * ``clear()``: clear all mappings
    * ``clone()``: return shallow copy, keys and values are not cloned
    * ``containsKey(Object)``: return true if contain argument as key
    * ``containsValue(Object)``: return true if one or more keys map to argument
    * ``entrySet()``: return a set of mappings
    * ``get(Object)``: return value of given key argument
    * ``isEmpty()``: return true if empty
    * ``keySet()``: return a set of keys
    * ``put(Object k, Object v)``: associate given value to given key
    * ``putAll(Map)``: copies all mappings from argument, existing same mappings are replaced
    * ``remove(Object)``: remove mapping of given key, return old value if present
    * ``size()``: return size
    * ``values()``: return a collection of values

    .. code-block:: java

       HashMap<Integer, String> m = new HashMap<Integer, String>();
       m.put(1, "hello");
       m.put(2, "world");
   
       System.out.println(m.remove(1)); // hello
       System.out.println(m.remove(1)); // null



TreeMap
-------
    * implement Map by using tree, efficient way of storing key-value pairs in ascending order
    * ``TreeMap()``, ``TreeMap(Comparator)``, ``TreeMap(Map)``, ``TreeMap(SortedMap)``: create empty
      tree map or with given comparator or initialize with entries from Map or SortedMap
    * ``clear()``: remove all mappings
    * ``clone()``: return shallow copy
    * ``comparator()``: return comparator, null if natural order is used
    * ``containsKey(Object)``: return true if contain argument as key
    * ``containsValue(Object)``: return true if one or more keys map to argument
    * ``entrySet()``: return set view of mappings
    * ``firstKey()``: return first key
    * ``get(Object)``: return value of given key argument
    * ``headMap(Object)``: return sorted map whose keys are less than argument
    * ``keySet()``: return set view of keys
    * ``lastKey()``: return last key
    * ``put(Object k, Object v)``: associate given value to given key
    * ``putAll(Map)``: copy all mappings from argument
    * ``remove(Object)``: remove mapping for the key, return object if present, else null
    * ``size()``: return size
    * ``subMap(Object start, Object end)``: return sorted map with keys range from start to end,
      exclusive
    * ``tailMap(Object)``: return sorted map with keys greater than or equal to argument
    * ``values()``: return a collection of values

    .. code-block:: java

       TreeMap<Integer, String> m = new TreeMap<Integer, String>();
       m.put(1, "hello");
       m.put(3, "foo");
       m.put(2, "world");
       System.out.println(m.keySet()); // [1, 2, 3]



WeakHashMap
-----------
    * only store weak references to keys, allowing key-value pair to be garbage-collected when
      the key is no longer referenced outside
    * useful for registry-like data structures
    * function same as HashMap, except entry is removed once memory manager no longer has
      strong reference to key object
    * weak reference: garbage collector can reclaim object's memory at any time, no need to
      wait until system out of memory
    * ``WeakHashMap()``: create default WeakHashMap with capacity of 16 and load factor of 0.75
    * ``WeakHashMap(int)``: create empty WeakHashMap with specific capacity
    * ``WeakHashMap(int, float)``: create WeakHashMap with specific capacity and load factor
    * ``WeakHashMap(Map)``: create WeakHashMap with given mappings
    * ``clear()``: clear all mappings
    * ``containsKey(Object)``: return true if contain argument as key
    * ``containsValue(Object)``: return true if one or more keys map to argument
    * ``entrySet()``: return set of mappings
    * ``get(Object)``: return value of given key
    * ``isEmpty()``: return true if empty
    * ``keySet()``: return a set of keys
    * ``put(Object k, Object v)``: associate given value to given key
    * ``putAll(Map)``: copy all mappings from argument, existing same mappings will be replaced
    * ``remove(Object)``: remove mapping for the key, return object if present, else null
    * ``size()``: return size
    * ``values()``: return a collection of values

    .. code-block:: java

       WeakHashMap<Integer, String> m = new WeakHashMap<Integer, String>();
       m.put(1, "hello");
       m.put(3, "foo");
       m.put(2, "world");
       System.out.println(m.containsKey(3)); // true



LinkedHashMap
-------------
    * extend HashMap, use linked list of entries in order inserted
    * can also create LinkedHashMap that return elements in access order
    * ``LinkedHashMap()``: create default LinkedHashMap
    * ``LinkedHashMap(int)``: create LinkedHashMap with specific capacity
    * ``LinkedHashMap(int, float)``: create LinkedHashMap with specific capacity and fill ratio
    * ``LinkedHashMap(Map)``: create LinkedHashMap with given mappings
    * ``LinkedHashMap(int, float, boolean)``: create LinkedHashMap with specific capacity, fill
      ratio and storing order, insertion if false, last access if true
    * ``clear()``: remove all mappigs
    * ``containsKey(Object)``: return true if contain argument as key
    * ``get(Object)``: return value of given key
    * ``removeEldesEntry(Map.Entry)``: return true if map should remove eldest entry

    .. code-block:: java

       LinkedHashMap<Integer, String> m = new LinkedHashMap<Integer, String>(3, 0.5f, true);
       m.put(1, "hello");
       m.put(2, "world");
       m.put(3, "foo");
       m.get(2);
       for (String v : m.values()) {
           System.out.println(v); // "hello, foo, world" instead of "hello, world, foo"
       }



IdentityHashMap
---------------
    * implement AbstractMap, similar to HashMap, but uses reference equality when comparing
      elements
    * not general purpose Map implementation
    * use rarely where reference equality is required
    * constant time for basic operations assuming hash function disperse elements properly
    * ``IdentityHashMap()``: create empty IdentityHashMap with default maximum size of 21
    * ``IdentityHashMap(int)``, ``IdentityHashMap(Map)``: create IdentityHashMap with specific
      maximum size or initialized with mappings
    * ``clear()``: remove all mappings
    * ``clone()``: return shallow copy, key-value pairs are not cloned
    * ``containsKey(Object)``: return true if contain argument as key
    * ``containsValue(Object)``: return true if one or more keys map to argument
    * ``entrySet()``: return a set of mappings
    * ``equals(Object)``: compare argument with this map for equality
    * ``get(Object)``: return value of given key
    * ``hashCode()``: return hash code value for this map
    * ``isEmpty()``: return true if empty
    * ``keySet()``: return an identity-based set of keys
    * ``put(Object k, Object v)``: associate given value to given key
    * ``putAll(Map)``: copy all mappings from argument, existing same mappings will be replaced
    * ``remove(Object)``: remove mapping for the key, return object if present, else null
    * ``size()``: return size
    * ``values()``: return a collection of values

    .. code-block:: java

           IdentityHashMap<Integer, String> m1 = new IdentityHashMap<Integer, String>();
           m1.put(1, "hello");
           m1.put(2, "world");
           m1.put(3, "foo");
   
           IdentityHashMap<Integer, String> m2 = new IdentityHashMap<Integer, String>();
           m2.put(1, "test");
           m1.putAll(m2); // "1 = test"



Enumeration
-----------
    * define methods to enumerate elements in a collection
    * legacy interface, superceded by Iterator
    * still used by methods in classes such as Vector, Properties and other API classes
    * ``hasMoreElements()``: must return true while there are more elements to extract
    * ``nextElement()``: must return next object in enumeration as generic Object reference

Algorithms
----------
    * static methods, most methods throw ``ClassCastException`` or ``UnsupportedOperationException``
    * ``binarySearch(List, Object, Comparator)``: search for value according to comparator,
      return position of value or -1 if not found
    * ``binarySearch(List, Object)``: search for value in sorted list
    * ``copy(List l1, List l2)``: copy elements of l2 to l1
    * ``enumeration(Collection)``: return enumeration over argument
    * ``fill(List, Object)``: assign object to each element of list
    * ``indexOfSubList(List, List subList)``: search list for first occurrence of subList, return
      index of first match or -1 if not found
    * ``lastIndexOfSubList(List, List subList)``: search list for last occurrence of subList,
      return index or -1 if not found
    * ``list(Enumeration)``: return ArrayList with elements of argument
    * ``max(Collection)``: return max element in collection
    * ``max(Collection, Comparator)``: return max element in collection, determined by comparator
    * ``min(Collection)``: return min element in collection
    * ``min(Collection, Comparator)``: return min element in collection, determined by comparator
    * ``nCopies(int, Object)``: return specific copies of Object, as immutable list
    * ``replaceAll(List, Object old, Object new)``: replace all occurrences of old with new,
      return true if at least one replacement
    * ``reverse(List)``: reverse the list
    * ``reverseOrder()``: return reverse comparator
    * ``rotate(List, int)``: rotate specific places to right, provide negative value for left
    * ``shuffle(List)``: shuffle elements in the list
    * ``shuffle(List, Random)``: shuffle elements using given argument as source of random numbers
    * ``singleton(Object)``: return argument as immutable set, easy way to convert single object
      into set
    * ``singletonList(Object)``: return argument as immutable list, easy way to convert single
      object into list
    * ``singletonMap(Object k, Object v)``: return as immutable map, easy way to convert single
      key-value pair into map
    * ``sort(List)``: sort elements by natural ordering
    * ``sort(List, Comparator)``: sort elements by comparator
    * ``swap(List, int i1, int i2)``: swap elements at i1 and i2
    * ``synchronizedCollection(Collection)``: return thread-safe collection
    * ``synchronizedList(List)``: return thread-safe list
    * ``synchronizedMap(Map)``: return thread-safe map
    * ``synchronizedSet(Set)``: return thread-safe set
    * ``synchronizedSortedMap(SortedMap)``: return thread-safe sorted map
    * ``synchronizedSortedSet(SortedSet)``: return thread-safe sorted set
    * ``unmodifiableCollection(Collection)``: return unmodifiable collection
    * ``unmodifiableList(List)``: return unmodifiable list
    * ``unmodifiableMap(Map)``: return unmodifiable map
    * ``unmodifiableSet(Set)``: return unmodifiable set
    * ``unmodifiableSortedMap(SortedMap)``: return unmodifiable sorted map
    * ``unmodifiableSortedSet(SortedSet)``: return unmodifiable sorted set

Iterator
--------
    * to cycle through elements in a collection
    * implement Iterator or ListIterator interface
    * ListIterator extend Iterator for bidirectional traversal of list and modification
    * each collection class provide ``iterator()``
    * use a loop with a call to ``hasNext()`` and ``next()`` to access each element
    * ``hasNext()``: return true if more elements remain
    * ``next()``: return next element, throw ``NoSuchElementException`` if not exist
    * ``remove()``: remove current element, throw ``IllegalStateException`` if ``next()`` is not
      called after
    * **ListIterator**
        - ``add(Object)``: insert argument in front of element returned by ``next()``
        - ``hasNext()``: return true if next element exist
        - ``hasPrevious()``: return true if previous element exist
        - ``next()``: return next element
        - ``nextIndex()``: return index of next element, return size of list if no next element
        - ``previous()``: return previous element
        - ``previousIndex()``: return index of previous index, return -1 if not exist
        - ``remove()``: remove current element
        - ``set(Object)``: assign argument to current element, which is the element returned by
          ``next()`` or ``previous()``

    .. code-block:: java

       ArrayList<Integer> a = new ArrayList<>();
       a.add(1);
       a.add(2);
       a.add(3);
   
       ListIterator<Integer> itr = a.listIterator();
   
       while (itr.hasNext()) {
           Integer e = itr.next();
           itr.set(e + 1);
       }
   
       itr = a.listIterator();
   
       while (itr.hasNext()) {
           System.out.println(itr.next()); // 2, 3, 4
       }



Comparator
----------
    * interface to define what sorted order means in collections
    * ``compare(Object o1, Object o2)``: compare two elements for order, return 0 if equal, > 0
      if o1 is greater, < 0 if o2 is greater
    * ``equals(Object o1, Object o2)``: return true if equal

`back to top <#java>`_

Generics
========

* `Generic Methods`_, `Generic Classes`_
* allow to specify a set of related methods or types with single method or class declaration
* also provide compile-time type safety


Generic Methods
---------------
    * can have a single generic method declaration called with arguments of different types
    * compiler handles each method call based on argument types
    * all declarations must have a type parameter section
    * each type parameter section contain one or more type parameters
    * type parameters can be used to declare return type
    * type parameters can only represent reference types, not primitive types

    .. code-block:: java

       public static <E> void printArray(E[] inputArray) {
           for (E e : inputArray) {
               System.out.println(e);
           }
       }
   
       Integer[] intArray = { 1, 2, 3 };
       String[] stringArray = { "hello", "world" };
       printArray(intArray);
       printArray(stringArray);


    * can restrict types that are allowed to be passed to type parameter

        .. code-block:: java

           public static <E extends Number> void printArray(E[] inputArray) {
               for (E e : inputArray) {
                   System.out.println(e);
               }
           }
   
           Integer[] intArray = { 1, 2, 3 };
           Double[] doubleArray = { 1.0, 2.2, 3.3 };
           String[] stringArray = { "hello", "world" };
           printArray(intArray); // ok
           printArray(doubleArray); // ok
           printArray(stringArray); // error



Generic Classes
---------------
    * class name followed by a type parameter section
    * can have one or more type parameters separated by commas, called parameterized class or
      parameterised types

    .. code-block:: java

       class Generic<T> {
           private T t;
   
           Generic(T t) {
               this.t = t;
           }
   
           public T get() {
               return t;
           }
       }
   
       Generic<Integer> i = new Generic<Integer>(1);
       Generic<String> s = new Generic<String>("hello");
       System.out.println(i.get()); // 1
       System.out.println(s.get()); // "hello"


`back to top <#java>`_

Serialization
=============

* `ObjectInputStream`_, `ObjectOutputStream`_, `Serializing`_, `Deserializing`_
* object represented as sequence of bytes, including data, type and types of data
* serialized object written to a file can be read and deserialized to recreate object in memory
* serialization and deserialisation is JVM independent, operations can be on different platform


ObjectInputStream
-----------------
    * ``readObject()``: get next object out of the stream and deserialize it, return an Object

ObjectOutputStream
------------------
    * ``writeObject(Object)``: serialize an object and send it to output stream

Serializing
-----------
    * class must implement ``java.io.Serializable`` interface and all fields must be serialisable
      or marked `transient`
    * convention to serialize an object to a file with ``.ser`` extension

    .. code-block:: java

       class S implements java.io.Serializable {
           public String name;
           public transient int age;
       }
   
       S s = new S();
       s.name = "foo";
       s.age = 9;
   
       try {
           FileOutputStream f = new FileOutputStream("serializable.ser");
           ObjectOutputStream o = new ObjectOutputStream(f);
           o.writeObject(s);
           o.close();
           f.close();
       } catch (IOException e) {
           e.printStackTrace();
       }



Deserializing
-------------

    .. code-block:: java

       S s = null;
   
       try {
           FileInputStream f = new FileInputStream("serializable.ser");
           ObjectInputStream i = new ObjectInputStream(f);
           s = (S) i.readObject(); // need to cast proper type
           i.close();
           f.close();
       } catch (IOException e) {
           e.printStackTrace();
       } catch (ClassNotFoundException e) {
           // need to catch as it is declared by "readObject()"
           e.printStackTrace();
       }
   
       System.out.println(s.name); // "foo"
       System.out.println(s.age); // 0, value is not sent to output stream as it is transient


`back to top <#java>`_

Networking
==========

* `Socket Programming`_, `URL Processing`_, `Email`_
* in ``java.net`` package


Socket Programming
------------------
    * sockets provide communication mechanism using TCP
    * server instantiate ServerSocket object with port number
    * server inovkes ``accept()`` to wait until client connects
    * client instantiate Socket object with server name and port number to connect to
    * after connected, client will have Socket object that can communicate with the server
    * server's ``accept()`` return reference to new socket that is connected to client's socket
    * after connection, communication can be done using I/O streams, as each socket has
      OutputStream and InputStream
    * client's OutputStream is connected to server's InputStream, and server's InputStream to
      client's OutputStream
    * as it use TCP, data can be sent across both streams at the same time
    * **ServerSocket**
        - use by server applications, all constructors throw ``IOException``
        - ``ServerSocket()``: create unbound server socket, must use ``bind()`` when ready
        - ``ServerSocket(int)``: create server socket with given port
        - ``ServerSocket(int port, int backlog)``: create server socket with given port and
          backlog, number of incoming clients to store in a wait queue
        - ``ServerSocket(int port, int backlog, InetAddress addr)``: create server socket with
          local IP address to bind to, InetAddress is used for servers that may have multiple
          IP
        - ``getLocalPort()``: return listening port
        - ``accept()``: wait for incoming client, block until client connect or socket time out,
          must set timeout value with ``setSoTimeout()``, return Socket object if client
          connect, throw ``IOException``
        - ``setSoTimeout(int)``: set timeout value during ``accept()``
        - ``bind(SocketAddress, int backlog)``: bind socket to given SocketAddress object
    * **Socket**
        - use by both client and server, client instantiate one and server obtain one from
          ``accept()``
        - all constructors, except default one, throw ``IOException``
        - ``Socket(String, int)``: connect to given server at given port, throw
          ``UnknownHostException``
        - ``Socket(InetAddress, int)``: connect to given server, using InetAddress, at given port
        - ``Socket(String host, int port, InetAddress local, int localPort)``: connect to given
          host and port, creating socket on local host at given address and port
        - ``Socket(InetAddress host, int port, InetAddress local, int localPort)``: connect to
          given host, using InetAddress, and port, creating socket on local host at given
          address and port
        - ``Socket()``: create unconnected socket, need to use ``connect()``
        - ``connect(SocketAddress, int timeout)``: connect socket to given host, throw
          ``IOException``
        - ``getInetAddress()``: return address to which the socket is connected
        - ``getPort()``: return port of the socket bounded on remote machine
        - ``getLocalPort()``: return port of socket on local machine
        - ``getRemoteSocketAddress()``: return address of remote socket
        - ``getInputStream()``: return InputStream of socket, which is connected to OutputStream
          of remote socket, throw ``IOException``
        - ``getOutputStream()``: return OutputStream of socket, which is connected to InputStream
          of remote socket, throw ``IOException``
        - ``close()``: close the socket, throw ``IOException``
    * **InetAddress**
        - ``InetAddress(byte[])``: create object with raw IP address, return InetAddress
        - ``getByAddress(String, byte[])``: create object with given host name and IP address,
          return InetAddress
        - ``getByName(String)``: return IP address, as InetAddress object, of given host
        - ``getHostAddress()``: return IP address string
        - ``getHostName()``: get host name string
        - ``getLocalHost()``: return local host as InetAddress
        - ``toString()``: convert IP address to string
    * **Client Example**

        .. code-block:: java

           public class SocketClient {
               public static void main(String[] args) {
                   String serverName = args[0];
                   int port = Integer.parseInt(args[1]);
   
                   try {
                       System.out.println("Connecting to " + serverName + " on port " + port);
                       Socket client = new Socket(serverName, port);
   
                       System.out.println("Connected to " + client.getRemoteSocketAddress());
                       OutputStream outToServer = client.getOutputStream();
                       DataOutputStream out = new DataOutputStream(outToServer);
   
                       out.writeUTF("Hello from " + client.getLocalSocketAddress());
                       InputStream inFromServer = client.getInputStream();
                       DataInputStream in = new DataInputStream(inFromServer);
   
                       System.out.println("Reply from server: " + in.readUTF());
                       client.close();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
           }


    * **Server Example**

        .. code-block:: java

           public class SocketServer extends Thread {
               private ServerSocket serverSocket;
   
               public SocketServer(int port) throws IOException {
                   serverSocket = new ServerSocket(port);
                   serverSocket.setSoTimeout(10000);
               }
   
               public void run() {
                   while (true) {
                       try {
                           System.out.println("Waiting for client on port "
                                           + serverSocket.getLocalPort() + "...");
                           Socket server = serverSocket.accept();
   
                           System.out.println("Connected to " + server.getRemoteSocketAddress());
                           DataInputStream in = new DataInputStream(server.getInputStream());
   
                           System.out.println(in.readUTF());
   
                           DataOutputStream out = new DataOutputStream(server.getOutputStream());
                           out.writeUTF("Disconnecting from " + server.getLocalSocketAddress());
                           server.close();
                       } catch (SocketTimeoutException e) {
                           System.out.println("Socket timed out!");
                           break;
                       } catch (IOException e) {
                           e.printStackTrace();
                           break;
                       }
                   }
               }
   
               public static void main(String[] args) {
                   int port = Integer.parseInt(args[0]);
                   try {
                       Thread t = new SocketServer(port);
                       t.start();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
           }



URL Processing
--------------
    * url: ``protocol://host:port/path?query#ref``
    * host is also called authority, path is also called filename
    * example protocols: HTTP, HTTPS, FTP, File
    * all constructors throw ``MalformedURLException``
    * ``URL(String protocol, String host, int port, String file)``: create URL
    * ``URL(String protocol, String host, String file)``: create URL with default port for given
      protocol
    * ``URL(String url)``: create URL from given string
    * ``URL(URL context, STring url)``: create URL by parsing arguments
    * ``getPath()``: return path of URL
    * ``getQuery()``: return query part of URL
    * ``getAuthority()``: return authority/host of URL
    * ``getPort()``: return port of URL
    * ``getDefaultPort()``: return default port for the protocol of URL
    * ``getProtocol()``: return protocol of URL
    * ``getHost()``: return host of URL
    * ``getFile()``: return filename/path of URL
    * ``getRef()``: return reference part of URL
    * ``openConnection()``: open connection to URL for client communication, return URLConnection
      object, throw `IOException`

    .. code-block:: java

       URL url = new URL("https://www.amrood.com/index.htm?language=en#j2se");
   
       System.out.println(url.getProtocol());
       System.out.println(url.getAuthority());
       System.out.println(url.getFile());
       System.out.println(url.getHost());
       System.out.println(url.getPort());
       System.out.println(url.getDefaultPort());


    * **URLConnection**
        - ``getContent()``, ``getContent(Class[])``: return contents as object
        - ``getContentEncoding()``: return content-encoding header field as string
        - ``getConentLength()``: return content-length header field as int
        - ``getContentType()``: return content-type header field as string
        - ``getLastModified()``: return last-modified header field as int
        - ``getExpiration()``: return expired header field as long
        - ``getIfModifiedSince()``: return ifModifiedSince field as long
        - ``setDoInput(boolean)``: default argument true to denote the connection will be used
          for input
        - ``setDoOutput(boolean)``: default argument false to denote connection will not be used
          for output
        - ``getInputStream()``: return InputStream of connection for reading from the resource,
          throw ``IOException``
        - ``getOutputStream()``: return OutputStream of connection for writing to resource,
          throw ``IOException``
        - ``getURL()``: return URL object of connection

Email
-----
    * need JavaMail API and JAF (Java ACtivation Framework) installed
    * in ``javax.mail`` and ``javax.activation``;
    * **Simple Email**

        .. code-block:: java

           String from = "foo@mail.com";
           String to = "bar@mail.com";
           String host = "localhost";
   
           Properties properties = System.getProperties();
           properties.setProperty("mail.smtp.host", host);
           Session session = Session.getDefaultInstance(properties);
   
           try {
               MimeMessage message = new MimeMessage(session);
               message.setFrom(new InternextAddress(from));
               message.addRecipient(Message.RecipientType.TO, new InternetAddress(to));
               message.setSubject("Mail subject");
               message.setText("Mail text message");
               Transport.send(message);
               System.out.println("Message sent");
           } catch (MessagingException e) {
               e.printStackTrace();
           }


    * **Html Email**

        .. code-block:: java

           // same as simple mail, with "setContent()"
           message.setContent("<h1>Mail message</h1>");


    * **Sending with Attachment**

        .. code-block:: java

           // same as simple mail, with additional message parts
           BodyPart messageBodyPart = new MimeBodyPart();
           messageBodyPart.setText("Message body");
   
           Multipart multipart = new MimeMultipart();
           multipart.addBodyPart(messageBodyPart);
   
           messageBodyPart = new MimeBodyPart();
           String filename = "file.txt";
           DataSource source = new FileDataSource(filename);
           messageBodyPart.setDataHandler(new DataHandler(source));
           messageBodyPart.setFileName(filename);
           multipart.addBodyPart(messageBodyPart);
   
           message.setContent(multipart);


    * **Setting Authentication**

        .. code-block:: java

           // set system properties
           Properties properties = System.getProperties();
           properties.setProperty("mail.user", "user");
           properties.setProperty("mail.password", "password");


`back to top <#java>`_

Threads
=======

* `Thread Life Cycle`_, `Creating Thread`_, `Thread Synchronization`_
* `Interthread Communication`_, `Thread Deadlock`_, `Thread Control`_
* Java thread priorities range between MIN_PRIORITY, constant 1, and MAX_PRIORITY, constant 10
* every thread has default NORM_PRIORITY, constant 5
* thread priorities cannot guarantee execution order and platform dependent
* Multithreading: subdividing specific operations within single application into individual
  threads


Thread Life Cycle
-----------------
    * **New**
        - new thread begins its life cycle in new state and remain in it until started
        - also called born thread
    * **Runnable**
        - becomes runnable when newly born thread is started
        - runnable thread is considered to be executing its task
    * **Waiting**
        - waiting for another thread to perform a task
        - change back to runnable state when another thread signal
    * **Timed Waiting**
        - runnable thread can enter this state for specified time
        - transition back to runnable when time expire or a waiting event occur
    * **Terminated**
        - thread enter terminated/dead state when a task is completed or it is terminated

Creating Thread
---------------
    * **Using Runnable**
        - implement Runnable interface to execute class as a thread
        - must implement ``run()`` provided by Runnable, which is an entry point for a thread,
          need to put complete business logic inside the method
        - instantiate a Thread object, ``Thread(Runnable, String name)``
        - can start the created Thread object with ``start()``

        .. code-block:: java

           class MyRunnable implements Runnable {
               private Thread t;
               private String threadName;
   
               MyRunnable(String name) {
                   threadName = name;
                   System.out.println("Creating " + threadName);
               }
   
               public void run() {
                   System.out.println("Running " + threadName);
                   try {
                       for (int i = 1; i < 5; ++i) {
                           System.out.println("Thread: " + threadName + ", " + i);
                           Thread.sleep(50);
                       }
                   } catch (InterruptedException e) {
                       System.out.println("Thread " + threadName + " interrupted");
                   }
   
                   System.out.println("Thread " + threadName + " exiting");
               }
   
               public void start() {
                   System.out.println("Starting " + threadName);
                   if (t == null) {
                       t = new Thread(this, threadName);
                       t.start();
                   }
               }
           }
   
           MyRunnable r1 = new MyRunnable("Thread1");
           r1.start();


    * **Using Thread**
        - extending Thread class provide more flexibility
        - override ``run()``, which is an entry point for a thread, need to put complete
          business logic inside the method
        - call ``start()`` once Thread object is created
        - ``start()``: start the thread in a separate path of execution, then invoke ``run()``
        - ``run()``: invoked when this Thread object is instantiated using separate Runnable
          target
        - ``setName(String)``: change the name of Thread object
        - ``getName()``: return name of Thread object
        - ``setPriority(int)``: set thread priority, between 1 and 10
        - ``setDaemon(boolean)``: this Thread is daemon thread if argument is true
        - ``join(long)``: invoked on second thread, current thread is blocked until the second
          thread terminate or given time in ms passes
        - ``interrupt()``: interrupt this thread, will continue execution if it was blocked
        - ``isAlive()``: return true if alive, which is any time after the thread has been
          started
        - below static methods perform operation on currently running thread
        - ``yield()``: yield to any other threads of same priority that are waiting to be
          scheduled
        - ``sleep(long)``: block current thread for given amount of time in ms
        - ``holdsLock(Object)``: return true if current thread hold the lock on argument
        - ``currentThread()``: return reference to this thread
        - ``dumpStack()``: print stack trace for current thread, useful for debugging
          multithreaded applications

        .. code-block:: java

           class MyThread extends Thread {
               private Thread t;
               private String threadName;
   
               MyThread(String name) {}
   
               public void run() {}
   
               public void start() {}
           }
   
           MyThread t1 = new MyThread("Thread1");
           t1.start();



Thread Synchronization
----------------------
    * synchronizing multiple threads to make sure only one can access the same resource at a
      given point in time, implemented using monitors concept
    * each object in Java is associated with a monitor, which a thread can lock or unlock
    * only one thread at a time may hold a lock on a monitor
    * can create and sync threads by using ``synchronized`` blocks, in which shared resources are
      placed
    * ``synchronized(objectID)``: objectID is a reference to an object whose lock associates with
      the monitor that the synchronized statement represents

    .. code-block:: java

       class MyThread extends Thread {
           private Thread t;
           private String threadName;
           DemoObject d;
   
           MyThread(String name, DemoObject d) {
               threadName = name;
               this.d = d;
           }
   
           public void run() {
               synchronized (d) {
                   d.foo();
               }
               System.out.println("Thread " + threadName + " exiting");
           }
   
           public void start() {
               System.out.println("Starting " + threadName);
               if (t == null) {
                   t = new Thread(this, threadName);
                   t.start();
               }
           }
       }
   
       DemoObject d = new DemoObject();
       ThreadDemo t1 = new ThreadDemo("Thread-1", d);
       ThreadDemo t2 = new ThreadDemo("Thread-2", d);
   
       t1.start();
       t2.start();
   
       try {
           t1.join();
           t2.join();
       } catch (Exception e) {
           System.out.println("Interrupted");
       }



Interthread Communication
-------------------------
    * important for applications where two or more threads exchange information
    * all three methods are ``final``, available in all classes, and can only be called from
      within `synchronized` context
    * ``wait()``: this thread wait until another invoke ``notify()``
    * ``notify()``: wake up a single thread that is waiting on this object's monitor
    * ``notifyAll()``: wake up all threads that call ``wait()`` on the same object

    .. code-block:: java

       class Chat {
           boolean flag = false;
   
           public synchronized void Question(String msg) {
               if (flag) {
                   try {
                       wait();
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
   
               System.out.println(msg);
               flag = true;
               notify();
           }
   
           public synchronized void Answer(String msg) {
               if (!flag) {
                   try {
                       wait();
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
   
               System.out.println(msg);
               flag = false;
               notify();
           }
       }
   
       class T1 implements Runnable {
           Chat m;
           String[] s1 = { "Hi", "How are you?", "I am also doing fine" };
   
           public T1(Chat m1) {
               this.m = m1;
               new Thread(this, "Question").start();
           }
   
           public void run() {
               for (int i = 0; i < s1.length; i++) {
                   m.Question(s1[i]);
               }
           }
       }
   
       class T2 implements Runnable {
           Chat m;
           String[] s2 = { "Hi", "I'm good, and you?", "Great!" };
   
           public T2(Chat m2) {
               this.m = m2;
               new Thread(this, "Question").start();
           }
   
           public void run() {
               for (int i = 0; i < s2.length; i++) {
                   m.Answer(s2[i]);
               }
           }
       }
   
       Chat m = new Chat();
       new T1(m);
       new T2(m);



Thread Deadlock
---------------
    * deadlock: two or more threads blocked forever, waiting for each other, occur when
      multiple threads need same locks but obtain them in different order
    * ``synchronized`` keyword causes the executing thread to block while waiting for the lock
      or monitor
    * deadlock example

        .. code-block:: java

           public class DeadLockDemo {
               public static Object Lock1 = new Object();
               public static Object Lock2 = new Object();
   
               public static void main(String[] args) {
                   ThreadDemo1 t1 = new ThreadDemo1();
                   ThreadDemo2 t2 = new ThreadDemo2();
                   t1.start();
                   t2.start();
               }
   
               private static class ThreadDemo1 extends Thread {
                   public void run() {
                       synchronized (Lock1) {
                           System.out.println("Thread 1: Holding lock 1...");
   
                           try {
                               Thread.sleep(10);
                           } catch (InterruptedException e) {
                           }
                           System.out.println("Thread 1: Waiting for lock 2...");
   
                           synchronized (Lock2) {
                               System.out.println("Thread 1: Holding lock 1 & 2...");
                           }
                       }
                   }
               }
   
               private static class ThreadDemo2 extends Thread {
                   public void run() {
                       synchronized (Lock2) {
                           System.out.println("Thread 2: Holding lock 2...");
   
                           try {
                               Thread.sleep(10);
                           } catch (InterruptedException e) {
                           }
                           System.out.println("Thread 2: Waiting for lock 2...");
   
                           synchronized (Lock1) {
                               System.out.println("Thread 2: Holding lock 1 & 2...");
                           }
                       }
                   }
               }
           }


    * **Deadlock Solution Example**

        .. code-block:: java

           // changing the order of locks prevent deadlock situation
           private static class ThreadDemo1 extends Thread {
               public void run() {
                   synchronized (Lock1) {
                       synchronized (Lock2) {}
                   }
               }
           }
   
           private static class ThreadDemo2 extends Thread {
               public void run() {
                   synchronized (Lock1) {
                       synchronized (Lock2) {}
                   }
               }
           }



Thread Control
--------------
    * allow threads to be suspended, resumed or stopped completely
    * ``suspend()``: put thread in suspended state, can be resumed with ``resume()``
    * ``stop()``: stop thread completely
    * ``resume()``: resume a suspended thread using ``suspend()``
    * ``wait():`` cause current thread to wait until another thread invoke ``notify()``
    * ``notify()``: wake up single thread that is waiting on this object's monitor
    * latest versions of Java has deprecated ``suspend()``, ``resume()`` and ``stop()``

`back to top <#java>`_

Javadoc
=======

* a tool that comes with JDK, used to generate documentation in HTML from source code

.. code-block:: java

   // normal text comment
   /* normal text comment */
   
   /**
    * <h1>can include html tags</h1>
    * This is a doc comment
    *
    * @author author-name
    * @version version of doc
    * @since doc released date
    *        {@code} display text in code font
    *        {@docRoot} relative path to generate doc root directory
    * @deprecated deprecated-text
    * @exception add Trhows subheading
    *                {@inheritDoc} inherit comment from nearest superclass
    *                {@link} insert in-line link
    *                {@linkplain} link is displayed in plain text
    * @param add parameter
    * @return add return section
    * @see add See Also heading with link or text entry
    * @serial for default serializable field
    * @serialData data written by writeObject() or writeExternal()
    * @serialField ObjectStreamField component
    * @throws same as @exception
    *              {@value} to display value of static field
    */


`back to top <#java>`_

Java8
=====

* `Lambda`_, `Method References`_, `Functional Interfaces`_, `Default Methods`_
* `Streams`_, `Optional Class`_, `Date/Time API`_, `Base64`_

Lambda
------
    * optional type declaration, compile can inference from the value
    * can omit parenthesis for single parameter
    * optional curly braces for single statement in expression body
    * compiler can auto return if body has single expression
    * used to define inline implementation of functional interface, which only has single
      method
    * eliminate the need of anonymous class and provide functional programming capability
    * can refer to any final variable
    * throw compilation error if a variable is assigned a value second time

    .. code-block:: java

       interface Operation {
           int operation(int a, int b);
       }
   
       interface Print {
           void foo(String msg);
       }
   
       public class MyClass {
           private int operate(int a, int b, Operation o) {
               return o.operation(a, b);
           }
   
           public static void main(String[] args) {
               MyClass t = new MyClass();
               Operation addition = (int a, int b) -> a + b;
               Operation multiplication = (int a, int b) -> a * b;
   
               System.out.println(t.operate(1, 2, addition)); // 3
               System.out.println(t.operate(1, 2, multiplication)); // 2
   
               Print p = m -> System.out.println(m);
               p.foo("hello"); // "hello"
           }
       }



Method References
-----------------
    * allow to point methods by names
    * can use on static, instance and constructors using new operators

    .. code-block:: java

       ArrayList<Integer> nums = new ArrayList<>();
       nums.add(1);
       nums.add(2);
       nums.add(3);
   
       nums.forEach(System.out::println); // pass "println()" as static method reference



Functional Interfaces
---------------------
    * in ``java.util.function``, only have single functionality
    * e.g Predicate interface has ``test(Object)`` that test object to be true or false

    .. code-block:: java

       import java.util.function.Predicate;
   
       public static void printEven(List<Integer> l, Predicate<Integer> p) {
           for (Integer n : l) {
               if (p.test(n)) {
                   System.out.println(n);
               }
           }
       }
   
       List<Integer> l = Arrays.asList(1, 2, 3, 4);
       System.out.println("Even numbers:");
       printEven(l, n -> n % 2 == 0); // print "2, 4"



Default Methods
---------------
    * capability added for backward compatibility so that old interfaces can be used to
      leverage lambda expression
    * when a class implement two interfaces with same default methods, it needs to override the
      method or call one using `super`
    * starting form Java 8, interface can have static helper methods

    .. code-block:: java

       interface A {
           default void hello() {
               System.out.println("Hello from A");
           }
   
           static void foo() {
               System.out.println("Foo");
           }
       }
   
       interface B {
           default void hello() {
               System.out.println("Hello from B");
           }
       }
   
       class C implements A, B {
           public void hello() {
               A.super.hello();
               A.foo();
           }
       }



Streams
-------
    * abstract layer to process data in a declarative way
    * **Stream Characteristics**
        - provide sequence of elements of specific type
        - take sources, such as Collections, Arrays or I/O resources, as input sources
        - support aggregate operations such as filter, reduce, find, etc.
        - results can be pipelined as most operations return stream itself
        - stream operations do iterations internally
    * Collection interface has ``stream()`` and ``parallelStream()`` to generate a Stream

    .. code-block:: java

       List<Integer> myList = Arrays.asList(1, 2, 3);
   
       // forEach: iterate each element
       myList.forEach(System.out::println);
   
       // map: map each element to its correspond result
       myList.stream().map(n -> n * n).forEach(System.out::println); // 1, 4, 9
   
       // filter: eliminate elements based on criteria
       List<Integer> oddList = myList.stream().filter(n -> n % 2 != 0).collect(Collectors.toList());
       oddList.forEach(System.out::println); // 1, 3
   
       // limit: reduce size of the stream
       myList.stream().limit(2).forEach(System.out::println); // 1, 2
   
       // sorted: sort the stream
       myList = Arrays.asList(2, 3, 1);
       myList.stream().sorted().forEach(System.out::println); // 1, 2, 3
   
       // parallelStream: alternative of stream for parallel processing
       myList.parallelStream().map(n -> n * n).forEach(System.out::println);


    * **Collectors**
        - used to combine result of processing on elements of a stream
        - can be used to return a list or a string
        - can use statistics collectors to calculate all statistics when stream processing is
          done

        .. code-block:: java

           List<Integer> myList = Arrays.asList(1, 2, 3);
           List<String> myStringList = Arrays.asList("hello", "world");
   
           List<Integer> squareList = myList.stream().map(n -> n * n).collect(Collectors.toList());
           squareList.forEach(System.out::println); // 1, 4, 9
   
           String myListString = myStringList.stream().collect(Collectors.joining(", "));
           System.out.println(myListString); // "hello, world"
   
   
           // statistics collectors
           IntSummaryStatistics stats = myList.stream().mapToInt(n -> n).summaryStatistics();
           System.out.println(stats.getMax()); // 3
           System.out.println(stats.getMin()); // 1
           System.out.println(stats.getSum()); // 6
           System.out.println(stats.getAverage()); // 2.0



Optional Class
--------------
    * in ``java.util.Optinal``
    * container object used to represent null with absent value
    * has methods to handle values as available or not available instead of checking null
      values
    * inherits methods from ``java.lang.Object``
    * ``empty()``: return empty optional instance
    * ``equals(Object)``: true if argument is equal to this Optional
    * ``filter(Predicate)``: return Optional describing the value if it is present and matches
      argument, return empty Optional otherwise
    * ``flatMap(Function)``: apply argument if a value is present and return the result,
      otherwise return empty Optional
    * ``get()``: return value if present in this Optional, otherwise throw ``NoSuchElementException``
    * ``hashCode()``: return hash code value of present value, 0 if no value
    * ``ifpresent(Consumer)``: invoke argument with value if present, otherwise do nothing
    * ``isPresent()``: return true if value is present
    * ``map(Function)``: apply argument if value if present, return Optional describing the
      result if result is non-null
    * ``of(T)``: return Optional with given present non-null value
    * ``ofNullable(T)``: return Optional describing argument if non-null, otherwise return empty
      Optional
    * ``orElse(T)``: return value if present, otherwise return argument
    * ``orElseGet(Supplier)``: return value if present, otherwise invoke argument and return
      the result
    * ``orElseThrow(Supplier)``: return the contained value if present, otherwise throw exception
      to be created by the argument
    * ``toString()``: return non-empty string of this Optional suitable for debugging

    .. code-block:: java

       import java.util.Optional;
   
       public class MyClass {
   
           public static void main(String[] args) {
               MyClass t = new MyClass();
               Integer x = null;
               Integer y = Integer.valueOf(10);
   
               Optional<Integer> a = Optional.ofNullable(x);
               Optional<Integer> b = Optional.ofNullable(y);
               System.out.println(t.sum(a, b)); // 10
           }
   
           public Integer sum(Optional<Integer> a, Optional<Integer> b) {
               System.out.println("First parameter: " + a.isPresent()); // false
               System.out.println("Second parameter: " + b.isPresent()); // true
   
               Integer x = a.orElse(Integer.valueOf(0));
               Integer y = b.get();
               return x + y;
           }
       }



Date/Time API
-------------
    * in ``java.time``
    * immutable, no setter methods and more utility methods
    * **Local Date-Time**
        - simplified and time-zones are not required

        .. code-block:: java

           LocalDateTime currentTime = LocalDateTime.now();
           System.out.println("Current DateTime: " + currentTime);
   
           LocalDate d1 = currentTime.toLocalDate();
           System.out.println("date: " + d1);
   
           Month m = currentTime.getMonth();
           int day = currentTime.getDayOfMonth();
   
           System.out.println("Month: " + m + " day: " + day);
   
           LocalDateTime d2 = currentTime.withDayOfMonth(10).withYear(2000);
           System.out.println("Date 2: " + d2);
   
           LocalDate d3 = LocalDate.of(2000, Month.DECEMBER, 12);
           System.out.println("Date 3: " + d3);
   
           LocalTime d4 = LocalTime.of(22, 15);
           System.out.println("Date 4: " + d4);
   
           LocalTime d5 = LocalTime.parse("20:15:22");
           System.out.println("Date 5: " + d5);


    * **Zoned Date-Time**
        - to use with time zone

        .. code-block:: java

           ZoneId id = ZoneId.of("Africa/Abidjan");
           System.out.println("ZoneId: " + id);
   
           ZoneId current = ZoneId.systemDefault();
           System.out.println("Current Zone: " + current);


    * **Chrono Units**
        - in ``java.time.temporal.ChronoUnit``, enum to replace integer values used for Date/Time

        .. code-block:: java

           LocalDate current = LocalDate.now();
           System.out.println("Current date: " + current);
   
           LocalDate nextWeek = current.plus(1, ChronoUnit.WEEKS);
           System.out.println("Next week: " + nextWeek);
   
           LocalDate nextMonth = current.plus(1, ChronoUnit.MONTHS);
           System.out.println("Next month: " + nextMonth);
   
           LocalDate nextYear = current.plus(1, ChronoUnit.YEARS);
           System.out.println("Next year: " + nextYear);


    * **Period & Duration**
        - period: date based amount of time
        - duration: time based amount of time

        .. code-block:: java

           LocalDate current = LocalDate.now();
           System.out.println("Current date: " + current);
   
           LocalDate nextWeek = current.plus(1, ChronoUnit.WEEKS);
           System.out.println("Next week: " + nextWeek);
   
           Period p = Period.between(nextWeek, current);
           System.out.println("Period: " + p);
   
           LocalTime t1 = LocalTime.now();
           Duration oneHour = Duration.ofHours(1);
   
           LocalTime t2 = t1.plus(oneHour);
           Duration d = Duration.between(t1, t2);
           System.out.println("Duration: " + d);


    * **Temporal Adjusters**
        - to perform date mathematics

        .. code-block:: java

           LocalDate current = LocalDate.now();
           System.out.println("Current date: " + current);
   
           LocalDate nextTuesday = current.with(TemporalAdjusters.next(DayOfWeek.TUESDAY));
           System.out.println("Next Tuesday: " + nextTuesday);
   
           LocalDate firstInYear = LocalDate.of(current.getYear(), current.getMonth(), 1);
           LocalDate secondSaturday = firstInYear.with(TemporalAdjusters.nextOrSame(
                   DayOfWeek.SATURDAY)).with(TemporalAdjusters.next(DayOfWeek.SATURDAY));
           System.out.println("Second Saturday: " + secondSaturday);


    * can use ``toInstant()`` to convert original Date and Calendar objects

        .. code-block:: java

           Date current = new Date();
           System.out.println("Current: " + current);
   
           Instant now = current.toInstant();
           ZoneId currentZone = ZoneId.systemDefault();
   
           LocalDateTime localDateTime = LocalDateTime.ofInstant(now, currentZone);
           System.out.println("Local date: " + localDateTime);
   
           ZonedDateTime zonedDateTime = ZonedDateTime.ofInstant(now, currentZone);
           System.out.println("Zoned date: " + zonedDateTime);



Base64
------
    * simple: output mapped to a set of characters, A-Za-z0-9+/, encoder does not add any line
      feed and decoder rejects characters that are not in the set
    * url: output mapped to a set of characters, A-Za-z0-9+&lowbar;, output is URL and filename
      safe
    * MIME: output mapped to MIME friendly format and represented in lines <= 76 characters
      each, and uses carriage return '\r' followed by linefeed '\n' as separator, no line
      separator at the end of encoded output
    * ``Base64.Decoder`` and ``Base64.Encoder`` classes implement encoder and decoder for byte data,
      inheriting methods from `java.lang.Object`
    * ``getDecoder()``: return ``Base64.Decoder`` that decode using basic type decoding scheme
    * ``getEncoder()``: return ``Base64.Encoder`` that encode using basic type encoding scheme
    * ``getMimeDecoder()``: return ``Base64.Decoder`` that decode using MIME type
    * ``getMimeEncoder()``: return ``Base64.Encoder`` that encode using MIME type
    * ``getMimeEncoder(int, byte[])``: return ``Base64.Encoder`` that encode using MIME type with
      given line length and line separator
    * ``getUrlDecoder()``: return ``Base64.Decoder`` that decode using URL and Filename safe type
    * ``getUrlEncoder()``: return ``Base64.Decoder`` that encode using URL and Filename safe type

    .. code-block:: java

       try {
   
           String base64EncodedString = Base64.getEncoder().encodeToString(
                   "HelloWorld".getBytes("utf-8"));
           System.out.println("Encoded with basic: " + base64EncodedString);
   
           byte[] base64DecodedBytes = Base64.getDecoder().decode(base64EncodedString);
           System.out.println("Decoded string: " + new String(base64DecodedBytes));
   
           base64EncodedString = Base64.getUrlEncoder().encodeToString(
                   "HelloWorld".getBytes("utf-8"));
           System.out.println("Encoded with url: " + base64EncodedString);
           StringBuilder stringBuilder = new StringBuilder();
   
           for (int i = 0; i < 10; ++i) {
               stringBuilder.append(UUID.randomUUID().toString());
           }
   
           byte[] mimeBytes = stringBuilder.toString().getBytes("utf-8");
           String mimeEncodedString = Base64.getMimeEncoder().encodeToString(mimeBytes);
           System.out.println("Encoded with MIME: " + mimeEncodedString);
       } catch (UnsupportedEncodingException e) {
           System.out.println(e.getMessage());
       }


`back to top <#java>`_

JVM
===

* `Life Cycle`_, `Availability`_, `Preparation for Threads`_, `JVM Types`_
* base of the entire Java platform's independence from specific hardware and OS, operating at
  the OS layer
* JVM does not know about the specifics of Java programming language
* the class files encapsulate JVM instructions or bytecodes, along with symbol table and
  supplementary information
* bytecode: platform-independent low-level representation of Java code
* any programming language that can be expressed in a valid class file can leverage JVM
* JVM acts as interpreter for Java bytecode, managing memroy, multithreading, and various
  runtime services
* even though Java programming language is platform independent, JVM is platform-specific and
  designed to adapt the host system


Life Cycle
----------
    * one application per JVM instance, ensuring independence and security of each Java
      application
    * **Instance Birth**
        - JVM runtime instance is created when a Java application is launched
        - the instance is responsible for executing bytecode and managing runtime environment
    * **Execution**
        - JVM instance runs the application by invoking ``main()``, which should be public,
          static, return ``void``, and accept ``String[]``
        - any class with proper ``main()`` can be the starting point for a Java application
    * **Application Execution**
        - JVM executes the application, managing necessary resources
    * **Application Completion**
        - JVM instance terminates once the application is executed
    * JVM can use native methods written and C or C++ and dynamically linked to interact and
      leverage platform-specific features
    * native methods provide access to OS and system resources that are not easily accessible
      through pure Java code

Availability
------------
    * concurrent processes ensure JVM continuous availability
    * **Timers**
        - organise periodic events such as interrupts and repetitive processes
        - crucial in maintaining the synchrony of JVM operations
    * **Garbage Collector Processes**
        - ensure efficient memory utilisation
        - manage memory by cleaning up objects that are no longer in use
    * **Compilers**
        - convert bytecode into native code, also known as just-in-time (JIT) compilation
        - enhances the performance of Java applications
    * **Listeners**
        - receive signals and information, and relay it to appropriate processes within JVM

Preparation for Threads
-----------------------
    * **Memory Allocation**
        - JVM allocates memory to the thread, includes dedicated portion of the heap
        - each thread has its own memory space, ensuring isolation
    * **Object Synchronisation**
        - mechanisms, such as locks and monitors, are set up to access shared resources
        - helps prevent data corruption in multi-threaded applications
    * **Specific Registers**
        - specific registers are part of the thread's execution context
        - registers hold data and execution state information
    * **Allocate Native Thread**
        - a native thread managed by the OS is allocated to support Java thread's execution
        - native thread is responsible for executing the Java code
    * JVM is responsible to handle exception, ensure thread safety, and close it if necessary
    * every resources must be released when a thread completes execution

JVM Types
---------
    * **Primitives**
        - JVM do not perform extensive type checking or runtime verification
        - boolean, number, returnAddress
    * **Reference Values**
        - objects, such as complex data structures, are type checked and verified at runtime
        - class, array

`back to top <#java>`_

References & External Resources
===============================

* Santana, Otavio. (2023). Mastering the Java Virtual Machine. Birmingham, UK: Packt Publishing

`back to top <#java>`_
