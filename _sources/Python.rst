======
Python
======

1. `Argparse`_
2. `Subprocess`_
3. `Multiprocessing`_
4. `Socket`_
5. `OOP`_
6. `NumPy`_
7. `PyTorch`_
8. `Tkinter`_
9. `References & External Resources`_

`back to top <#python>`_

Argparse
========

* `Positional Arguments`_, `Optional Arguments`_, `Mutually Exclusive Group`_
* recommended command-line parsing module
* has ``--help`` or ``-h`` option by default

.. code-block:: python

   import argparse
   
   parser = argparse.ArgumentParser()
   # OR
   parser = argparse.ArgumentParser(description="Explains what the program does")


* options given are strings by default
* specify command-line options with ``add_argument()``
* can specify description about what each argument does and it's data type
* can just use ``argparse.ArgumentParser()`` instance for most options

Positional Arguments
--------------------
    * required when calling the program
    * not providing the argument will error

    .. code-block:: python

       import argparse
   
       parser = argparse.ArgumentParser()
   
       # calling program requires to specify option
       parser.add_argument("foo", help="explains what foo does")
   
       args = parser.parse_args() # parse_args() returns data from specified options
   
       print(args.foo) # value of foo is string
   
       # python main.py bar (foo's value is bar)
   
       parser.add_argument("foo", type=int)
       print(args.foo*2) # can do integer operations



Optional Arguments
------------------
    * optional when calling the program
    * optional arguments have value ``None`` by default

    .. code-block:: python

       import argparse
   
       parser = argparse.ArgumentParser()
   
       # argument is optional
       parser.add_argument("--foo")
   
       args = parser.parse_args()
   
       if (args.foo)
           print(args.foo)
   
       # python main.py (no error)
       # python main.py --foo (not providing value will error)
   
       parser.add_argument("-f", "--foo") # can also specify short options for argument


    * with ``action="store_true"``, argument is treated as boolean and no need to specify value
      if used

        .. code-block:: python

           parser.add_argument("--foo", action="store_true") # args.foo is False by default
           # python main.py --foo (args.foo is True)
           # python main.py --foo bar (error if value specified)


    * can limit the values optional argument can accept

        .. code-block:: python

           parser.add_argument("--foo", choices=["bar", "baz"]) # providing other values will error


    * ``action="count"`` will count the number times the argument is provided

        .. code-block:: python

           parser.add_argument("-f", "--foo", action="count")
           args = parser.parse_args()
   
           print(args.foo)
   
           # python main.py -fff (prints 3)
           # python main.py --foo --foo (prints 2)


    * can specify default value with ``default=VALUE``

        .. code-block:: python

           parser.add_argument("--foo", default="bar") # foo's default value is bar
           args = parser.parse_args()
   
           print(args.foo)
   
           # python main.py (prints bar)



Mutually Exclusive Group
------------------------
    * ``parser.add_mutually_exclusive_group()`` allow to specify conflicting options

    .. code-block:: python

       import argparse
   
       parser = argpars.ArgumentParser()
       group = parser.add_mutually_exclusive_group()
       group.add_argument("--foo", action="store_true")
       group.add_argument("--bar", action="store_true")
   
       # python main.py --foo (ok)
       # python main.py --bar (ok)
       # python main.py --foo --bar (error)
       # python main.py --bar --foo (error)


`back to top <#python>`_

Subprocess
==========

* `checkoutput()`_, `shell`_, `PIPE`_
* not available in WebAssemply platforms
* ``run()`` is recommended for most cases
* can use ``Popen()`` interface for advance cases
* returns a ``CompletedProcess`` instance when command completes
* can specify input, capture stdin and stderr, set timeouts, etc.

.. code-block:: python

   import subprocess
   
   subprocess.run(["ls", "-l"]) # output not captured
   subprocess.run(["ls", "-l"], capture_output=True) # set both stdout=PIPE, stderr=PIPE
   
   # cannot set stdout and stderr with capture_output at same time
   subprocess.run(["ls", "-l"], stdout=PIPE, stderr=STDOUT) # combine both streams
   
   # input must be byte sequence or string if encoding provided or text is True
   subprocess.run(["sudo", "ls", "-l"], input="PASSWORD", text=True)
   
   # save output to variable
   output = subprocess.run(["ls", "-l"], stdout=PIPE)



checkoutput()
-------------
    * command returns output in bytes, decoding is required
    * raises ``CalledProcessError`` for non-zero return code
    * command is same as ``run(..., check=True, stdout=PIPE).stdout``
    * ``input=None`` will be same as ``input=b''``

    .. code-block:: python

       output = subprocess.check_output(["ls"])
       print(output) # in bytes
       print(output.decode("utf-8")) # in string



shell
-----
    * when ``shell=True``, command is executed through the shell
    * can access other shell features such as shell pipes, filename wildcard, environment
      variable expansions, etc.
    * **For security**
        - must ensure whitespace and metacharacters are quoted properly
        - can be vulnerable to shell injection

    .. code-block:: python

       subprocess.checkoutput("dmesg | grep hda", shell=True) # can use shell pipe feature



