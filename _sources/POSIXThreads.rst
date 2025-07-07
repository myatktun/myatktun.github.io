=============
POSIX Threads
=============

1. `Basics`_
2. `Thread Life Cycle`_
3. `Mutex`_
4. `Condition Variables`_
5. `Deadlock`_
6. `Pthreads API`_
7. `References & External Resources`_

`back to top <#posix-threads>`_

Basics
======

* `Threads`_, `Concurrency`_, `Parallelism`_, `Thread Safety`_, `Asynchrony`_
* `Thread Evaporation`_, `Invariants`_, `Critical Sections`_
* Uniprocessor: single execution unit, even if it has vector processors or I/O coprocessors
* Multiprocessor: more than one processor sharing instruction set and physical memory

Threads
-------
    * can be thought as a lightweight process, smaller and faster than traditional process
    * can be used to utilise resources more efficiently on multiprocessors, asynchronously
    * asynchronous: two operations proceeding independently of each other, but concurrently
    * mutex: primary synchronisation object
    * condition variables: used to signal or broadcast to indicate shared data state changes
    * no threads per process limit, but a limit on total number of processes on the system
    * threads force to think about and plan for synchronisation requirements
    * threaded programming model isolates loosely couple functional execution streams, with
      each function having explicit synchronisation for dependencies
    * threads execute independently, but share address space and file descriptors
    * each thread can read and write variables in shared memory
    * data race: threads simultaneously read and write the same data
    * **Cons**
        - can be less efficient by using synchronisation too much
        - threads writing to the same memory locations may spend more time synchronising the
          memory on processors with read/write ordering
        - adding threads to remove a bottleneck can give rise to another
        - performance decrease when using too many compute-bound threads
        - a bug might not occur if one thread runs slower than another due to debugger trap
    * **When to use**
        - for code parallelizable into multiple threads, and written for multiprocessor
        - to perform I/O which can be overlapped, e.g. distributed server applications
        - one thread manager can control, and divide work among worker threads

Concurrency
-----------
    * describes the behaviour of threads or processes on uniprocessor system
    * appear to happen at the same time, but may occur serially
    * does not imply that tasks proceed simultaneously, but rather correctly executed
      alternatively
    * allows a program to take advantage of asynchrony, progressing even when certain
      tasks are blocked
    * e.g. during waiting for I/O, the system can switch to another task
    * can be achieved on both uniprocessor and multiprocessor systems
    * execution context, scheduling, and synchronisation are important for any concurrent
      system
    * **Execution Context**
        - state of concurrent entity, must be able to create and delete it, and maintain state
          independently
        - must be able to save the state of one context and dispatch to another at various
          times
        - must be able to continue a context from the last execution, with same register
          contents
    * **Scheduling**
        - determine which context should be executed, switches between contexts when necessary
        - may allow each thread to run until block, round-robin, class scheduler or other
          scheduling policies
    * **Synchronisation**
        - threads cooperating to accomplish a task
        - a way to coordinate shared resources usage of concurrent execution contexts
        - common forms are mutexes, condition variables, semaphores, events, UNIX pipes,
          sockets, POSIX message queues, or other communication protocols between asynchronous
          processes
        - any form of communication protocol contains some form of synchronisation

Parallelism
-----------
    * tasks are executed at the same time, multiple CPU cores can run instructions
      simultaneously
    * software parallelism is the same as the word "concurrency" meaning, but different
      from software concurrency
    * true parallelism can only occur on a multiprocessor system
    * even without hardware parallelism, fast enough concurrency can make the tasks look like
      executing at the same time
    * e.g. RUBY MRI and CPython use global interpreter lock (GIL) to limit threading
    * **Scaling**
        - overhead of creating the extra threads and performing synchronisation
        - e.g. dual core may be 1.95 times faster than single core, quad core 3.8 times faster
          than triple core
        - scaling falls off as the number of processors increases, due to more chance of lock
          and memory collisions
        - Amdahl's law: $Speedup = \frac{1}{(1 - p) + \frac{p}{n}}$,
          (p = $\frac{Parallelizable Code}{Total Execution Time}$, n = number of processors)
        - Amdahl's law shows parallelism is limited by the amount of serialisation needed
        - with more synchronisation, parallelism has less benefit
        - better scaling with independent activities than highly dependent ones
    * **Barrier**
        - synchronisation mechanism that blocks each thread until a certain number has reached
        - e.g. keep any thread from executing a parallel code until all threads are ready

