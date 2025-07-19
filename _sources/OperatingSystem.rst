================
Operating System
================

1. `Basics`_

`back to top <#operating-systems>`_

Basics
======

* `How Computer Operates`_, `Storage`_, `I/O Structure`_
* OS is a program that manages computer hardware
* also acts as intermediary between user and hardware, by providing means for proper use of
  resources
* the kernel, a program running at all time, is part and core of an OS
* operating system and kernel are sometimes used interchangeably


How Computer Operates
---------------------
    * computer contains one or more CPUs and device controllers that can execute concurrently
    * a common bus provides access to shared memory
    * memory controller ensure orderly access to the shared memory
    * **Bootstrap Program**
        - initial program, firmware, that runs when a computer is powered up
        - stored in ROM (read-only memory) or EEPROM (electrically erasable programmable
          read-only memory)
    * **Interrupt**
        - a signal sent when an event occurs
        - hardware trigger it by sending a signal to CPU
        - software trigger it by executing system call, also called monitor call
        - when CPU receives an interrupt, it stops current execution and immediately transfer
          it to a fixed location
        - interrupted execution resumes when the interrupt service routine completes
        - interrupts must be handled quickly
        - interrupt vector: a table of pointers to interrupt routines, there are only a number
          of predefined interrupts
        - if necessary, interrupt routine must save CPU state and restore it before returning

Storage
-------
    * most programs are run from rewritable main memory, RAM (random-access memory), which is
      usually in the form of DRAM (dynamic random-access memory)
    * only static programs are stored in ROM
    * load instruction moves a word from main memory to internal register in CPU
    * store instruction moves the content of register to main memory
    * **Von Neumann Architecture**
        - fetch instruction from memory and store it in instruction register
        - instruction is decoded and may cause to fetch operands from memory
        - after instruction is executed, result may be stored back in memory
        - memory unit sees only a stream of memory, without knowing how they are generated
    * secondary storage: extension of main memory, to hold large data permanently, e.g.
      magnetic disk
    * other storage types are also available, such as cache memory, CD-ROM and magnetic tapes
    * various storage systems differ in speed, cost, size and volatility
    * volatile storage lose contents when power is removed
    * **Electronic Disk**
        - can be designed to be volatile or nonvolatile
        - electronic disk stores data in large DRAM array during normal operation
        - most devices contain hidden magnetic hard disk and battery for backup power
        - when external power is interrupted, electronic disk controller copies data from RAM
          to magnetic disk, and vice versa when power is restored
        - flash memory: a form of electronic disk, slower than DRAM but no power required to
          retain contents
        - NVRAM: nonvolatile, DRAM with backup battery
    * register > cache > main memory > electronic disk > magnetic disk > optical disk >
      magnetic tape
    * caches can be installed to increase performance

I/O Structure
-------------
    * CPUs and device controllers are connected through common bus
    * **Controllers**
        - each controller is in charge of specific type of device
        - more than one device may be attached to a controller
        - maintains local buffer storage and special-purpose registers
        - move data between managed devices and its local buffer storage
        - operating systems have a device driver for each device controller
    * **Example I/O Operation**
        - driver loads the appropriate registers within controller
        - controller check the contents to determine action to be taken
        - controller transfer data from device to its local buffer
        - controller informs the driver via interrupt upon transfer completion
        - driver returns control to OS, also return data or pointer to data for read operation
          or status information for other operations
    * interrupt-driven I/O is fine for small data, but not for large one, as one interrupt per
      byte is generated
    * **DMA (direct memory access)**
        - used for bulk data movement
        - controller transfer entire block of data directly to or from its buffer to memory
        - only one interrupt per block
    * CPU can do other work while device controller is performing operations
    * some high-end systems use switch instead of bus, as multiple components can talk to
      others concurrently, no need to compete for cycles on shared bus

`back to top <#operating-systems>`_