PIPE
----
    * instead of using ``shell=True`` to use shell pipe feature, use ``stdout`` and ``stdin`` to pass
      output between commands

    .. code-block:: python

       p1 = Popen(["dmesg"], stdout=PIPE)
       p2 = Popen(["grep", "hda"], stdin=p1.stdout, stdout=PIPE) # use p1's stdout as input
       p1.stdout.close()
       output = p2.communicate()[0] # output in bytes


`back to top <#python>`_

Multiprocessing
===============

* `Process`_, `Process Synchronization`_, `Communication`_, `Sharing State`_, `Worker Pool`_
* not available in WebAssemply platforms
* supports local and remote concurrency by using subprocesses instead of threads
* the package mostly replicates API of ``threading`` module

Process
-------
    * object created to spawn a process, multiple ``Process`` objects for multiple processes
    * ``start()`` is called after creating the object

    .. code-block:: python

       from multiprocessing import Process
   
       def f(x):
           print(x * x)
   
       if __name__ == "__main__":
           p1 = Process(target=f, args=(2,))
           p2 = Process(target=f, args=(2,))
   
           p1.start()
           p2.start()
   
           p1.join() # wait for p1 to finish
           p2.join() # wait for p2 to finish


    * three ways to start a process depending on the platform: spawn, fort, forkserver
    * ``spawn``
        - parent process starts fresh Python interpreter process
        - child only inherit necessary resources to run object's ``run()``
        - this method is slower than others
        - available on POSIX and Windows
        - default on Windows and macOS
    * ``fork``
        - parent uses ``os.fork()`` to fork Python interpreter
        - child process is identical to parent, all resources inherited
        - safely forking a multithreaded process is problematic
        - available on POSIX
        - default on POSIX except macOS
    * ``forkserver``
        - spawn server process, single threaded unless side-effects spawn threads
        - parent process request the server to fork a new process if needed
        - no unnecessary resources are inherited
        - available on POSX that support passing file descriptors over Unix pipes

    .. code-block:: python

       import multiprocessing as mp
   
       if __name__ == "__main__":
           mp.set_start_method('spawn') # should not be used more than once



Process Synchronization
-----------------------
    * can use lock to ensure only one process access resource at a time

    .. code-block:: python

       from multiprocessing import Process, Lock
   
       def f(lock, i):
           lock.acquire()
           try:
               print(i)
           finally:
               lock.release()
   
       if __name__ == "__main__":
           lock = Lock()
           for num in range(10):
               Process(target=f, args=(lock, num)).start()



Communication
-------------
    * **Queue**
        - near clone of ``queue.Queue``
        - thread and process safe

        .. code-block:: python

           from multiprocessing import Process, Queue
   
           def f(q):
               q.put("hello")
   
           if __name__ = "__main__":
               q = Queue()
               p = Process(target=f, args=(q,))
               p.start()
               print(q.get()) # print "hello"
               p.join()


    * **Pipe**
        - returns a pair of connection objects connected by duplex pipe
        - two connection objects returned represent two ends of the pipe
        - each connection object has ``send()``, ``recv()`` and other methods
        - data in pipe can be corrupted if two processes try to read or write to same pipe end
          at same time

        .. code-block:: python

           from multiprocessing import Process, Pipe
   
           def f(conn):
               conn.send("hello")
               conn.close()
   
           if __name__ = "__main__":
               parent_conn, child_conn = Pipe()
               p = Process(target=f, args=(child_conn,))
               p.start()
               print(parent_conn.recv()) # print "hello"
               p.join()



Sharing State
-------------
    * avoid shared state in concurrent programming if possible
    * **Share Memory**
        - can store data in a shared memory map using ``Value`` or ``Array``
        - shared objects are process and thread-safe
        - can also use ``multiprocessing.sharedctypes`` module

        .. code-block:: python

           from multiprocessing import Process, Value, Array
   
           def f(n, a):
               n.value = 2.0
               for i in range(len(a)):
                   a[i] = -a[i]
   
           if __name__ == "__main__":
               # 'd' and 'i' are typecodes
               num = Value('d', 0.0) # float
               arr = Array('i', range(10)) # integer
   
               p = Process(target=f, args=(num, arr))
               p.start()
               p.join()
   
               print(num.value)
               print(arr[:])


    * **Manager Object**
        - returned by ``Manager()`` and controls a server process
        - holds Python objects and allows other processes to manipulate them using proxies
        - support types list, dict, Namespace, Lock, RLock, Semaphore, BoundedSemaphore,
          Condition, Event, Barrier, Queue, Value and Array

        .. code-block:: python

           from multiprocessing import Process, Manager
   
           def f(d, l):
               d[1] = '1'
               d['2'] = 2
               l.reverse()
   
           if __name__ == "__main__":
               with Manager() as manager:
                   d = manager.dict()
                   l = manager.list(range(10))
   
                   p = Process(target=f, args=(d, l))
                   p.start()
                   p.join()