Thread Safety
-------------
    * code can be called safely, but does not require to run efficiently on multiple threads
    * can use mutexes, condition variables, and thread-specific data to make existing functions
      thread-safe
    * **Serialised Function**
        - to make a function that doesn't need persistent context thread-safe
        - e.g locking a mutex on entry and unlocking before return
        - can be called in multiple threads, but only one thread can truly perform at a time
    * **Example Thread-Safe Function**
        - multiple invocations can safely be run concurrently
        - granularity: amount of data that a mutex protects
        - inefficient way is to give a function its own mutex and lock it right away to make
          it thread-safe
        - multi-threaded programs usually add a mutex as member variable to data structures,
          to associate the lock with its data

        .. code-block:: c

           pthread_mutex_t foo_mtx = PTHREAD_MUTEX_INITIALIZER;
           // takes a global lock, and no two thread can run it at once
           void foo()
           {
               pthread_mutex_lock(&foo_mtx);
   
               // safe but inefficient execution
   
               pthread_mutex_unlock(&foo_mtx);
           }


    * **Reentrancy**
        - efficient thread-safe code with more sophisticated measures
        - often necessary to change the function interface to make it reentrant
        - should avoid relying on static data and any form of thread synchronisation
        - save state in a context structure controlled by the caller, which is responsible for
          synchronisation of the data

Asynchrony
----------
    * processes execute asynchronously with respect to each other in UNIX
    * e.g. ``ls | less`` have synchronisation by data dependency, ``less`` cannot be ahead of ``ls``
    * asynchronous process needs state to enable the OS to switch between them
    * UNIX processes has all required information to execute code, and additional state
      that is not directly related to execution context, e.g. address space, file descriptors
    * a thread has a program counter, a pointer to it's current instruction, pointer to
      the top of it's stack, general registers, and floating-point or address registers
    * a thread does not have most of the state associated with a process
    * all threads in a process share files, and memory
    * switching between two threads within a process is faster than switching between
      processes
    * threads of the same process share virtual address space and all other process data
    * non-blocking I/O is not same as async I/O
    * non-blocking I/O: allow the program to delay I/O operation until it can complete
      without blocking
    * async I/O: can proceed while the program does other things, context required for
      operation is cheaper than a thread

Thread Evaporation
------------------
    * **Initial Thread**
        - execution of main() in a C program, also called main thread
        - can do anything like other threads, e.g. get thread ID with ``pthread_self()``
        - if ID is accessible, another thread can wait or detach the initial thread
        - when ``main()`` returns, the process terminates without allowing other threads to
          complete
    * Evaporation: threads state released when the process exits
    * detaching a running thread only informs the system to reclaim resources when it
      terminates
    * as the process usually outlives the created threads, always detach each thread when
      finished to make resources used by terminated threads available to the process
    * terminated but not detached threads may retain virtual memory, stacks, and other
      resources
    * can use ``attribute`` to create a thread that will not be controlled

Invariants
----------
    * assumptions made by program about relationships between variables
    * e.g. relationship between each element in a queue through a pointer to next element
    * invariants usually break, and  make incorrect results or fail the program
    * make sure to fix broken invariants before other code encounter them
    * synchronisation protects the program from broken invariants
    * predicate: logical expression describing the state of invariants, may be a boolean,
      pointer NULL test result, more complicated expression, or return value from a function

Critical Sections
-----------------
    * also called serial regions, areas of code that affect a shared state
    * can almost always be translated into a data invariant, and vice versa
    * e.g. code section that removes a queue element

`back to top <#posix-threads>`_

Thread Life Cycle
=================

* `States`_, `Creation`_, `Startup`_, `Termination`_, `Recycling`_

States
------
    * **Ready**
        - thread able to run, but waiting for the processor
        - may have just started, unblocked, or preempted by other thread
        - timesliced: preempted for running too long
    * **Running**
        - currently running, can be more than one thread on multiprocessor system
        - was ready, and selected by processor for execution
        - one thread running usually means the other was blocked, or preempted
    * **Blocked** <a id="blocked"></a>
        - not able to run as it is waiting, e.g. condition variable, mutex, I/O to complete,
        - can also be blocked when it calls ``sigwait`` for a signal that is not currently
          pending, or system operations such as page fault
        - becomes ready again when it is unblocked
    * **Terminated**
        - terminated by calling ``pthread_exit()`` and return, or cancelled and handle cleanup
        - stays in terminated state until detached or joined
        - will be immediately recycled once detached
    * threads sleep when it's blocked, resource not available, or when preempted, the system
      reassign the processor on which it is running
    * a thread spends most time in Ready, Running and Blocked states