Worker Pool
-----------
    * ``Pool``, object to parallelize execution of function across multiple input values
    * distribute the input data across processes, data parallelism
    * has methods to offload tasks to the worker processes in different ways
    * methods of pool should only be used by the process which created it
    * require ``__main__`` module be importable by the children and some will not work in
      interactive interpreter

    .. code-block:: python

       from multiprocessing import Pool
   
       def f(x):
           return x * x
   
       if __name__ == "__main__":
           x = [1, 2, 3]
           with Pool(processes=5) as p:  # 5 worker processes
               print(p.map(f, x))  # [1, 4, 9]
   
               # print in arbitary order
               for i in p.imap_unordered(f, x):
                   print(i)
   
               # f(3) in async
               res = p.apply_async(f, (3,))  # runs in only one process
               print(res.get(timeout=1))  # 9
   
               res = p.apply_async(os.getpid, ())  # runs in only one process
               print(res.get(timeout=1))  # PID of the process
   
               # multiple aysnc may use more processes
               multiple = [p.apply_async(os.getpid, ()) for i in range(4)]
               print([res.get(timeout=1) for res in multiple])
   
               res = p.apply_async(time.sleep, (10,))
               try:
                   print(res.get(timeout=1))  # will get TimeoutError
               except TimeoutError:
                   print("multiprocessing.TimeoutError")
   
           print("Pool is closed now")


`back to top <#python>`_

Socket
======

* `Server`_, `Client`_
* provide access to BSD socket interface, which is available on most platforms
* not available in WebAssemply platforms

Server
------
    * usual workflow is socket->bind->listen->accept
    * ``socket.accept()``
        - return a pair (client_socket, client_address)
        - client_socket can be used to send and receive data
        - client_address is the address bound to the socket on the other end

    .. code-block:: python

       import socket
   
   
       def main():
           BACKLOG = 10
           HOST = "localhost"
           PORT = 8080
           # address family: a pair of (host, port) for AF_INET
           addr = (HOST, PORT)
   
           server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   
           # set socket to be reusable
           server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
   
           server_socket.bind(addr)
   
           server_socket.listen(BACKLOG)
           print(f"Server listening on port {PORT}...")
   
           while True:
               (client_socket, client_addr) = server_socket.accept()
               print(f"Got connection from {client_addr}")
               buf = client_socket.recv(1024)
               if len(buf) > 0:
                   print(
                       f"Client {client_socket.fileno()} send: {buf.decode('utf-8')}")
               break
   
   
       if __name__ == "__main__":
           main()



Client
------
    * usual workflow is socket->connect->send

    .. code-block:: python

       import socket
   
   
       def main():
           HOST = "localhost"
           PORT = 8080
           # address family: a pair of (host, port) for AF_INET
           addr = (HOST, PORT)
   
           client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   
           client_socket.connect(addr)
           print(f"Connected to {HOST} on port {PORT}")
   
           client_socket.send(b"hello")
   
   
       if __name__ == "__main__":
           main()



`back to top <#python>`_

OOP
===

* `ABCs`_, `Protocols`_

ABCs
----
    * give more structure to types, and type hints do not need updates for new subclasses
    * can be difficult to combine classes from other libraries, and virtual subclasses need
      explicit registering

    .. code-block:: python

       from abc import ABC, abstractmethod
   
       class Animal(ABC):
           @abstractmethod
           def walk(self):
               pass
   
       class Duck(Animal):
           def walk(self):
               pass
   
       assert isinstance(Duck(), Animal)  # True



Protocols
---------
    * mainly designed to be used when type checking, also called structural subtyping or static
      duck typing
    * do not need to inherit or register, and easier than ABCs when combining libraries
    * need to decorate the protocol to make it runtime-checkable
    * **``runtime_checkable``**
        - any object that adheres to the protocol becomes an instance of it at runtime
        - only checks the existence of protocol members, and names, but not signatures

    .. code-block:: python

       from typing import Protocol, runtime_checkable
   
       @runtime_checkable
       class Animal(Protocol):
           def walk(self):
               pass
   
       # implicitly considered to be a subtype of Animal
       class Duck():
           def walk(self):
               pass
   
       assert isinstance(Duck(), Animal)  # True, but TypeError without runtime_checkable


`back to top <#python>`_

NumPy
=====

* `NumPy Data Types`_, `Vectorization`_, `Broadcasting`_, `ndarray`_, `Strides`_
* `NumPy Arrays`_, `NumPy Random`_, `NumPy UFunc`_, `NumPy Source Code`_
* fundamental package for scientific computing
* provides multidimensional array object and routines for fast operations on arrays
* supports object-oriented approach

NumPy Data Types
----------------
    * **Scalar Types**
        - e.g. ``np.float64``, ``np.int32``
        - used to build data types, which are attached to NumPy arrays

        .. code-block:: python

           d = np.dtype(np.float32).newbyteorder('>')


    * all NumPy arrays have the same type of ndarray
    * **Array Scalars**
        - array scalar is a bridge between scalar numbers and NumPy arrays
        - each array scalar has its own type and an attached ``dtype``, e.g. x[0]

Vectorization
-------------
    * absence of any explicit looping in the code, but operates in optimized, pre-compiled C
      code
    * vectorized code is more concise and easier to read, more Pythonic code
    * fewer lines and fewer bugs, closer to standard mathematical notation

Broadcasting
------------
    * implicit element-by-element behaviour of operations
    * allows to combine arrays of different shapes sensibly
    * in NumPy, all operations broadcast
    * when combining two arrays of different shapes, shapes are matched from right to left
        - match when dimensions are equal, and one dimension is either None or 1
        - (5, 10) + (3, 5, 10) = (3, 5, 10)
        - (5, 10) + (6, 10) = cannot combine
        - (5, 10, 1) + (10, 5) = (5, 10, 5)

        .. code-block:: python

           x = np.zeros((3, 5))
           y = np.zeros((8,))
           print(x * y)    # error, cannot broadcast
   
           # change x to match y
           x = x.reshape((3, 5, 1))
           x = x[..., np.newaxis] # take all dimensions and add new axis at the end, same as above
           x = x[:, :, np.newaxis] # same as above
           print(x * y)    # (3, 5, 1) * (8) = (3, 5, 8)
   
           # or change y to match x
           y = y.reshape((8, 1, 1))
           y = y[..., np.newaxis, np.newaxis]
           y = y[:, np.newaxis, np.newaxis]
           print(x * y)    # (3, 5) * (8, 1, 1) = (8, 3, 5)


    * best to avoid ``:`` and ``...`` in broadcasting, as output shape is sometimes hard to predict
    * can also use broadcasting inside of indexing
        - takes the indexing arrays and broadcast them against one another
        - the result matrix contains coordinates that are used to index the NumPy array

        .. code-block:: python

           x = np.array([[1, 2], [3, 4]])
           ix0 = np.array([0, 0, 1, 1])
           ix1 = np.array([[1], [0]])
           coordinates = np.broadcast_arrays(ix0, ix1) # (1, 4) + (2, 1) = (2, 4), used to index x
           x = x[ix0, ix1] # x becomes (2, 4)



ndarray
-------
    * n-dimensional arrays of homogeneous data types
    * fixed size at creation, changing the size will create a new array and delete the original
    * exception: can have arrays of objects, thus allowing different sized elements
    * efficient mathematical operations, in compiled code, on large numbers of data
    * ``numpy.array`` is not same as Standard Python Library class ``array.array``
    * element-by-element operations are default mode

        .. code-block:: python

           # a, b, c are Python lists
           for i in range(len(a)):
               c.append(a[i] * b[i])   # inefficient when large
   
           # a, b, c are ndarray
           # vectorization and broadcasting
           c = a * b   # same as above, but at near-C speed


    * dimensions are called axes

        .. code-block:: python

           # one axis and length of 3
           [1, 2, 1]
   
           # 2 axes, first axis has length 2 and second axis has length 3
           [[1, 0, 0],
            [0, 1, 2]]


    * ``ndarray.ndim``: number of axes/dimensions of the array
    * ``ndarray.shape``: tuple of integers with size of the array in each dimension, e.g. shape
      n x m matrix is (n, m), length of the `shape` tuple is the number of axes, `ndim`
    * ``ndarray.size``: total number of elements, equal to the product of the elements of ``shape``
    * ``ndarray.dtype``: object describing type of elements, can specify ``dtype`` using Python types
      or NumPy types, e.g. `numpy.int32`, `numpy.int16`, `numpy.float64`
    * ``ndarray.itemsize``: size in bytes of each element, equal to ``ndarray.dtype.itemsize``,
      e.g. `float64` has `itemsize` of 8 bytes
    * ``ndarray.data``: buffer containing the actual elements, do not need to use normally

    .. code-block:: python

       import numpy as np
   
       a = np.arange(15).reshape(3, 5)
       print(a)
       print(a.shape)
       print(a.ndim)
       print(a.dtype.name)
       print(a.itemsize)
       print(a.size)
       print(type(a))



Strides
-------
    * data pointer: shows where in memory the data is stored
    * stride tells how many bytes to skip in memory to move forward in any single dimension of
      the array
    * e.g. strid(6, 2): need to skip 6 bytes to get to next row, and 2 bytes to the next column
    * strides allow NumPy to do operations without copying data
    * by only flipping the strides, the array can be transposed

        .. code-block:: python

           # dtype:uint8, stride(3, 1)
           arr = [[0, 1, 2],
                  [3, 4, 5],
                  [6, 7, 8]
                  ]
   
           # changing stride to (1, 3)
           arr = [[0, 3, 6],
                  [1, 4, 7],
                  [2, 5, 8]
                  ]