Creation
--------
    * initial thread is created when the process is created
    * on systems that support threaded programming, cannot execute any code without a thread
    * can also create a thread when a process receives POSIX signal if the process signal
      notify mechanism is set to `SIGEV_THREAD`
    * threads can be created using ``pthread_create()``, or other non-standard mechanisms
    * a thread is in ready state once created, and may remain in it before executing
    * no synchronisation between creation and ``pthread_create()`` return
    * thread may start, and even complete and terminate, before the function returns

Startup
-------
    * once created, will begin executing the thread start function with arguments given in
      ``pthread_create()``
    * for initial thread, ``main()`` is called from outside the program with ``argc`` and ``argv``,
      instead of single `void*`
    * most UNIX systems link the program with ``crt0.o``, that initialises the process and calls
      ``main()``
    * only when the initial thread returns, the process is terminated
    * call ``pthread_exit()`` instead of returning from ``main()`` to terminate the initial thread,
      and allow other threads to continue running
    * initial thread runs on the default process stack
    * if a thread overflows its stack, program will fail with a segmentation fault or bus error

Termination
-----------
    * usually terminate by returning from its start function
    * using ``pthread_exit()`` or ``pthread_cancel()`` terminate after calling each cleanup handler
      registered with `pthread_cleanup_push()`
    * non-NULL thread-specific data is cleared by calling associated destructors
    * threads in terminated state remain available for another thread to join
    * terminated thread keeps ``pthread_t`` value, and the ``void*`` return value
    * different from terminated with ``pthread_exit()``, cancelled thread return value is always
      ``PTHREAD_CANCELLED``
    * if the terminated thread is detached by ``pthread_join()``, it may be recycled before
      ``pthread_join()`` returns
    * return value should never be a stack address related with the terminated thread's stack

Recycling
---------
    * if the thread is created with ``PTHREAD_CREATE_DETACHED``, or other threads call
      ``pthread_detach()``, it is immediately recycled when it becomes terminated
    * when ``pthread_join()`` or ``pthread_detach()`` returns, the thread cannot be accessed again
    * resources that remain at termination are released when recycled

`back to top <#posix-threads>`_

Mutex
=====

* `Create & Destroy`_, `Lock & Unlock`_, `Non-blocking Mutex Lock`_, `Mutex Size/Scope`_
* mutual exclusion: only one thread is allowed to write at a time
* a type of semaphore, easier to use than other semaphore types
* used to modify and read data written by another thread
* must be used if the data written order matters
* by locking a mutex, only one thread will be able to enter the code section
* using a copied mutex is undefined, use a copy of pointer to a mutex
* perform an atomic operation while a mutex is locked
* threads sensitive to the invariant must use the same mutex before modifying the state of it

Create & Destroy
----------------
    * usually declared using ``extern`` or ``static``
    * use ``PTHREAD_MUTEX_INITIALIZER`` for static mutex with default attributes

        .. code-block:: c

           typedef struct my_struct_tag {
               pthread_mutex_t mutex; // protect access to value
               int value; // access protected by mutex
           } my_struct_t;
   
           my_struct_t data = { PTHREAD_MUTEX_INITIALIZER, 0 };


    * use ``pthread_mutex_init()`` when using ``malloc()`` to create a structure with mutex
    * must use dynamic initialisation for a mutex with non-default attributes, and destroy with
      ``pthread_mutex_destroy()``

        .. code-block:: c

           my_struct_t *data;
           int status;
   
           data = malloc(sizeof(my_struct_t));
   
           status = pthread_mutex_init(&data->mutex, NULL);
   
           status = pthread_mutex_destroy(&data->mutex);
   
           free(data);


    * if possible, associate a mutex with the data it protects
    * can destroy a mutex when no threads are blocked on it, and no additional threads will try
      to lock it
    * if in heap, unlock and destroy the mutex before freeing the storage of mutex