NumPy Arrays
------------
    * NumPy arrays are printed in similar way to nested lists
    * the last axis is printed from left to right
    * the second-to-last is printed from top to bottom
    * the rest are printed from top to bottom, with each slice separated from the next by
      an empty line
    * one-dimensional arrays are printed as rows, bi-dimensional as matrix and
      tri-dimensional as lists of matrices
    * if array is too large, central part is skipped, and only corners are printed
    * use ``np.set_printoptions(threshold=sys.maxsize)`` to print the entire array
    * **numpy.array()**
        - function to create array form python list or tuple
        - can transform sequences of sequences into 2-dimensional arrays, and so on
        - can specify type of array at creation time

        .. code-block:: python

           a = np.array([(1, 2, 3), (4, 5, 6)])
           b = np.array([(1, 2), (3, 4)], dtype=complex)


    * has several functions to create arrays with initial placeholder content to minimize the
      necessity of growing arrays, `dtype` is `float64` by default
    * **numpy.zeros()**
        - creates an array full of zeros
        - ``numpy.zeros_like``: return an array of zeros with same shape and type as given array

        .. code-block:: python

           a = np.zeros((2, 3))    # dtype float64
   
           b = np.zeros_like(a)    # b has shape of (2, 3)


    * **numpy.ones()**
        - creates an array full of ones
        - ``numpy.ones_like``: return an array of ones with same shape and type as given array

        .. code-block:: python

           a = np.ones((2, 3))    # dtype float64
   
           b = np.ones_like(a)    # b has shape of (2, 3)


    * **numpy.empty()**
        - creates an array with random initial content
        - depends on the state of memory
        - ``numpy.empty_like``: return an array with same shape and type as given array

        .. code-block:: python

           a = np.empty((2, 3), dtype=int)
   
           b = np.empty_like(a)    # b has shape of (2, 3)


    * **numpy.arange()**
        - analogous to Python ``range``, but returns an array
        - accepts float arguments, but number of elements obtained is unpredictable due to the
          finite floating point precision

        .. code-block:: python

           a = np.arange(10, 30, 5) # [10, 15, 20, 25]
           b = np.arange(0, 2, 0.6) # [0., 0.6, 1.2, 1.8]


    * **numpy.linspace()**
        - return evenly spaced numbers over a specified interval
        - better to use than ``numpy.arange`` with float arguments

        .. code-block:: python

           a = np.linspace(0, 2, 9)    # 9 numbers from 0 to 2
           b = np.linspace(0, 2 * np.pi, 100)  # useful to evaluate function at lots of points
           c = np.sin(b)


    * **numpy.fromfunction()**
        - create array by executing a function over each coordinate
        - array has a value fn(x, y, z) at coordinate (x, y, z)

        .. code-block:: python

           a = np.fromfunction(lambda i, j: i + j, shape=(2, 3))
           # [[0. 1. 2.]
           #  [1. 2. 3.]]


    * **numpy.fromfile()**
        - create array from data in a text or binary file

        .. code-block:: python

           import numpy as np
           import tempfile
   
           # Define a structured data type with nested structure for 'time' and a float for 'temp'
           dt = np.dtype([('time', [('min', np.int64), ('sec', np.int64)]), ('temp', float)])
   
           # Create a NumPy array of shape (1,) with the defined structured data type
           x = np.zeros((1,), dtype=dt)
   
           # Set values for the fields in the structured array
           x['time']['min'] = 10
           x['temp'] = 98.25
   
           fname = tempfile.mkstemp()[1]   # Create a temporary file and get the file path
           x.tofile(fname) # Write the structured array to the temporary file
   
           a = np.fromfile(fname, dtype=dt)
   
           # recommended way
           np.save(fname, x)   # save an array to a binary file in NumPy `.npy` format
           a = np.load(fname + ".npy")



NumPy Random
------------
    * ``numpy.random`` implements pseudo-random number generators
    * only designed for statistical modeling and simulation, not suitable for security or
      cryptographic purposes
    * create a generator with ``default_rng()`` and call various methods to get samples from
      different distributions
    * **Seeds**
        - with no seed provided, ``default_rng()`` will seed from non-deterministic data from OS
          and generate different numbers each time
        - seeds should be large positive integers of any size, use ``secrets.randbits()``

        .. code-block:: python

           import secrets
           rng = np.random.default_rng(secrets.randbits(128))
           num = rng.random()


    * **numpy.random.Generator.random**
        - return random floats between [0.0, 1.0)
        - has ``size`` parameter that accepts int or tuple of ints, (m * n * k) samples are
          drawn for (m, n, k) shape

        .. code-block:: python

           rng = np.random.default_rng()
           num = rng.random()
           arr1 = rng.random((5,))
           arr2 = rng.random((3, 2))


    * **numpy.random.Generator.normal**
        - draw random samples from normal distribution
        - ``loc`` (mean/centre of distribution), and ``scale`` (stand deviation, spread/width)
          parameters accept float or ``array_like`` of floats

        .. code-block:: python

           mu, sigma = 0, 0.1  # mean and standard deviation
           rng = np.random.default_rng()
           arr1 = rng.normal(mu, sigma, size=(1000,))
           arr2 = rng.normal(3, 2.5, size=(2, 4))



NumPy UFunc
-----------
    * vectorized functions that takes a fixed number of scalar inputs and produces a fixed
      number of scalar outputs
    * supports array broadcasting, type casting, and other standard features
    * **Output Type**
        - determined by input class with highest ``__array_priority__`` or by ``output`` parameter
        - ``__array_prepare__``: called before ufunc, provided context about the ufunc, pass the
          array to the ufunc after prepare
        - ``__array_wrap__``: called after execution of ufunc

    * can check type handling, e.g. ``np.add.types``
    * defined in ``_core/include/numpy/ufuncobject.h``

    .. code-block:: python

       def range_sum(a, b):
           return np.arange(a, b).sum()
   
       # frompyfunc() takes any Python function and turns it into ufunc
       rs = np.frompyfunc(range_sum, 2, 1)  # 2 inputs, 1 output
       x = np.array([[1, 2, 3, 4]])
       y = rs(x, x.T)
       print(y)



NumPy Source Code
-----------------
    * **numpy/\_core**
        - contains most of the C code base
        - code for multi-array, ufunc extensions
        - various support libraries such as npymath, npysort
        - public headers in include

    * numpy/lib: various tools on top of core
    * Python interface is pretty straightforward

    * **npymath**
        - C99 abstraction for cross platform math operations
        - implement fundamental IEEE 754-related features
        - half float implementation, C99 layer for functions, macros and constant definitions
    * **PyArrayObject**
        - every NumPy array has a corresponding ``PyArrayObject``
        - defined in ``numpy/ndarraytypes.h``
    * **PyArray_Descr**
        - contains instance-specific data of ``dtype``
        - one ``dtype`` object -> one ``PyArray_Descr`` instance
        - ``PyArrayDescr_Type``L extension type (singleton) which defines the ``dtype`` class
    * **PyArray\_Type**
        - of ``PyTypeObject``, which is a C-api to define new type extension
        - extension type (singleton) which defines the array behaviour
        - contains most of Python and C layering
        - understanding the data structure will help know which function will be called on the
          NumPy array
        - defined in ``multiarray/arrayobject.c``

`back to top <#python>`_

PyTorch
=======

* `Tensors`_, `PyTorch Basic Functions`_, `PyTorch Dataset`_, `nn Module`_, `Matrix Dot Product`_

Tensors
-------
    * specialised data structure similar to arrays and matrices, and NumPy's ndarrays
    * used to encode inputs, outputs, and parameters of a model
    * can run on GPU or other hardware accelerators, optimised for automatic differentiation
    * shape of the tensor, a tuple, determines the dimensionality
    * tensors are created on CPU by default, need to explicitly move tensors to GPU
    * copying large tensors across devices can be expensive

    .. code-block:: python

       data = [[1, 2], [3, 4]]
   
       t1 = torch.tensor(data)  # create tensor directly, auto infer datatype
   
       np_arr = np.array(data)
       t2 = torch.tensor(np_arr)  # create  tensor from ndarray
   
       # create tensor from another tensor
       t3 = torch.ones_like(t1)  # retain properties of argument tensor
   
       t4 = torch.rand_like(t1, dtype=torch.float)  # override datatype
   
       shape = (2, 3,)
       t5 = torch.ones(shape)
   
       if torch.cuda.is_available():
           t5 = tensor.to('cuda')


    * **Tensor Attributes**
        - describe the shape, datatype, and the device on which tensors are stored

        .. code-block:: python

           t = torch.ones((2, 3,))
           print(t.shape)  # (2, 3)
           print(t.dtype)  # float32
           print(t.device)  # cpu


    * **Tensor Operations**
        - over 100 operations available, each can be run on GPU
        - ``tensor[:, -1]``:  select last element along the second dimension
        - ``tensor[..., -1]``: ellipsis, to select last element along the last dimension, more
          flexible with higher-dimensional tensors
        - in matrix multiplication, ``t1 @ t2 != t2 @ t1``
        - can convert single-element tensor to Python numerical value
        - in-place operations: stores the result into the operand, denoted by _ suffix, save
          memory, but can be problematic when computing derivatives because of an immediate
          loss of history

        .. code-block:: python

           t1 = torch.ones((4, 4,))
           print(t1[0])  # first row
           print(t1[:, 0])  # first column
           print(t1[..., -1])  # first column
           t1[:, 1] = 0  # change values on second column to 0
           print(t1)
   
           t2 = torch.rand((4, 4))
           t3 = torch.cat([t1, t2], dim=1)  # concat t1 and t2 along 1st dimension
           print(t3)
   
           t4 = t1 @ t2  # matrix multiplication
           t4 = t1.matmul(t2)  # same as above
           print(t4)
   
           t5 = t1 * t2  # element-wise product
           t5 = t1.mul(t2)  # same as above
           print(t5)
   
           t6 = t1.sum()  # aggregate all values of tensor
           t6_item = t6.item()
           print(t6_item)
   
           t1.add_(5)  # changes t1
           print(t1)


    * tensors on CPU and NumPy arrays can share their underlying memory locations, changing one
      will change the other

        .. code-block:: python

           t1 = torch.ones(5)
           n1 = t1.numpy()  # convert tensor to NumPy array
           t1.add_(3)  # changes both t1 and n1
           print(n1)
   
           n2 = np.ones(5)
           t2 = torch.from_numpy(n2)  # convert NumPy array to tensor
           np.add(n2, 2, out=n2)  # changes both n2 and t2
           print(t2)