Lock & Unlock
-------------
    * lock with ``pthread_mutex_lock()`` or ``pthread_mutex_trylock()``, and unlock with
      ``pthread_mutex_unlock()``
    * need to lock around any code that read or write variables
    * error or self-deadlock when trying to lock a mutex that the calling thread already has

Non-blocking Mutex Lock
-----------------------
    * use ``pthread_mutex_trylock()``, return error status instead of blocking the caller if the
      mutex is already locked
    * unlock only if the function return with success
    * unlocking with error can unlock the mutex while other thread is relying on it locked

Mutex Size/Scope
----------------
    * the scope/size of mutex protection can be as big as necessary
    * e.g. when protecting two shared variables, each variable can have a small mutex, or a
      larger mutex can protect both
    * as it takes time to lock and unlock mutexes, code that locks fewer mutexes will usually
      run faster
    * in some situations, instead of threads waiting on one big mutex, splitting it into
      smaller ones can be effective
    * it is best to apply two separate mutexes to independent data

`back to top <#posix-threads>`_

Condition Variables
===================

* `Condition Wait`_, `Rules for Condition Variables`_
* wait for a predicate to become true, and communicate to other threads
* condition variables are not for mutual exclusion
* mutexes are used separately, and it is common for one mutex to have more than one condition
  variables

Condition Wait
--------------
    * automatically release the associated mutex, and wait until another thread signal or
      broadcast the condition variable
    * mutex must always be locked when waiting on a condition variable
    * when a thread wake up from condition wait, it always resume with the mutex locked

Rules for Condition Variables
-----------------------------
    * one variable to one predicate if possible, using one-to-many or many-to-one can cause
      deadlock or race problems
    * when sharing a condition variable between multiple predicates, always broadcast although
      signal is more efficient
    * both the condition variable and predicate are shared data in the program, and always
      controlled using the same mutex
    * it is safer to signal or broadcast a condition variable with mutex locked

`back to top <#posix-threads>`_

Deadlock
========

* `Lock Hierarchy`_, `Backoff`_, `Lock Chaining`_
* threads waiting each others' locks, but none unlocks for any other
* threads lock resources in different orders, and refuse to give locks up
* livelock: threads fight for access to the locks
* common solutions for deadlock are lock hierarchy and backoff
* can use lock hierarchy for well-defined code, and backoff for functions that need flexibility

Lock Hierarchy
--------------
    * if there is no clear hierarchical order, make an arbitrary order for locks, and always
      take earlier locks before later ones
    * e.g. if ``thread_1`` and ``thread_2`` need both ``mutex_a`` and ``mutex_b``, they must always lock
      ``mutex_a`` first and then ``mutex_b``
    * most efficient way to prevent deadlock, but not always easy to design
    * can create coupling between different parts of the program
    * **Creating Lock Hierarchy**
        - sort a list of mutexes in ID address order and lock them
        - can also name mutexes, and lock in alphabetical or numerical order
        - code with functional locking hierarchy will lock mutexes in proper order

Backoff
-------
    * take a lock, check other locks with ``pthread_mutex_trylock()``, and if it fails, release
      all locks in reverse order and try again from the start
    * unlocking in reverse order lower the chance that another thread will need to backoff
    * less efficient than lock hierarchy as it can make waste calls to lock and unlock
    * but flexible as there is no strict lock hierarchy
    * need to use ``sched_yield()`` to put the calling thread to sleep and at the back of the
      scheduler's run queue

Lock Chaining
-------------
    * special case of lock hierarchy, and compatible
    * lock the first mutex, after the second mutex is locked successfully, the first is no
      longer needed and released
    * useful in traversing data structures such as trees or linked lists
    * e.g. use lock hierarchy when balancing or pruning a tree, and chaining when searching
    * without any purpose, chaining can waste time locking and unlocking
    * use only when threads will always be active within different parts of the hierarchy

`back to top <#posix-threads>`_

Pthreads API
============

* `pthread_t`_, `pthread_create`_, `pthread_detach`_, `pthread_equal`_, `pthread_exit`_
* `pthread_join`_, `pthread_self`_, `pthread_mutex_destroy`_, `pthread_mutex_init`_
* `pthread_mutex_lock`_, `pthread_mutex_trylock`_, `pthread_mutex_unlock`_
* primary Pthreads synchronisation model uses mutexes for protection, and condition variables
  for communication