PyTorch Basic Functions
-----------------------
    * ``arange(start=0, end, step=1)``: return 1-D tensor of size (end-start)/step with values
      from [start, end)
    * ``cat(tensors, dim=0)``: concat tensors in given dimension, all tensors must be same shape
      or 1-D empty tensor with size 0
    * ``empty(size)``: return tensor filled with uninitialized data, size can be variable number
      of arguments or collection
    * ``empty_like(input)``: return uninitialized tensor with same size as input Tensor, same as
      ``empty(input.size(), dtype=input.dtype, layout=input.layout, device=input.device)``
    * ``exp(input)``: return tensor with exponential of elements of input tensor
    * ``eye(n)``: return 2-D tensor with ones on the diagonal and zeros elsewhere
    * ``linspace(start, end, steps)``: create 1-D tensor of size steps with values evenly spaced
      from start to end, [$start, start + \frac{end-start}{steps-1}, ..., end$]
    * ``logspace(start, end, steps, base=10.0)``: create 1-D tensor of size steps with values
      evenly spaced from $base^{start}$ to $base^{end}$,
      [$base^{start}, base^{(start+\frac{end-start}{steps-1})}, ..., base^{end}$]
    * ``masked_fill(mask, value)``: fills elements of self tensor with value where mask is True,
      out-of-place version, mask is boolean tensor
    * ``multinomial(input, num_samples)``: return a tensor where each row contains num_samples
      indices sampled from the multinomial, input is a tensor of probabilities
    * ``ones(size)``: return tensor filled with scalar 1, size can be variable number of
      arguments or collection
    * ``randint(low=0, high, size)``: return tensor with random ints uniformly between [low, high)
    * ``stack(tensors)``: concat tensors along a new dimension
    * ``tensor(data)``: create tensor with no autograd history by copying data, data can be a list,
      tuple or NumPy ndarray, scalar and other types
    * ``transpose(input, dim0, dim1)``: return transposed version of input tensor, dim0 and dim1
      are swapped
    * ``tril(input, diagonal=0)``: return lower triangular part, elements on and below diagonal,
      of 2-D tensor or batch of matrices
    * ``triu(input, diagonal=0)``: return upper triangular part, elements on and above diagonal,
      of 2-D tensor or batch of matrices
    * ``zeros(size)``: return tensor filled with scalar 0, size can be variable number of
      arguments or collection

PyTorch Dataset
---------------
    * decouple dataset code from model training code for readability and modularity
    * PyTorch provides two data primitives to use pre-loaded and own datasets
    * has functions specific to particular data that can be used to prototype and benchmark
      models
    * ``utils.data.Dataset``: stores samples and labels, retrieves one sample at a time
    * ``utils.data.DataLoader``: wraps an iterable around ``Dataset``, can reshuffle data at every
      epoch to reduce model overfitting

    .. code-block:: python

       from torch.utils.data import DataLoader
       from torchvision import datasets
       from torchvision.transforms import ToTensor
   
       train_data = datasets.FashionMNIST(
           root='data',  # path of stored data
           train=True,  # training or test dataset
           download=True,  # download if not available at root
           transform=ToTensor()  # feature and label transformation
       )
   
       test_data = datasets.FashionMNIST(
           root='data',
           train=False,
           download=True,
           transform=ToTensor()
       )
   
       train_dataloader = DataLoader(train_data, batch_size=64, shuffle=True)
       test_dataloader = DataLoader(test_data, batch_size=64, shuffle=True)
       train_features, train_labels = next(iter(train_dataloader))
       print(f"Feature batch shape: {train_features.shape}")
       print(f"Labels batch shape: {train_labels.shape}")
       img = train_features[0].squeeze()
       label = train_labels[0]
       print(f"Image: {img}")
       print(f"Label: {label}")


    * **Image Datasets**
    * **Text Datasets**
    * **Audio Datasets**
    * **Custom Datasets**
        - must implement ``__init__()``, ``__len__()``, and ``__getitem__()``

        .. code-block:: python

           from torchvision.io import read_image
           import os
           import pandas as pd
   
           class CustomImageDataset(Dataset):
               def __init__(self, annotations_file, img_dir, transform=None,
                            target_transform=None):
                   self.img_labels = pd.read_csv(annotations_file)
                   self.img_dir = img_dir
                   self.transform = transform
                   self.target_transform = target_transform
   
               def __len__(self):
                   return len(self.img_labels)
   
               def __getitem__(self, idx):
                   img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
                   image = read_image(img_path)
                   label = self.img_labels.iloc[idx, 1]
                   if self.transform:
                       image = self.transform(image)
                   if self.target_transform:
                       image = self.target_transform(label)
                   return image, label