* functions return an error code from ``<errno.h>``, per-thread ``errno`` also available, but
  setting or reading it has overhead

pthread_t
---------
    * to hold ID returned by ``pthread_create()``
    * can declare with ``auto`` if ID is required only in a function, but mostly with ``static``
      or `extern`

pthread_create
--------------
    * ``int pthread_create(thread, attr, thread_function, arg)``
    * create a new thread executing the third argument, thread function's address
    * return 0 on success and stores the ID of new thread in the buffer pointed to by the
      thread, otherwise return error number on failure
    * can specify scheduling parameters at creation time or later while the thread is running
    * Thread Function: only expect single argument of type ``void*``, and return same type
    * cannot find the thread ID unless the creator or the thread itself stores it somewhere

pthread_detach
--------------
    * ``int pthread_detach(thread)``
    * mark the specified thread as detached, and release resources as soon as it terminates
    * return 0 on success, otherwise error number
    * if not detach, Pthreads can hold the thread's resources for another thread to determine
      it has exited, and retrieve a final result
    * detached thread cannot be joined or made joinable again
    * a thread can detach itself, or other thread that knows its ID can detach it
    * ``EINVAL``: not joinable thread
    * ``ESRCH``: invalid thread ID

pthread_equal
-------------
    * ``int pthread_equal(t1, t2)``
    * compare two thread IDs for equality
    * greater than or less than compare is not needed, as there is no ordering between threads
    * return nonzero value if equal, otherwise return 0

pthread_exit
------------
    * ``void pthread_exit(retval)``
    * terminate the calling thread
    * no return value, always succeed

pthread_join
------------
    * ``int pthread_join(thread, retval)``
    * waits for the specified joinable thread to terminate, and detach automatically
    * can check the thread's return value or completion status
    * return 0 on success, otherwise error number
    * will block the caller until the specified thread is terminated
    * if the calling thread is canceled, the target thread will not be detached and remain
      joinable
    * if multiple threads need to know for termination, wait on a condition variable instead of
      calling `pthread_join()`
    * ``EDEADLK``: deadlock detected, or specified thread is the calling thread, thread may hang
    * ``EINVAL``: not joinable thread, or another thread already waiting to join
    * ``ESRCH``: invalid thread ID

pthread_self
------------
    * ``pthread_t pthread_self()``
    * get ID of the calling thread
    * return thread ID, and always succeed

pthread_mutex_destroy
---------------------
    * ``int pthread_mutex_destroy(mutex)``
    * destroy the mutex object, setting it to an invalid value
    * return 0 on success, otherwise an error number
    * destroyed mutex can be reinitialised with ``pthread_mutex_init()``
    * only safe to destroy an initialised mutex that is unlock

pthread_mutex_init
------------------
    * ``int pthread_mutex_init(mutex, attr)``
    * initialised the mutex with attributes, default attributes are used for ``NULL``
    * return 0 on success, otherwise an error number
    * after successful initialisation, mutex state becomes initialised and unlocked
    * can also use ``PTHREAD_MUTEX_INITIALIZER`` macro for default attributes

pthread_mutex_lock
------------------
    * ``int pthread_mutex_lock(mutex)``
    * lock the given referenced mutex object
    * return 0 on success, otherwise error number is returned
    * the caller is blocked if the mutex is already locked by another thread

pthread_mutex_trylock
---------------------
    * ``int pthread_mutex_trylock(mutex)``
    * same as ``pthread_mutex_lock()``, but return immediately, and the caller is not blocked if
      the mutex is already locked by another thread
    * return 0 on success, otherwise error number is returned
    * ``EBUSY``: mutex already locked

pthread_mutex_unlock
--------------------
    * ``int pthread_mutex_unlock(mutex)``
    * release the given referenced mutex object
    * return 0 on success, otherwise error number is returned
    * if threads are blocked on the mutex, scheduling policy determine which thread will get it

`back to top <#posix-threads>`_

References & External Resources
===============================

* begriffs. (2020). Concurrent programming, with examples [online].
  Available at: https://begriffs.com/posts/2020-03-23-concurrent-programming.html
* Butenhof, David R. (1997). Programming with POSIX Threads. Reading, Massachusetts:
  Addison-Wesley.

`back to top <#posix-threads>`_