nn Module
---------
    * module to train and build neural network layers such as input, hidden, and output
    * **Softmax**
        - $Softmax(x_i) = \frac{exp(x_i)}{\Sigma_jexp(x_j)}$
    * ``Linear(in_features, out_features, bias=True)``: apply linear transformation to incoming
      data
    * ``functional.softmax(input, dim=None)``: apply softmax function to all slices along dim,
      and will rescale so that elements lie in the range [0, 1] and sum to 1
    * **model.train()**
        - model learns from the data, and weights and biases are updated as training goes on
        - some layers, such as dropout and batch normalisation operate differently
        - ``dropout``: active during training, dropout random neurons not to overfit, helps the
          model learn better with noise present
    * **model.eval()**
        - use the entire network to see how well it performs
        - some layers, such as dropout and batch normalisation operate differently
        - ``dropout``: inactive during evaluation
    * **functional.relu()**
        - applies element-wise ReLU, [0, $\inf$]
        - if number is <= 0, returns 0, return same number otherwise
        - offers non-linearity to linear network
    * **functional.sigmoid()**
        - applies element-wise sigmoid, (0, 1)
        - $Sigmoid(x) = \frac{1}{1 + exp(-x)}$
    * **functional.tanh()**
        - applies element-wise tanh, (-1, 1)
        - $tanh(x) = \frac{exp(x)-exp(-x)}{exp(x)+exp(-x)}$
    * **nn.Sequential()**
        - one block depends on another to complete synchronously
    * **nn.ModuleList()**
        - does not run sequentially
        - each layer or head is isolated and gets its own unique perspective
        - parallelism in a model isn't due to ``ModuleList()``, instead the computations are
          structured to take advantage of the GPU

Matrix Dot Product
------------------
    * number of rows of first matrix must be equal to the number of columns of the second

        .. code-block:: python

           a = torch.tensor([[1, 2],
                             [3, 4],
                             [5, 6]])
           print(a.shape)  # (3, 2)
           b = torch.tensor([[7, 8, 9],
                             [10, 11, 12]])
           print(b.shape)  # (2, 3)
   
           c = a @ b
           print(c.shape)  # (3, 3)
   
           c = torch.matmul(b, a) # same as using @
           print(c.shape)  # (2, 2)


    * cannot multiply tensors of different ``dtype``

        .. code-block:: python

           x = torch.randint(1, (3, 2))
           y = torch.rand(2, 3)
           print(x @ y)    # error
   
           x = torch.randint(1, (3, 2)).float()
           print(x @ y)    # ok


`back to top <#python>`_

Tkinter
=======

* `Frame`_, `Widgets`_, `Event Loop`_, `Example Tkinter Code`_
* works cross-platform
* do not need to implement redrawing, parsing and dispatching events, hit detection or handling
  events on each widget
* no complex code for widgets, only need to attach them to variables
* always encapsulate the main code rather than putting into global variable space

Frame
-----
    * main application window is not part of newer themed widgets, and its background color
      doesn't match the themed widgets
    * using a themed frame widget to hold content ensures that the background is correct

    .. code-block:: python

       mainframe = ttk.Frame(root, padding="3 3 12 12")
       # place the frame inside main application window
       mainframe.grid(column=0, row=0, sticky=(N, W, E, S))
       # expand frame to fill extra space when window resizes
       root.columnconfigure(0, weight=1)
       root.rowconfigure(0, weight=1)



Widgets
-------
    * need to specify a parent when creating widget
    * parent is passed as the first parameter when instantiating a widget object
    * can provide options such as ``width`` and ``textvariable``, which must be instance of
      ``StringVar`` class
    * widgets do not auto appear on screen, must be placed in appropriate column and row
    * ``sticky`` describes how the widget should line up within the grid cell, using compass
      directions

    .. code-block:: python

       username = StringVar()
       username_entry = ttk.Entry(mainframe, width=7, textvariable=username)
       username_entry.grid(column=2, row=1, sticky=(W, E))
       ttk.Label(mainframe, text="Username").grid(column=1, row=1, sticky=W)
       ttk.Button(mainframe, text="Register", command=register).grid(
           column=2, row=8, sticky=W)



Event Loop
----------
    * necessary for everything to appear onscreen

    .. code-block:: python

       root.mainloop()



Example Tkinter Code
--------------------

    .. code-block:: python

       from tkinter import *
       from tkinter import ttk
   
   
       class HelloWorld:
           def __init__(self, root: Tk):
               root.title("Hello World")
   
               mainframe = ttk.Frame(root, padding="3 3 12 12")
               # place the frame inside main application window
               mainframe.grid(column=0, row=0, sticky=(N, W, E, S))
               # expand frame to fill extra space when window resizes
               root.columnconfigure(0, weight=1)
               root.rowconfigure(0, weight=1)
   
               ttk.Label(mainframe, text="Hello").grid(column=1, row=1, sticky=(W, E))
   
               self.world = StringVar()
               ttk.Label(mainframe, textvariable=self.world).grid(
                   column=2, row=1, sticky=(W, E))
   
               ttk.Button(mainframe, text="Say world", command=self.hello_world).grid(
                   column=1, row=2, sticky=W)
   
               for child in mainframe.winfo_children():
                   child.grid_configure(padx=5, pady=5)
   
           def hello_world(self):
               self.world.set("world")
   
   
       root = Tk()
       HelloWorld(root)
       root.mainloop()


`back to top <#python>`_

References & External Resources
===============================

* EuroPython Conference. (2022). Protocols in Python: Why You Need Them - presented by Rogier
  van der Geer. Available at: https://youtu.be/Lddegb2ToNY?si=xYzoKf0NNlDVozzS
* EuroPython Conference. (2022). What happens when you import a module? - presented by Reuven
  M. Lerner. Available at: https://youtu.be/Aty6rJIvpfU?si=AnpdXpURoVXYx-EP

`back to top <#python>`_
