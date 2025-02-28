==================
CompTIA Cert Notes
==================


1. `CompTIA A+`_
2. `CompTIA Network+`_

##

<a id="a+top"></a>
CompTIA A+ Cert Notes
=====================

3. `Safety & Professionalism`_
4. `Computer`_
5. `CPUs`_
6. `RAM`_
7. `Firmware`_
8. `Motherboard`_
9. `Power Supplies`_
10. `Mass Storage Technologies`_
11. `Implementing Mass Storage`_
12. `Essential Peripherals`_
13. `Building PC`_
14. `Windows`_
15. `Groups & Permissions`_
16. `Maintain & Optimize OS`_
17. `CLI`_
18. `Troubleshooting OS`_
19. `Display Technologies`_
20. `Networking`_
21. `Local Area Network`_
22. `Wireless Networking`_
23. `The Internet`_
24. `Virtualization`_
25. `Portable Computing`_
26. `Mobile Devices`_
27. `Troubleshooting Mobile Devices`_
28. `Printers`_
29. `Securing Computers`_
30. `Operational Procedures`_

`back to top <#top>`_

1. Safety & Professionalism
===========================

* "Do unto others as you would have them do unto you"
* "Five minutes early is on time, and on time is late"
* avoid using the word "you", as it can sound accusatory

magic question
--------------
    * "May i start working on the problem?"
* always listen and take notes about problems
* you get paid because people make mistakes and machines break
* ask open-ended questions to narrow the scope of the problem

Electrostatic discharge (ESD)
-----------------------------
    * the passage of a static electrical charge from one item to another

Antistatic Tools
----------------
    * antistatic wrist strap or ESD strap
    * **antistatic mat or ESD mat**
        - acts as a point of common potential
    * antistatic bag
    * **self-grounding**
        - when antistatic tools aren't available
        - take a moment to touch the power supply every once in a while as you work
        - not as good as wrist strap but better than nothing at all

Electromagnetic interference (EMI)
----------------------------------
    * prevent EMI by keeping magnets away from computer equipment

Radio Frequency Interference (RFI)
----------------------------------
    * cell phones, wireless network cards, baby monitors, microwave ovens
    * keep radio-emitting devices as far away as possible from other electronics
    * becomes a big problem when two devices share the same frequencies

Troubleshooting
---------------
    * **methodology**
        1. identify the problem
        2. establish a theory of probable cause
        3. test the theory to determine cause
        4. establish a plan of action to resolve the problem and implement the solution
        5. verify full system functionality and implement preventive measures if possible
        6. document findings, actions and outcomes

`back to top <#a+top>`_

2. Computer
===========


basic level stages of computer
------------------------------
    * input
    * processing
    * output
    * data storage
    * network connection

`back to top <#a+top>`_

3. CPUs
=======

* it's the speed of the CPU that makes it look intelligent

External data bus (EDB)
-----------------------
    * used to exchange information between CPU and outside components

general purpose registers
-------------------------
    * first used by 8088
        - AX, BX, CX, DX
    * in 32-bit processors
        - additional Extended EAX, EBX, etc.
    * in 64-bit processors
        - RAX, RBX, etc.

clock wire (CLK)
----------------
    * a single charge, called *clock cycle*, on CLK wire tells the CPU that another piece of
    information is waiting to be processed
    * CPU requires at least two clock cycles or to act on a command
    * **clock speed**
        - maximum number of clock cycles that a CPU can handle in a given period of time
        - although all CPUs come off of the same assembly lines, they have subtle differences in
        the silicon that makes on CPU faster than another
    * **system crystal (quartz oscillator)**
        - determines the speed at which CPU and the rest of the PC operate
        - sends out an electric pulse at a certain speed
        - the pulse signal goes first to a clock chip that adjusts the pulse, usually increasing
        - in the old days, crystal and clock chip must be adjusted to send out the correct clock
        pulse for the particular CPU
        - in today's systems, the CPU tells the motherboard the clock speed it needs, and the
        clock chip auto adjusts for the CPU

Memory & RAM (Random Access Memory)
-----------------------------------
    * enable CPU to jump to any line of stored code and all done at or at least near the clock
    speed of the CPU
    * terms for **quantities of bits**
        - 1 or 0 = a bit
        - 4 bits = a nibble
        - 8 bits = a byte
        - 16 bits = a word
        - 32 bits = a double word
        - 64 bits = a paragraph or quad word
    * **DRAM (dynamic RAM)**
        - used in computers
        - needs both constant electrical charge and a periodic refresh of the circuits, loses
        data otherwise
        - refresh can cause some delays and CPU has to wait for it

Address Bus
-----------
    * allows CPU to communicate with **memory controller chip (MCC)**
        - can grab the contents of any line of RAM and place that data on the [EDB](#edb)
    * 8088 had 20 wires in its address bus
        - 2^20 (1,048,576) combinations, has address space of 1,048,576 bytes or 1MB
        - 2^(number of wires in address bus) = maximum amount of RAM CPU can handle
    * **special prefixes** invented by *International Electrotechnical Committee (IEC)*
        - kibi (Ki) = 2^10
        - mebi (Mi) = 2^20
        - gibi (Gi) = 2^30
        - tebi (Ti) = 2^40

arithmetic logic unit (ALU)
---------------------------
    * also called integer unit
    * handles basic math for non-decimal numbers

floating point unit (FPU)
-------------------------
    * handles complex decimal numbers

ARM
---
    * developed by ARM Holdings
        - designs ARM CPUs, but doesn't manufacture
        - license to others such as Qualcomm
    * used in mobile devices
    * use **reduced instruction set computing (RISC)** architecture
        - a simpler, more energy efficient design
    * saves cost and battery life
    * but today, there's no clear distinction between RISC and *CISC (complex instruction set
    computing)*

Product lines & names
---------------------
    * **Intel**
        - Enthusiast: Core i7/i9
        - Mainstream desktop: Core i7/i5/i3
        - Budget desktop: Pentium, Celeron
        - Portable/Mobile: Core i7/i5/i3 (mobile), Mobile Celeron
        - Server: Xeon
        - Workstation: Xeon
    * **AMD**
        - Enthusiast: Ryzen, Ryzen Threadripper
        - Mainstream desktop: A-Series Pro, Ryzen
        - Budget desktop: A-Series, FX
        - Portable/Mobile: Ryzen, A-Series
        - Server: Opteron, EPYC
        - Workstation: Ryzen PRO, Ryzen Threadripper

microarchitecture
-----------------
    * major new design that each company comes up with about every three years

throttling
----------
    * saving energy by making CPU run more slowly when demand is light

TDP (thermal design power)
--------------------------
    * how much heat a busy CPU generates
    * measured in watts
    * mobile devices: 2-15 watts
    * laptop CPUs: 7-65 watts
    * desktop CPUs: 50-140 watts

Clock multipliers
-----------------
    * all CPUs run at some multiple of the system clock speed
    * in early systems, the clock speed and the multiplier had to be manually configured via
    jumpers or *dual in-line package (DIP)*
    * today's CPUs report to the motherboard through a function called *CPUID*, and the speed
    and multiplier are set automatically

64-bit processing
-----------------
    * new registers were added such as *multimedia extensions (MMX)*, *streaming SMID
    extensions (SSE)*
    * exabyte (EB) = 2^60
    * can handle 2^64 bytes or 16EB of RAM

Parallel execution
------------------
    * CPUs accomplish through multiple pipelines, dedicated cache and multiple threads
    * **Pipelining**
        - takes at least four steps
        - Fetch, Decode, Execute, Write
        - each stage does its job with each clock-cycle pulse, much more efficient
        - not always perfect, can hit a complex command requiring more than one clock-cycle that
        force the pipeline to stop
        - CPU tries to avoid **pipeline stalls**
           + also called wait states
           + mostly caused by decode stage as RAM can't keep up with CPU
        - with single pipline, only ALU or FPU worked at any execution stage

Cache
-----
    * **threads and data**
        - each thread is a series of instructions designed to do a particular job with the data
    * **SRAM (static RAM)**
        - called *cache*, built-in high-speed RAM to reduce wait states
        - preloads as many instructions as possible and keeps copies of already run data
    * **L1, L2, L3 caches**
        - L1 in CPU, smallest and fastest
        - L2 and L3 on CPU package
        - speed: L1 > L2 > L3
        - **frontside bus**
           + new term for address and external data bus
           + also referred to as double-pumped and quad-pumped buses
        - **backside bus**
           + connection between CPU and L2 cache

Multithreading
--------------
    * **Hyper-threading**
        - turning CPU into two CPUs on one chip
    * enhances CPU efficiency but has **limitations**
        - OS and apps must be designed to take advantage of the feature
        - doesn't double the processing power

Multicore Processing
--------------------
    * divide up work independently of the OS
    * differs from hyper-threading
    * still apps must be optimized to have impact on performance

Integrated memory controller (IMC)
----------------------------------
    * moved from motherboard chip into CPU to optimize flow of data
    * enables faster control over things like large L3 cache

NX bit
------
    * technology that enables the CPU to protect certain sections of memory

Common Sockets
--------------
    * zero insertion force (ZIF) sockets
    * **Intel**
        - uses *land grid array (LGA)* package, holes on CPU, pins on socket
        - LGA1150 / H3: Core i3/i5/i7, Pentium, Celeron, Xeon
        - LGA1151 / H4: Core i3/i5/i7, Pentium, Celeron, Xeon
        - LGA2011 / R or R3: Core i7, Core i7 Extreme Edition, Xeon
        - LGA2066 / R4: Core i5/i7/i9, Xeon
    * **AMD**
        - uses *pin grid array (PGA)*, pins on CPU, holes on socket
        - FM2+: A-Series
        - AM3+: FX, Opteron
        - AM4: Ryzen, A-Series
        - TR4: Ryzen Threadripper

numbering schemes
-----------------
    * **Intel**
        - Intel Core i7 7500 U
           + Intel Core = brand, i7 = brand modifier, 7 = generation, 500 = SKU numbers,
           U = alpha suffix (U for desktop processor using ultra-low power)
    * **AMD**
        - AMD Ryzen 7 2700X
           + AMD Ryzen = brand, 7 = market segment, 2 = generation, 7 = performance level,
           00 = model number, X = power suffix (X for high performance)

passive cooling
---------------
    * no fan, heat sint by itself on chip

OEM CPU coolers
---------------
    * Original Equipment Manufacturer heat-sinks and fans

`back to top <#a+top>`_

4. RAM
======

* to save space, manufacturers created wider DRAM chips, such as x4, x8, x16, and put multiple
of them on a small circuit borad called *stick* or *module*

SIMM (single inline memory module)
----------------------------------
* modern MCC provides at least 64 bits of data every time CPU requests info from RAM
* modern DRAM sticks come in 32-bit and 64-bit-wide data from factors (x32 and x64,called by
their width)

SDRAM (synchronous DRAM)
------------------------
    * synchronous, tied to the system clock so MCC knows when data is ready to be grabbed
    * first debuted on a stick called **DIMM (dual inline memory module)**
    * common **pin sizes**
        - desktops: 168-pin
        - laptops: 68, 144 or 172-pin micro-DIMM and 72, 144 or 200-pin *SO-DIMM (small-outline)*
        - all DIMM varieties delivered 64-bit-wide data except 32-bit 72-pin SO-DIMM
    * need a PC designed to use SDRAM
    * a DIMM in any one of the DIMM **slots** could fill the 64-bit bus
        - each slot is called a *bank*
        - need to install two sticks of RAM to make full bank on laptops that use 72-pin SO-DIMM
    * early SDRAM speeds: 66, 75, 83, 100 and 133 MHz (prefixed with "PC", PC66, PC133)
* RAM speed had to match or exceed the system speed
* 400-MHz frontside bus speed
    * wan't achieved by making the system clock faster
    * done by making CPUs and MCCs capable of sending 64 bits of data two or four times for
    every clock cycle

RDRAM (Rambus DRAM)
-------------------
    * could handle speeds up to 800 MHz
    * a stick of RDRAM was called **RIMM**

DDR SDRAM (double data rate SDRAM)
----------------------------------
    * also referred to as DDR, DDR RAM, DDRAM
    * desktops: 184-pin DIMMs (match 168pin DIMMs in physical size but not in pin compatibility)
    * laptops: 200-pin SO-DIMMs or 172-pin micro-DIMMs
    * **naming convention** based on the number of bytes per second
        - bytes per second = MHz speed multiplied by 8 bytes (the width of all DDR SDRAM sticks)
        - PC3200 = 400MHz x 8
        - DDRxxx for individual DDR chips
        - PCxxxx for DDR sticks
    * **example DDR speeds**
        - Clock_Speed DDR_Speed_Rating PC_Speed_Rating
        - 100 MHz     DDR-200          PC-1600
        - 166 MHz     DDR-333          PC-2700
        - 217 MHz     DDR-433          PC-3500
        - 275 MHz     DDR-550          PC-4400
        - 300 MHz     DDR-600          PC-4800
    * **dual-channel architecture**
        - using two sticks of RDRAM together to increase throughput
        - require two identical sticks of DDR

DDR2
----
    * clock double the input/output circuits on the chips
    * uses 240-pin DIMM that's not compatible with DDR
    * **example DDR2 speeds**
        - Clock_Speed DDR_Speed DDR2_Speed PC_Speed_Rating
        - 100 MHz     200 MHz   DDR2-400   PC2-3200
        - 133 MHz     266 MHz   DDR2-533   PC2-4200
        - 166 MHz     333 MHz   DDR2-667   PC2-5300
        - 200 MHz     400 MHz   DDR2-800   PC2-6400
        - 266 MHz     533 MHz   DDR2-1066   PC2-8500

DDR3
----
    * uses 240-pin DIMM
    * SO-DIMMs for laptops have 204 pins
    * not compatible with DDR2 socket
    * doubles the buffer of DDR2 from 4 bits to 8 bits
    * for **overclocking**
        - XMP or Extreme Memory Profile
        - AMP or AMD Memory Profile
    * also use higher-density memory chips up to 16GB DDR3 modules
    * **triple-channel or quad-channel architecture**
        - feature supported by some motherboards
        - work like dual-channel but with three or four sticks of RAM
    * I/O speeds are quadruple the clock speeds, due to increased in buffer size
    * **example DDR3 speeds**
        - Clock_Speed DDR_Speed DDR2_Speed PC_Speed_Rating
        - 100 MHz     400 MHz   DDR3-800    PC3-6400
        - 133 MHz     533 MHz   DDR3-1066   PC3-8500
        - 200 MHz     800 MHz   DDR3-1600   PC3-12800
        - 300 MHz     1200 MHz   DDR3-2400   PC3-19200
    * **DDR3L**
        - low-voltage version
        - runs at 1.35V (regular DDR3 at 1.5V or 1.65V)
    * **DDR3U**
        - ultra-low-voltage version
        - runs at 1.25V

DDR4
----
    * arrived in late 2014
    * offers higher density and lower voltage than DDR3
    * DIMMs can have maximum 64GB (at that time) and run only at 1.2V
    * performance version at 1.35V and low-voltage version at 1.05V
    * uses 288-pin DIMM, not backward compatible
    * DDR4 SO-DIMMs have 260 pinx, not compatible with DDR3 204-pin SO-DIMMs
    * switched from bit rate to megatransfers per second (MT/s)
    * **example DDR4 speeds**
        - Clock_Speed Bandwidth   DDR4_Speed  PC_Speed_Rating
        - 200 MHz     1600 MT/s   DDR4-1600    PC3-12800
        - 300 MHz     2400 MT/s   DDR4-2400    PC3-19200
        - 563 MHz     4500 MT/s   DDR4-4500    PC3-36000
        - 588 MHz     4700 MT/s   DDR4-4700    PC3-37600
* *single-sided RAM*: have chips on only one side of the stick
* *double-sided RAM*: have chips on both sides

Latency
-------
    * delay in RAM's response time
    * *column array strobe (CAS) latency*
    * 17CL: delays 17 clock cycles
    * in same speed rating RAMs, 17CL will be slightly faster than 19CL
    * speeding up system clock might take the RAM an extra click before it can respond
    * high-latency stick in a low-latency setup motherboard will be unstable or dead PC

parity RAM
----------
    * first type of error-detecting RAM
    * store and extra bit of data, parity bit, that the MCC used to verify data correct or not

ECC RAM (error correction code)
-------------------------------
    * also called error checking and correction
    * detects and corrects any time a single bit is flipped
    * can detect but not correct a double-bit error
    * slower than non-ECC RAM
    * comes in every DIMM package type and have some **odd-sounding numbers**
        - DDR4 RAM with 240-pin, 72-bit versions
        - SO-DIMM with 200-pin, 72-bit
        - extra 8bits beyond 64-bit data stream for the ECC
    * need designated motherboard for ECC RAM

registered RAM or buffered RAM
------------------------------
    * refers to a small register installed on some memory modules to act as buffer between DIMM
    and MCC
    * helps compensate for electrical problems that crop up in systems with lots of memory
    modules (eg. servers)
    * motherboard will use either buffered or unbuffered RAM, but not both

symptons for more RAM
---------------------
    * system slow
    * excessive hard drive accessing

Virtual Memory
--------------
    * portion of the hard drive used by OS to save a *page file* or *swap file*
    * default and recommended page-file size: 1.5 times the amount of installed RAM
    * fully automated process and does not require user intervention
    * **disk thrashing**
        - move programs between RAM and the page file

ReadyBoost
----------
    * Windows feature
    * use flash media devices as fast, dedicated virtual memory
    * system with SSDs won't see much benefit

Mixing RAM speeds
-----------------
    * worst case, cause the system to lock up every few seconds or minutes
    * some data corruption
    * won't break anything other than data

SPD (serial presence detect) chip
---------------------------------
    * added on RAM
    * motherboard auto detect and setup any DIMM
    * stores all info about DRAM
    * cannot fix a broken SPD chip
* system lockup: computer stops functioning
* page fault
    * milder error that can be caused by memory issues but not necessarily system RAM problems

NMI (non-maskable interrupt)
----------------------------
    * interruption the CPU cannot ignore
    * eg. BSoD (blue screen of death)
    * sometimes triggered by bad RAM, usually by buggy programming or clashing code

GPF (general protection fault)
------------------------------
    * error that can cause an app to crash

`back to top <#a+top>`_

5. Firmware
===========

* programs stored on ROM chips (those stored on dynamic media are called software)
* multiple controllers are combined into **chipset**
    * Intel call it *PCH (Platform Controller Hub)*
    * extends the data bus to every device on PC

scan code
---------
    * code pattern of ones and zeros sent by the scanning chip on keyboard to the keyboard
    controller when a key is pressed

BIOS (basic input/output services)
----------------------------------
    * program dedicated to enable CPU to communicate with devices
    * every device on the computer needs BIOS

ROM (read-only memory)
----------------------
    * nonvolatile, read-only
    * motherboards use flash ROM (same as smartphones or SSDs) called **system ROM chip**
        - contains code ( system BIOS, 2 to 30 lines) that enables CPU to talk to basic hardware

system BIOS
-----------
    * support all of the hardware that never changes (eg. keyboard, speaker)
    * support all hardware that might change from time to time (eg. RAM, HDD, SSD)

UEFI (unified extensible firmware interface)
--------------------------------------------
    * used in modern systems
    * **advantages** over BIOS
        - supports booting to partitions larger than 2.2TB
        - native 32 or 64-bit, let to include features for setup and diagnoses
        - handles all boot-loading duties, no need to jump boot sectors
        - portable to other chip types, not just 16-bit x86

CMOS (complementary metal-oxide semiconductor) chip
---------------------------------------------------
    * store all the various BIOS settings
    * handles the system's *real-time clock (RTC)*
    * has been incorporated into the main chipset
    * computer cannot access hardware if data on CMOS is different from the actual hardware
    * **system setup utility**
        - also called CMOS setup program
        - enable to access and modify CMOS data
    * **CMOS RTC RAM / CMOS clear**
        - find CMOS RTC clear wires
        - move the shunt from wires 1 & 2 to wires 2 & 3
        - wait for 10s and move the shunt back to default position
    * needs continuous charge, usually 3V Lithium coin battery (CR2032 battery)
    * **example errors of lost CMOS info**
        - CMOS config mismatch
        - CMOS date/time not set
        - BIOS time and settings reset
        - no boot device available
        - CMOS battery state low

TPM (Trusted Platform Module)
-----------------------------
    * secure cryptoprocessor
    * can be circuit board plugged into motherboard or built directly into the chipset
    * common use is hard disk encryption
    * include digital rights management (DRM), network access control, application execution
    control, password protection

Option ROM / Bring Your Own BIOS
--------------------------------
    * put the BIOS on the hardware device itself
    * most BIOS on option ROMs display information on boot

Device drivers
--------------
    * file stored on hard drive that contains all of the commands necessary
    * loaded into RAM on every system boot

POST (Power-On Self Test)
-------------------------
    * program stored on system ROM chip
    * checks out the system every boot
    * sends command to all standard devices to run their own built-in diagnostic
    * doesn't specify what to check
    * quality of diagnostic is up to those who made the device
    * send **beep codes** or **text messages** if POST errors occur
        - two beep codes (before and during video tests)
           + bad or missing video (one long beep followed by two or three short beeps)
           + bad or missing RAM (single beep that repeats indefinitely)
        - text erros are after the video has tested okay
    * **POST card**
        - inoperative device can sometimes disrupt POST, forcing the machine into endless loop
        and cause PC to act dead with no beeps and text erros
        - POST card monitor POST and identify which hardware is causing the problem
        - simple cards snapped at expansion slots (small two-character LED)
        - mostly use on dead PC to determine at which level it's dead

boot process
------------
    1. power supply tests proper voltage and sends a signal down the power good wire to wake CPU
    2. CPU immediately sends a built-in memory **address** via address bus
        * on Intel CPUs, first line of the POST program on the system ROM
    3. after POST, find the programs on hard drive to start the OS
    4. differs between old BIOS and UEFI
        * **old BIOS way**
            - POST pass the control to **bootstrap loader**
               + few dozen lines of BIOS code tacked to the end of POST program
               + reads CMOS info to find the OS
               + CMOS setup utility tell which devices to check and in which order (boot sequence)
            - **bootable disk or system disk**
               + has boot sector containing special programming designed to tell the system where
                 to locate the operating system
            - if good boot sector is found, bootstrap loader pass control to the OS and remove
            itself from the memory, goes to next device if not
            - some include **PXE (preboot execution environment)**
               + enable to boot a PC without any local storage by retrieving an OS from server over
            network
        * **UEFI system**
            - POST pass control to the **boot manager**
               + checks the boot configuration and loads the OS boot loader directly
               + no need to scan for a boot sector
            - UEFI firmware store the boot manager and boot configuration

`back to top <#a+top>`_

### 6. Motherboard


traces
------
  * wires that make up the buses
  * mostly of the ports used by the peripherals and distributes the power from power supply
* motherboards are **layered PCBs (printed circuit boards)**
  * copper etched onto a nonconductive material and coated with epoxy for strength
  * every motherboard is four or more layers thick
  * layered structure enables multiple wires to send data without signals interfering

characteristics
---------------
  * **form factor**
    - physical size
    - different form factors define different connections
    - **AT**
       + huge, around 12inches wide by 13inches deep
       + lack external ports
       + only dedicated connector was keyboard port
    - **ATX**
       + has all necessary ports built in
       + place RAM closer to the northbridge and CPU to offer ehanced performance
       + full-sized: 12 by 9.6 inches
       + microATX: 9.6 by 9.6 inches, not all boards have same physical size
       + FlexATX: 9 by 7.5 inches, not used anymore now
    - **ITX**
       + Mini-ITX: 6.7 by 6.7 inches
       + only tiny amount of power needed
    - **proprietary form factors**
       + have feature such as unique power connections and riser cards (separated from board
          but connected by cable, some plug into unique socket called daughter boards)
  * **chipset**
    - determine type of processor, RAM and internal/external devices that the board supports
    - relatively centrally located on the board
    - before, northbridge chip handle RAM and southbridge some expansion devices and storage
      drives, some have a third chip called Super I/O chip to handle legacy devices
    - today, chips have been combined
    - system ROM chip provides part of BIOS for the chipset, and other expansion devices BIOS
      from device drivers
  * built-in **components**
    - determine core functionality of the system
    - socket for CPU, slots for RAM, ports for storage devices, USB ports, NICs, etc.

network
-------

RAID (redundant array of independent)
-------------------------------------
  * mirroring: using two drives to hold same data
  * striping: making two drives act as one drive by separating data across

expansion bus
-------------
  * before, connect directly to the chipset
  * nowadays, expansion slots connect to the CPU and some types connect to the chipset
  * chipset provide extension of the address bus and data bus to the expansion slots
  * chips on expansion cards has CLK wires and need to be pushed by clock chip
  * extension to the external data bus run at its own standardized speed controlled by
    **expansion bus crystal**
  * expansion slots run slower than frontside bus
  * the chipset acts as the divider between two buses

PCI (peripheral component interconnect) bus architecture
--------------------------------------------------------
  * provided wider, faster, more flexible alternative than any previous expansion bus
  * original PCI bus was 32 bits wide and ran at 33 MHz
  * capable to coexist with other expansion buses
  * self-configuring
  * had a powerful burst-mode feature enabling efficient data transfers
  * **Mini-PCI**
    - use low power and lie flat, good for laptops
  * both only used on older computers

PCIe (PCI Express)
------------------
  * uses **serial** connection, not like PCI which use shared **parallel** communication
    - in parallel, 32 wires each carry on bit of data
       + need high speed checking of data as some bits can get faster than others
    - in serial, only one wire carries 32 bits
       + all bits arrive one after the other in single stream
       + almost one-to-one correlation between transfer rate and binary data rate
       + effective rate drops a bit due to encoding scheme (data broken down and reassembled)
  * doesn't share the bus, has its own direct connection (P2P) to the CPU so doesn't wait for
    other devices
  * uses one wire for sending and one for receiving
  * **PCIe lane**
    - pairs of wires between PCIe controller and a device
    - **speed** for each direction of lane
       + PCIe 1.x: 2.5 GTps (gigatransfers per second)
       + PCIe 2.x: 5 GTps
       + PCIe 3.x: 8 GTps
       + PCIe 4.0: 15 GTps
    - each P2P connection can use 1, 2, 4, 8, 12 or 16 lanes and can have theoretical
      maximum 256 GTps
    - full-duplex throughput can be up to 32GBps on x16 connection (often for video cards)
  * Mini-PCIe for laptops
  * installing PCIe card: knowledge, physical installation, device drivers, verification
  * always unplug the PC before inserting an expansion card
* motherboard mounts to the case via small connectors called *standoffs*
* incorrect installation of LED and frontmounted ports wires only results in device not
  working, it won't damage the computer
* very old boards require to set jumpers to determine the **bus speed**
  * too high bus speed result no power to CPU
  * too low, no optimal use of CPU

symptons
--------
  * catastrophic failure
    - no boot or shutdown after sometime
    - manufacturing defects called *burn-in failure*, uncommon and usually happen in first 30
      days of use
    - must replace motherboard
  * component failure
    - rare and usually loose connection between device and board
    - just replace component or upgrade BIOS
  * ethereal
    - reboot itself, getting BSoD
    - usually caused by faulty component, buggy device driver or software, corruption of OS,
      power supply problems
`back to top <#a+top>`_

### 7. Power Supplies

* PSU (power supply unit) falls into category of *field replaceable unit (FRU)*
* converts high-voltage AC power from the wall socket to low-voltage DC
* connects to the power cord via a standard **IEC-320 connector**
  * has three holes: **hot, neutral, ground**
    - hot wire carries electrical voltage
    - neutral wire no voltage, complete the circuit by returning electricity to local source
    - ground wire makes it possible for excess electricity to return safely to the ground
  * using **multimeter**: hot (115V), neutral & ground (0V)
    - volt-ohn meter (VOM) or digital multimeter (DMM)
    - offer at least four types of electrical tests: **continuity, resistance, AC voltage
      (VAC), DC voltage (VDC)**
       + continuity: test whether electrons can flow from one end of wire to other, can use
          to determine fuse if good or check for breaks in wire
       + resistance: broken wire or fuse will show infinit resistance
* available with dual-voltage options (110-120 V in US, ~115 VAC, 220-240 VAC in other)
* fixed-input: supplies with voltage-selection switches
* auto-switching: supplies not have to manually switch for different voltages

AC adapter
----------
  * many computing devices use this rather than internal power supply
  * converts AC current to DC just like power supply
  * things to match before plugging into device: **voltage, amperage, polarity**
    - too low voltage or amperage, device won't run
    - reversed polarity, same as putting a battery backward
    - always check these three before plugging into a device

handling spikes and sags
------------------------
  * surges or spikes are more dangerous than sags
  * **surge suppressors**
    - absorbs extra voltage from a surge to protect the PC
    - has the Underwriters Laboratories UL 1449 for 330-V rating should rate at a minimum of
      2000 joules
    - also clamp overvoltage to a more manageable voltage for a certain amount of time
    - most can clamp 600V down to 180V or less for at least 50&micro;s
  * **UPS (uninterruptible power supply)**
    - provides perfect AC power, moving back and forth 60 or 50 times a second
    - measured in both watts and volt-amps (VA)
    - watts value is a guess, VA rating is always higher than watt rating
    - **online**
       + devices are constantly powered through the UPS's battery
    - **standby**
       + devices receive power only when the AC sags below ~80-90V
    - **line-interactive**
       + similar to standby but has special circuitry to handle moderate AC sags and surges
          without the need to switch to battery power
    - always look for joule and UL 1449 ratings, replacement battery cost
    - UPS with USB or Ethernet (RJ-45) connection has monitoring and maintenance software
* most common size: 150 mm x 140 mm x 86 mm desktop PSU
* power supply converts AC into **3.3, 5.0, 12.0 DC voltages**
  * 12V to power motors on devices such as hard drives and optical drives
  * 3.3 and 5V current for support of onboard electronics
* modern motherboards use 20 or 24pin P1 power connector, some use special 4, 6 or 8pin
  connectors to supply extra power
* at least **three types of connector**
  * **Molex**<a id="molex"></a>
    - supply 5V and 12V current for fans and older drives
    - has notches called chamfers
    - require a firm push to plug in properly
    - always check for proper orientation
    - **case fans** may come with standard Molex connectors or with special three-pronged
      connectors that need to connect to motherboard
       + intake fan: near the bottom of the front bezel
       + exhaust fan: near the top and rear of the case
       + industry use 80mm power supply and cooling fans
       + sleeve-bearing fans get louder as they age; ball-bearing fans do not
       + can't use CMOS settings to tell the fans when to turn on or off
  * **mini or Berg**
    - few power supplies still support
    - supply 5V and 12V to peripherals
    - originally used as standard connector on 3.5 inch floppy disk drives
    - plugging in wrong way will destroy the device
  * **SATA (Serial ATA)**<a id="sata"></a>
    - SATA drives need a 15pin SATA power connector
    - larger pin count supports the SATA hot-swappable feature and 3.3, 5 and 12V devices
      (3.3V pins are not used in any current SATA drives and reserved for future use)
    - all three generations use L shaped connector
    - no other device uses the SATA power connector excpet a SATA drive
    - also supports slimline connector (has 6 pin power segment and 9pin micro connector)
* can use adapter if SATA connector is needed but only have Molex (same voltages on wires)

ATX power supply
----------------
  * **motherboard power connector**
    - single cable with 20pin P1
  * also had at least two other cables, each with two or more Molex or mini connectors
  * **soft power**
    - always run 5V to the motherboard
    - always on, even when powered down
    - power switch only tells the computer whether it has been pressed, not true power switch
    - prevents user from turning off a system before the OS has been shut down
    - enables PC to use sleep mode
    - all important settings reside in CMOS setup
  * **ATX 12V 1.3**
    - introduced 4 pin motherboard power connector (P4), provides more 12V power to assist
      the 20/24pin P1
    - **AUX connector**
       + 6pin auxiliary connector
       + supply increased 3.3 and 5V current to motherboard
  * **EPS12V**
    - non-ATX standard
    - 24pin main motherboard power connector that resembled a 20pin ATX connector
    - also came with AUX connector, ATX12V P4 connector and unique 8pin connector
    - not interchangeable with ATX12V
    - mostly use in servers
  * **Rails**
    - three primary DC voltage rails: 12V, 5V, 3.3V
    - each rail has a maximum amount of power it can supply
    - **over-current protection (OCP)**
       + monitor amount of amperage going through each rail
       + shut down the power supply if current goes beyong its cap
       + in single-rail system, single OCP circuit monitors all the pathways
       + multi-rail system, each pathway gets its own OCP circuit
    - 12V rails push 70amps or more
  * **ATX12V 2.0**
    - 24pin motherboard power connector is backward compatible with older 20pin connector
    - requires two 12V rails for any power supply rated higher than 230W
    - drop the AUX and requires SATA hard drive connectors
    - has a convertible 24-to-20-pin motherboard adapter (incompatible with P4)
  * never turns off as long as the power supply stays connected to a power outlet, will supply
    5V to the motherboard
* many modern ATX motherboards feature an **8 pin CPU power connector** (like in EPS12V) to
  supply power to high-end CPUs
  * also referred to as EPS12V, EATX12V, ATX12V 2x4
  * one half of connector is pin compatible with P4 power connector
  * other half may be under protective cap
  * in some power supplies, 8pin can be split inot two 4pin sets (one of which is P4)

auxiliary PCIe power connector
------------------------------
  * higher-end video cards have one or two sockets that require specific 6pin or 8pin PCIe
    power connectors
  * not compatible with EPS12V connector
  * some devices with 8pin connector will accept 6pin PCIe, but may limit performance
* some common power supply types
  * Mini-ITX and microATX
  * TFX12V: optimized for low-profile ATX systems
  * SFX12V: optimized for systems using FlexATX motherboards

Active PFC (power factor correction)
------------------------------------
  * **harmonics**
    - current pushing or pulling at the top and bottom of each cycle and creating an
    electrical phenomena
    - create humming sounds from components
    - damage electrical equipment
  * good PC power supplies come with active PFC
  * smooths out power coming from the wall before passing it to the main power supply circuits,
    eliminates any harmonics

Wattage requirements
--------------------
  * typical HDD draws about 15W
  * total wattage of all devices is the minimum for power supply to provide
  * all power supplies provide less power to the system than the wattage drawn from the wall,
    lost in heat generation
  * **graded for efficiency**
    - Bronze: 85%
    - Gold: 90%
    - Titanium: 94%
  * more efficiency, waste less power
  * big power supply will put out only required amount to the system
  * running a power supply at less than 100% load helps it live longer
  * voltages vary by as much as -10/+10% of stated values

power supply failures
---------------------
  * **sudden death**
    - computer will not start and the fan in power supply will not turn
  * **slowly over time**
    - cause some of the most difficult to diagnose problems
  * mostly use multiclass fire extinguisher (eg. ABC)
    - Class A: ordinary free-bruning (wood, paper)
    - Class B: flammable liquids
    - Class C: live electrical equipment
    - Class D: combustible metals (titanium, magnesium)
    - Class K: cooking oils

ATX tester
----------
  * power supplies will not start unless connected to a motherboard
  * can use ATX tester if motherboard not available
* power supply that provides 500W at 25&deg;C will supply substantially less in warmer temps
`back to top <#a+top>`_

### 8. Mass Storage Technologies

* traditional **HDD (hard disk drive)**
  * also called magnetic hard drives, rotational drives, platter-based hard drives
  * composed of individual disks or platters with **read/write heads** on actuator arms
    controlled by a servo motor
    - one to read the top of the platter
    - other to read the bottom of the platter
    - each head has a bit-sized transducer to read or write to each spot on the drive
  * run at a set **spindle speed**
    - older drives at 3600RPM
    - most common speeds: 5400 and 7200RPM
    - higher performance drives: 10,000 and 15,000RPM
  * faster drive, more heat
    - rise of 5&deg;C may reduce the life expectancy by as much as two years
  * drive bay fans at the front blow air across the drive
  * form factors: 2.5inch and 3.5inch
    - most laptops use 2.5inch form factor

SSD (solid-state drive)
-----------------------
  * based on the combination of semiconductors and  transistors
  * form factors: 2.5inch, flat form factors called mSATA & M.2 (both connect to specific
    mSATA or M.2 slots on motherboard)
  * **M.2 slots**
    - Key B, Key M or Keys B+M
    - key A and Key E are used in wireless networking devices
  * use nonvolatile flash memory chips to store data, such as NAND, that retains data when
    power is turned off or disconnected
  * **memory technology**
    - multi-level cell (MLC): less reliable and less expensive
    - single-level cell (SLC): more efficient and cost more
    - 3D NAND: most popular type, a form of MLC that stacks cells vertically
  * **write data** in a scattershot fashion in accordance with the rules contained in the
    internal SSD controller
    - process is hidden from the OS, making SSD appear to be traditional magnetic drive
  * **throughput**
    - rates at which SSD can read and write long sequences of data, expressed in MBps
    - traditional hard drives top out at 200MBps
    - SATA SSDs max to 600MBps
    - NVMe SSDs at 2500MBps or faster
  * **random read/write**
    - measure read or write small, fixed-size chunks of data at random locations on the drive
    - reflect size of data chunk, usually 4KB (4K read, 4K random write, 4K mixed)
    - typically expressed as a number of *input/output operations per second (IOPS)*
    - traditional hard drives at fewer than 150IOPS
    - NVMe SSDs at hundreds of thousands of IOPS
  * **latency**
    - measures how quickly it responds to a single request
    - express in ms or &micro;s
    - traditional hard drives under 20ms, SSDs under 1ms

Hybrid Hard Drives (HHDs/SSHDs)
-------------------------------
  * supported by Windows
  * drives that combine flash memory and spinning platters to provide fast and reliable
  * store most accessed data in the flash memory (eg. slash boot times)
  * can extend the battery life for portable computers
  * Apple use Fushion Drive which separates hard drive and SSD

connection
----------
  1. **standardized physical connections** between CPU, drive controller and physical drive
      * ST-506: late 1980s to early 1990s
      * PATA: early 1990s to early 2010s
      * SATA: late 1990s to today
      * M.2: late 2010s to today
      * SAS: 2005 to today
  2. CPU needs to use **standardized protocols**
      * **ATA (advanced technology attachment) standards**
        - ATA drives are often called integrated drive electronics (IDE)
        - went through seven major revisions
        - ATA/ATAPI version introduced **S.M.A.R.T** (Self-Monitoring, Analysis and Reporting
         Technology)
           + internal drive program that tracks erros and error conditions
           + stored in nonvolatile memory on the drive
        - **PATA (parallel ATA)**<a id="pata"></a>
           + introduced with ATA/ATAPI version 1
           + use 40pin ribbon cables or IDE cables, usually plugged directly into motherboard
           + all PATA drives use standard [Molex power connector](#molex)
           + latest drive speed up to 133MBps
           + single ribbon cable could connect up to two drives to a single ATA controller
           + cables impede airflow and have limited length (18inches)
           + cannot hot-swap
        - **SATA (serial ATA)**
           + introduced with ATA/ATAPI version 7
           + creates P2P connection between device and controller, HBA (host bus adapter)
           + SATA interface needs far fewer physical wires, only 7 connectors
           + cable length about 40inches (1 meter)
           + each drive to one port, no maximum number of drives
           + many motherboards support up to eight SATA drives
           + speed up to 30 times faster than PATA
           + SATA-specific varieties: 1.5Gbps, 3Gbps, 6Gbps
           + encoding scheme used takes about 20% of the transferred bytes as overhead,
             leaving 80% for pure bandwidth
           + hot-swapping: plug device without harming and auto recognized and functional
           + **SATAe (SATA Express) or SATA 3.2**
             + ties drives directly into PCI Express bus
             + drops both SATA link and transport layers
             + lack of overhead enhance the speed of SATA throughput, up to 8Gbps
             + uniques connectors but provide full backward compatibility
             + need a motherboard with SATAe support
           + SATA 1.0: 1.5Gbps/150MBps
           + SATA 2.0: 3Gbps/300MBps
           + SATA 3.0: 6Gbps/600MBps
           + SATA 3.2: up to 16Gbps/2000MBps
           + SATA 3.3 (2016) increased support drive sizes but not throughput speed
           + most hard drives sold today are SATA drives
      * **eSATA (external)**
        - eSATA port operate at the same revision and speed as internal port
        - used connectors similar to inter SATA but keyed differently
        - used shielded cable in lengths up to 2m outside the PC and hot-swappable
        - disappeared when USB 3.0 came out
  * both provided by Small Form Factor (SFF) committee
  * **external enclosures**
    - casing of external HDDs and SSDs
    - use USB 3.0, 3.1 or C ports or Thunderbolt ports
  * **cable lengths**
    - PATA: 18 inches
    - SATA: 1 meter
    - eSATA: 2 meters

drive command sets
------------------
  * **AHCI**
    - Advanced Host Controller Interface, for spinning SATA drives
    - unlocks some advanced features of SATA, such as native command queuing and hot-swapping
    - **NCQ (native command queuing)**
       + disk-optimization feature for SATA drives to achieve faster read/write speeds
    - enabled at [CMOS](#cmos) level
    - needs to be enabled before installing OS
    - options/modes/HBA configs
       + IDE/SATA or compatibility mode
       + AHCI (works best for most system)
       + RAID
  * **NVMe**
    - Non-Volatile Memory Express
    - circuitry that makes the OS see SSD as traditional spinning drive
    - supports communication connection between the OS and SSD directly through [PCIe](#pcie)
     bus lane
    - formats: add-on expansion card and M.2

SCSI (small computer system interface)
--------------------------------------
  * rules the server market
  * parallel and serial, both use standard SCSI command set
  * use a variety of ribbon cables depending on the version
  * **SAS (Serial Attached SCSI) hard drives**
    - provides fast and robust storage for servers and storage arrays today
    - SAS-3 provides speed up to 12Gbps
    - SAS contorllers aslo support SATA drives
- Data is king, data is your PC's raison d'etre

RAID (redundant array of independent disks)
-------------------------------------------
  * one drive is primary drive, and the other mirror drive
  * **disk mirroring**
    - reading and writing data at the same time to two drives
  * **disk duplexing**
    - using separate controller for each drive
    - super-drive mirroring technique
    - faster than disk mirroring as one controller does not write each piece of data twice
  * both methods are slower than classic one-drive, one-controller setup
  * **disk striping (without parity)**
    - spreading data among multiple (at least two) drives
    - by itself provide no redundancy
    - file is split into multiple pieces, saving half of the pieces on each drive
    - only advantage is speed
    - all data is lost if either drive fails
  * **disk striping (with parity)**
    - protects data by adding extra information called parity data, that can be used to
     rebuild data if one of the drives fails
    - requires at least three drives
    - protects data and is quite fast
    - used by majority of network servers
  * **JBOD**<a id="jbod"></a>
    - just a bunch of disks or drives
    - a term for a storage system composed of multiple independent disks fo various sizes
  * **RAID 0**
    - disk striping, requires at least 2 drives, scary RAID
    - number of functional failures: 0
  * **RAID 1**
    - disk mirroring/duplexing, require at least 2 drives or any even number of drives
    - ultimate in safety but lose storage space as data is duplicated
    - number of functional failures: 1
  * **RAID 5**
    - disk striping with distributed parity, require at least 3 drives
    - use one drive's worth of space for parity
    - number of functional failures: 1
  * **RAID 6**
    - disk striping, RAID 5, with extra parity
    - need at least 4 drives but can lose up to two drives at the same time
    - number of functional failures: 2
  * **RAID 10 (RAID 1+0)**
    - nested, striped mirrors
    - at least 4 drives, a pair as mirror and another pair to achieve RAID 1 arrays
    - arrays look like single drive to the OS or RAID controller
    - number of functional failures: up to 2
  * **RAID 0+1**
    - like RAID 10 but works in opposite config from RAID 10
    - at least four drives
    - start with two RAID 0 striped arrays, then mirror the two arrays to each other
  * specialized RAID controller cards support RAID arrays of up to 15 drives
  * **software RAID**
    - does not require special controllers
    - can use the regular SATA controllers
    - built-in RAID software in Windows, which can configure RAID 0, 1, or 5 and works with
     PATA or SATA
    - operating system is in charge of all RAID functions
  * **hardware RAID**
    - centers on intelligent controller that handles all of RAID functions
    - controllers have chips with their own processor and memory
    - almost all provide hot-swapping
    - invisible to the OS
    - have special config utility in flash ROM that need to be accessed after CMOS but before
     the OS loads
  * **dedicated RAID box**
    - take two or more drives and connect via USB or Thunderbolt or FireWire or eSATA

drive installation
------------------
  * [**PATA drives**](#pata)
    - have jumpers on the drive that must be set properly
    - set jumpers to master or standalone for only one hard drive
    - two drives: set one to master and other to slave or both to cable select
    - make certain that pin 1 on the controller is same wire as pin 1 on the hard drive
    - on newer motherboards, require special add-on PATA controller expansion card
  * [**SATA drives**](#sata)
    - supports only single device per controller channel
    - some older drives have jumpers to configure SATA version/speed or power management

BIOS support for dirves
-----------------------
  * motherboards provide support for SATA hard drive controllers via system BIOS but require
    config in CMOS for specific hard drives attached
  * most controllers reamin active and auto detect new drives, but can be disables
* common numbering method for drives
  * channels for each controller (eg. first boot device as channel 1)
`back to top <#a+top>`_

### 9. Implementing Mass Storage


hard drive partitions
---------------------
  * sectors: magnetically preseted storage areas
  * older hard drives: 512byte sectors
  * modern drives: 4096byte Advanced Format (AF) sectors
  * **SSDs**
    - each NAND chip storing millions of 4096byt storage areas known as *pages*
    - a group of pages are combined into a *block*, which has common size of 128 pages
  * **LBA (logical block addressing)**<a id="lba"></a>
    - makes any form of storage easy and the OS interacts with storages via it
    - used by contorllers on HDD or SSD to present all storage chunks as a number starting
      at LBA0
    - LBA chunks are also called blocks
  * partition is the first step in organizing mass storage and provides flexibility
  * never actually split a partition
  * to turn one partition into two, remove the existing and create two new ones or shrink the
    existing and add new one to the unallocated space

partition in Windows
--------------------
  * **basic disk**
    - uses either MBR or GPT
    - uses partitions and logical drives
  * **dyanmic disk**
    - uses the dynamic storage partitioning scheme
    - once basic disk is converted to dynamic, primary and extended partitions no longer exist
    - dynamic disks are divided into volumes instead of partitions
    - conversion from dynamic to basic requires to delete all volumes
    - capability to extend and span volumes
  * system will run perfectly even if hard drives use different schemes
  * **MBR (master boot record)**
    - in the first sector of MBR drive, code that informs the system about the installed OS
    - also contains partition table which describe number and size of partitions
    - support up to four partitions
    - *partition boot sector* loads the OS after locating appropriate partition
    - each partition has a partition boot sector
    - only one MBR and one partition table per disk
    - partition table support two types of partitions: primary and extended
    - single MBR disk may have up to 4 primary or up to 3 primary and 1 extended
    - **primary partitions**
       + to support bootable OS, usually assigned drive letters and appear in File Explorer
       + small primary, "System Reserved", for essential Windows boot files
       + has special settings, *active*, stored in partition table that determines the active
         partition
       + BIOS/POST read MBR to find the active partition and boots the OS on that partition
       + only one partition can be active at a time as only one OS is run at a time
       + limited to only 4 drive letters is using only primary partitions
    - **extended partitions**
       + not bootable, can contain multiple logical drives
       + each logical drive is in the same extended partition
       + extended partitions don't receive drive letters but logical drives do
  * **proprietary dynamic storage scheme**
    - volume: drive structure created with a dynamic disk
    - technically a partition, but it can do things a regular partition cannot
    - can create as many volumes as want
    - can create new drive structures with software that are impossible with MBR drives
    - can implement RAID, span volumes over multiple drives and extend volumes on one or more
    - simple, spanned, striped and mirrored are compatible on all Windows/Server while RAID 5
     is only compatible with Windows Server
    - **simple volumes**
       + works a lot like primary partitions
       + cannot install operating system
    - **spanning volumes**
       + use unallocated space on multiple drives to create a single volume but a bit risky
       + can also extend the volume to grab extra space on completely different dynamic disks
    - **striped volumes**
       + RAID 0 volumes
       + using tow or more drives in a group called *stipe set*, striping writes data first
         to a certain number of clusters (allocation unit) on one drive and then to next
       + all stripes must be same size on each drive
    - **mirrored volumes**: RAID 1 volumes
  * **GPT (GUID partition table)**
    - **GUID (globally unique identifier)**
       + provides reference number for an object or process that has almost impossibly small
         chance of duplication
    - shares a lot with MBR scheme but has **improvements**
       + can have unlimited number of primary partitions (Windows is limited to 128)
       + MBR partitions cannot be larger than 2.2TB but GPT is limited only in z
         (almost no restrictions)
    - arranged by [LBA](#lba) instead of sectors, LBA0 is protective MBR so that utilities
     know it is GPT drive
    - use GPT header and partition entry array, both located at beginning and end of drive
    - can configure 64bit Windows to boot from GPT only if using UEFI motherboard
    - most Linux distros can boot from GPT with older BIOS or UEFI

other partition types
---------------------
  * **hidden partition**
    - just a primary partition that is hidden from OS
    - only special BIOS tools can access
    - used to hide backup copy of OS to restore system, called factory recovery partition
  * **swap partitions**
    - found only on Linux and UNIX systems, to act like RAM
    - Windows has similar function, *page file* that use special file instead of partition

tools for partitioning
----------------------
  * Windows: FDISK (cli), Disk Management (GUI), diskpart (cli), Avanquest (third-party)
    - Disk Management does not enable to specify primary or extended partition when creating
     a volume on MBR drives and can't do any nested RAID arrays (RAID 0+1 or RAID 1+0)
  * Linux: fdisk, GParted
* in early days, partition size or type cannot be changed once made with any Microsoft tools

Formatting
----------
  * process of making a partition into something that stores files
  * must format every partition/volume
  * first, creates a files system enabling file storage and retrieval
  * second, create a root directory in file system to enable the partition to store folders

File systems
------------
  * macOS
    - APFS (Apple File System) default, HFS+ (Hierarchical File System Plus)
    - only read NTFS
  * Linux
    -lots to choose; most used is ext4 (Fourth Extended File System)
    - older distros ext2 or ext3, enterprise level BTRFS, XFS, ZFS
    - ext4 supports volumes up to 1EB with file sizes up to 16TB and backward compatible
    - able to read/write to NTFS, FAT32, exFAT, HFS+, ext4
  * Windows
    - NTFS, FAT32, exFAT
    - all Windows fs organize blocks of data into groups called clusters (size varies with
     file system and the size of partition)
  * **FAT32**<a id="fat32"></a>
    - each block stores up to 4096bytes of data
    - in small partition, each cluster is made up of 1 block
    - will use many clusters as needed if file is larger than 4096bytes
    - if a file smaller than 4096bytes is stored, rest of the cluster goes to waste (this
     waste is acceptible as most files are larger than 4096)
    - **FAT (file allocation table)**
       + first supported in MS-DOS version 2.1
       + data structure and indexing system to keep track of stored data on hard drive
    - **left column** gives each cluster a hexadecimal number from 00000000 to FFFFFFFF
       + each hexa 4bits, eight hexa 32bits
    - **right column** contains info on the status of cluster
       + even brand-new drives contain faulty blocks that cannot store data because of
         imperfections in the construction of the drive
       + high-level formatting: OS mapping bad blocks and marking them unusable
       + quick format: creates the FAT and creates a blank root dir
       + full format: testing every sector to mark unusable one in the FAT
       + bad block: 0000FFF7
       + good blocks: 00000000
    - when **saving a file**
       + starts at the beginning of the FAT to look for first space marked 00000000 and store
       + if file fits within one cluster, cluster status area is marked with 0000FFFF (last
         cluster), called end-of-file marker
       + if require more than one cluster, searches for the next open cluster and places the
         number of next cluster in the status area, repeat until entire file is saved
       + after saving all clusters, Windows locate the file's folder (which are stored on
         blocks but has different set of blocks), and records the filename, size, date/time,
         starting cluster
       + process is reversed if a program request a file
    - FAT32 auto makes two copies of the FAT
    - offers 4KB cluster sizes up to a partition size of 2GB (match the size of 4KB block)
    - supports dirves up to 2TB and files up to 4GB
    - Drive Size         Cluster Size
    - 512MB to 1023MB    4KB
    - 1024MB to 2GB      4KB
    - 2GB to 8GB         4KB
    - 8GB to 16GB        8KB
    - 16GB to 32GB       16KB
    - >32GB              32KB
    - still commonly used but not for OS partitions
    - when **deleting a file**
       + Windows only alter the info in the folder, changing the first letter of file to
         lower Greek letter
       + causes file to disappear to OS (won't show up in Explorer)
       + does not actually delete files but move the file listing (not actual blocks) to
         hidden dir that can be accessed via Recycle Bin
       + in 1st gen SSDs
           + once data was written into memory cell, it stayed there until dirve is full
           + the cell wasn't immediately erase or overwritten even if contain deleted contents
           + SSD controller don't know the cell's contents were deleted
           + SSD memory cells have finite number of write times before wearing out
           + waited until all the cells were filled before erasing and reusing previous cell
  * **NTFS (New Technology File System)**
    - uses clusters of blocks and file allocation tables
    - improvements: redundancy, security, compression, encryption, disk quotas, cluster sizing
    - use enhanced file allocation table called *master file table (MFT)*, immovable chunk
    - keeps a backup copy of the most critical parts of the MFT in the middle of the disk
    - views individual files and folders as objects and provides security through a feature
     called *Access Control List (ACL)*
    - enable to compress individual files and folders to save space
    - compression make access time to the data slower
    - in Windows, can encrypt with utility called *encrypting file system (EFS)*
    - disk quotas enable admins to set limits on drive space usage for users (usually used on
     multi-user systems)
    - Drive Size         Cluster Size
    - 7MB to 16TB        4KB
    - 16TB to 32TB       8KB
    - 32TB to 64TB       32KB
    - 64TB to 128TB      64KB
    - 128TB to 256TB     128KB
    - supports partitions up to ~16TB on dynamic disk, up to 2TB on basic disk
    - by tweaking cluster sizes, can get partition size up to 16exabytes
    - Windows require NTFS on any partition greater than 32GB
  * **exFAT**
    - support partition size up to 64ZB (2^70) and files up to 16EB (2^60)
    - extends FAT32 from 32bit cluster entries to 64bit

Fragmentation & Defragmentation
-------------------------------
  * fragmentation
    - file is stored in not contiguous manner
    - takes place all the time on [FAT32](#fat32) systems
    - SSDs also have fragmentation
  * defragmentation
    - removing fragmentation and improving read/write speeds
    - crucial for ensuring top performance of a mechanical hard drive
    - defragging SSD can shorten its lifetime
* Operating System: any software that can boot up a system
* boot device or boot disc: any removable media that has a bootable OS
* a hard drive must have a partition and be formatted to support an OS installation

disk initialization
-------------------
  * process of placing special information onto the drive
  * information include identifiers and other that define what the hard drive does in system
  * all new drives must be initialized before use

drive status
------------
  * unallocated
  * active
  * foreign drive: when dynamic disk is moved from one computer to another
  * formatting
  * failed: damaged or corrupt
  * online: healthy and communicating
  * offline: corrupt or communication problems
  * new installed drive is always set as basic disk

storage spaces
--------------
  * virtual dirves created from storage pool free space
  * Windows 8 and later, can group one or more physical drives of any size into single storage
    pool
  * storage spaces functions like a RAID management tool
  * resiliency (providing one or more layers of redundancy like RAID) and fixed provisioning
  * thin provisioning feature: can create a space with more capacity than current physical
    drive provide and don't have to redo an array or space if limits reached
  * **Simple spaces**
    - just pooled storage like [JBOD](#jbod)
    - no resiliency
    - good for temp storage, scratch files
  * **Mirrored spaces**
    - keep more than one copy of data like RAID mirror array
    - number of drives in the array determines mirror options (2 for two-way, 5 or more for
     three-way)
  * **Parity spaces**
    - add another layer of resiliency like RAID 5 or RAID 6
    - more space efficient than two-way mirroring
    - can impact overall performance
    - mostly for big files that don't change a lot (like movie collection)
    - can lose 1 drive and recover in a three-drive parity space
    - need seven-drive parity space to recover from a two-drive loss
  * SSDs work great with some space types (simple two or three-way mirror) and not others

error checking tools
--------------------
  * Windows: chkdsk (cli), Error checking (GUI)
  * macOS: Disk Utility
  * Linux: fsck (cli)
  * when the tool finds bad blocks, it puts the electronic equivalent of orange cones (placing
    0000FFF7 in FAT/MFT) around them so the system won't try to place data
  * use if Windows error or core boot files become corrupted
  * after checking, drive may have many bad blocks and need to be recycled
  * built-in **error correction code (ECC)**
    - constantly checks the drive for bad blocks

disk cleanup tools
------------------
  * clean files in recycle bin, temporary internet files, downloaded program files, temp files
  * Windows: Disk Cleanup
  * Linux: BleachBit
* drive installation errors: connectivity, system setup, partitioning, formatting
* partitioning errors: failing to partition and making wrong size or type of partition
* formatting errors: failing to format a drive makes it unable to hold data
* modern drives hide significant number of extra blocks that they use to replace bad blocks
* dying drives may make sounds like
  * continuous high-pitched squeal
  * loud clicking noise, a short pause and another series of clicks
  * continuous grinding or rumbling
* if autodetect doesn't see the drive, it may have a physical problem
* recovering broken hard drive data prices usually start around $1000
* properly functioning hardware RAID array will always show up in the configuration utility
  * if the array is gone but drives can be seen, then the controller may have broken the array
    on its own
* a live CD does not touch the hard drive
`back to top <#a+top>`_

### 10. Essential Peripherals


Serial Ports
------------
  * old devices connect through serial connections
  * 9pin, D-shell male socket called a DB-9 or RS-232

USB Ports
---------
  * bidirectional connection, can suffers from electrial interference
  * core of USB is the **USB host controller**
    - act as the interface between the system and every USB device
    - single host controller supports up to 127 USB devices
    - sends commands and provides power to USB devices
    - host controller is *upstream*, controlling devices connected *downstream* to it
    - shared by every device plugged into it
  * connected to the host controller is the **USB root hub**
    - part of host controller that makes physical connection to the USB ports
    - every USB root hub is a bus
  * **USB1.1**
    - Low-Speed: 1.5Mbps (keyboards, mice)
    - Full-Speed: 12Mbps (headphones, bluetooth devices)
    - use the USB1.1 host controller
  * **USB2.0**
    - Hi-Speed: 480Mbps (webcams, card scanners, old wireless adapters & flash-media drives)
    - backward compatible with USB1.1 devices
    - use the USB2.0 host controller
  * **USB3.0**
    - SuperSpeed: up to 5Gbps (flash-media drives, external storage, cameras, wirelss adapter)
    - also referred to as USB3.1 Gen1
  * **USB3.1**
    - SuperSpeed: 10Gbps (flash-media drives, external storage, networking)
    - also referred to as USB3.1 Gen2
    - both Gen1 and Gen2 are backward compatible with USB2.0 devices but use separate host
     controller
  * must connect a USB device to a USB port at least as fast as the device
  * connectors: Type-A, Type-B, Mini-B, Micro-B, Micro-B3.0
  * USB A connectors plug upstream toward the host controller
  * USB B connectors plug downstream into USB devices
  * USB 1.1 (port color: White) & 2.0 (port color: Black) cables use 4pin connectors
  * USB 3.0 (port color: Blue)/3.1 (port color: Teal) A and B ports and connectors use 9pin,
    3.0 connector look exactly like USB A
  * **USB Type-C**
    - use 24pins and can be inserted either orientation
    - fully supports USB3.1 and other busses such as Thunderbolt
  * **cable lengths**
    - USB1.1 & USB2.0: 5 meters
    - USB3.x: no limit
  * **USB hubs**
    - device that extends a single USB connection to two or more USB ports
    - powered and bus-powered versions
  * mismatch between available and required power for devices result in error codes and can
    even malfunctioning
  * sometimes USB devices go to sleep to save power and won't wake up

FireWire Ports
--------------
  * also known as IEEE 1394, looks and acts much like USB
  * has all features of USB but uses different connectors
  * first standard IEEE 1394a: 400Mbps, IEEE 1394b: 800Mbps
  * were primarily used isn Apple devices but replaced by Thunderbolt

Thunderbolt Ports
-----------------
  * developed by Intel and Apple as high-speed alternative
  * use PCIe bus for up to six external peripherals
  * functions alongside USB these days
  * supports video up to single 4K monitor
  * Thunderbolt 1 & 2: connect with mDP (mini displayport)
  * Thunderbolt 1: full duplex at 10Gbps
  * Thunderbolt 2: combines internal data channels and up to 20Gbps
  * Thunderbolt 3: uses USB Type-C connector but not compatible with USB, up to 40Gbps at half
    the power consumption of Thunderbolt 2
  * can use copper (extend up to 3m) or fiber cabling (extend up to 60m)
* almost any I/O port on a motherboard can be turned off in system setup

common peripherals
------------------
  * **Keyboards**, Pointing devices (mouse, touchpad)
    - repeat delay: amount of time to hold down a key before the keyboard starts repeating
     the character
    - repeat rate: how quickly the character is repeated after the repeat delay
  * **Biometric devices**
    - recognition is different from security in that the device doesn't care who the person is
  * **Smart card readers**
    - cans the chip embedded
  * **Barcode scanners/QR scanners**
    - read standard *UPC (universal product code) barcodes* or *QR (quick response) codes*
    - pen scanners and hand scanners
  * Touch screens
  * **KVM switches**
    - keyboard, video, mouse switch
    - hardware device that enables multiple computers to be viewed and controlled by single
     mouse, keyboard and screen and vice versa
    - can use single KVM switch to control multiple server systems
  * Game controllers and joysticks
  * **Digitizers**
    - pen tablet for digital drawing
    - also use for signature pads but replace by tablets
  * Multimedia devices, Digital cameras, Webcams
  * **Sound processors, speakers and microphones**
    - sampling: process of capturing sound waves in electronic format
    - sampling rate: measured in  units of thousands of cycles per second (KHz)
    - most sounds in computing are recorded from 11KHz to 192KHz
    - bit depth: number of characteristics of particular sound captured during sampling
    - 8-bit sample: 2^8 (256) characteristics, would sound like cheap recording
    - 16-bit sample: 2^16 (65,536) characteristics
    - single track (monoaural), two tracks (stereo)
    - CD quality: 44.1KHz with 16-bit depth and in stereo
    - PCM (pulse code modulation): grandaddy of all sound formats
    - WAV format: can be large when sampled at high frequency and depth
    - **codecs**
       + compressor/decompressor programs for compressing and discarding unnecessary audio
         qualities
       + MP3: MPEG-1 Layer 3 codec, AAC (advanced audio encoding)
       + MPEG-2 Part 2: for DVDs, broadcast TV
       + H.264: from smartphone video and streaming video to Blu-ray movies
       + H.265: half the size fo h.264 at same quality, used to support 4k
       + VP9: google's competitor to h.265 used in android devices and YouTube
    - streaming media: broadcast of data that is played and immediately discarded
    - **MIDI**
       + second processor of sound card to interpret standardized musical instrument digital
         interface (MIDI) files
       + not an independent music file like WAV
       + a text file that takes advantage of the sound-processing hardware to enable the
         computing device to produce sound
       + tiny in comparison to equivalent WAV files
       + hardware dependent
    - most motherboards support five or more speakers in discrete channels
    - subwoofer: provides amazing low-frequency sounds
    - all modern systems support both surround sound and subwoofer (Dolby Digital or DTS)
    - 2.1 system: two satellites and a subwoofer, 5.1: five satellites and a sub
    - **audio jacks**
       + main stereo: green, line in: blue, microphone: pink
       + main speaker out: standard speaker connector
       + line out: output sounds from the computer
       + line in: import sounds into computer
       + rear out: connect to rear speakers for surround sound output
       + analog/digital out: special digital connection to external digital devices and as
         analog connection to center and subwoofer channels
    - **SPDIF**
       + S/PDIF, Sony/Philips Digital Interface
       + comes in many sound processors to connect sound card directly to 5.1 speaker system
         or receiver
       + optical: square with a small door
       + coaxial: standard RCA connector
    - both HDMI and DisplayPort are capable of carrying audio
    - **container file or wrapper**
       + wrap the compressed tracks of video after each video and audio tracks are compressed
       + doesn't specify how the video or audio tracks were encoded
       + AVI (windows), MOV (Apple QuickTime), MP4 (for h.264 and h.265 video)

Storage devices
---------------
  * highly internetworked computers have reduced the need for removable media as a method of
    sharing programs and data
  * **flash memory**
    - **USB thumb drives**
       + don't need external power source as they are USB devices
       + nonvolatile flash memory is solid-state, shock resistant and retain data for decades
    - CF (CompactFlash): CF I (3.3 mm thick), CF II (5mm thick)
    - **SD (SecureDigital) cards**
       + miniSD, microSD
       + standard SD cards: 4MB to 4GB
       + SDHC (secure digital high capacity): from 4GB to 32GB
       + SDXC (secure digital extended capacity): 32GB to 2TB
       + first-gen cards use speed class (2, 4, 6 ,10), Class10 should write at minimum 10MB/s
       + second-gen: Ultra High Speed (UHS) bus, Class U1 should both read and write at
         minimum of 10MB/s, U3 cards should read and write at minimum of 30MB/s
       + third-gen: Video Speed Class, to support 4k and 8k, V6 at 6MB/s, V90 at 90MB/s
       + Application Performance Class ratings, A1 and A2 both a minimum of 10MB/s
         + A1 at 1500IOPS read and 500 IOPS when write
         + A2 at 4000IOPS read and 2000IOPS when write
         + IOPS doesn't matter while writing but make difference when multiple apps are using
    - **Extreme Digital (xD) Picture Cards**
       + half the size of SD card used in Olympus and Fujifilm cameras
       + original, Standard (Type M), and Hi-Speed (Type H)
    - XQD: high-speed transfers and capacities of 2+ TB, used in Nikon cameras, replaced by
     CFexpress which uses NVMe rather than PCIe
  * **optical discs**
    - from CD-ROMs (compact disc) to DVDs (digital versatile disc) and Blu-ray Discs
    - **CD**
       + store data by using microscopic pits burned into glass master CD with powerful laser
       + CDDA (CD-Digital Audio): lack advanced error checking, file support or directory
         structure
       + **CD-ROM**
         + created not to tied to proprietary formats like FAT or NTFS
         + divides CD into fixed sectors, each holding 2353bytes
         + CDFS (CD File System): most used format
         + original speed: 150KBps, same speed as CD-audio
         + each increase in speed is measured in multiples of original 150KBps
         + 1x 150KBps, 2x 300KBps, 24x 3600KBps, 72x 10800 KBps
         + require specialized, expensive equipment
       + **CD-R (CD-recordable)**
         + enable affrodable CD-R drives/burners
         + 74-minute disc with about 650MB, 80-minute disc with about 700MB
         + burner must be designed for 80-minute CD-R format
         + records data by using organic dyes embedded into the disc
         + both record speed and read speed expressed as multiples of 150KBps speed
         + 8x24x : burn at 8x and read at 24x
       + **CD-RW (CD-rewritable)**
         + enable to burn over existing data, colored bottom side
         + CD-RW drive works by using laser to heat an amorphous substance which is cooled to
           crystalline, which are relective areas
         + has three multiplier values: write, rewrite, read (8x4x32x)
    - **DVDs**
       + lowest capacity holds 4.37GB of data or two hours standard video
       + use smaller pits than CD
       + SS (single-sided), DS (double-sided)
       + SL (single-layer), DL (dual-layer)
       + DVD version               Capacity
       + DVD-5 (12cm, SS/SL)       4.37GB, more than 2hrs of video
       + DVD-9 (12cm, SS/DL)       7.95GB, about 2hrs of video
       + DVD-10 (12cm, DS/SL)      8.74GB, about 4.5hrs of video
       + DVD-18 (12cm, DS/DL)      15.90GB, more than 8hrs of video
       + DVD-ROM: up to almost 16GB, most drives sold with PCs are DVD-ROM drives
       + DVD-R: works like CD-R, can write but not erase what's written
       + DVD-RW: like CD-RW
       + DVD-RW DL: can be written on two layers, doubling the capacity
    - **Blu-ray Disc**
       + up to 25GB (SL), 50GB (DL), 100GB (BDXL)
       + popular until flash memory prices dropped in 2010
       + BD-R (recordable), BD-RE (rewritable)
    - most internal optical drives use SATA
    - external optical drives often use USB or Thunderbolt connections
`back to top <#a+top>`_

### 11. Building PC


Windows Editions
----------------
  * Home
  * Pro, Enterprise
    - can use Windows Domain/Active Directory for controlling network access and user accounts
    - offer much better control over files and folders with BitLocker and the Encrypting File
     System (EFS)
    - BranchCache: method for distributing apps to many locations in Enterprise edition
* CPUs for older sockets tend to cost more than CPUs for current sockets
* when choosing OS, think about customer's preferences to OS user interface
* end-of-life date: when the manufacturer no longer supports the hardware
* consider compatibility concerns between OS and hardware
* Operating systems don't always publish recommended requirements, just minimum

custom PC configurations
------------------------
  * **Standard Thick Clients**
    - runs modern OS and general productivity apps
    - has everything needed to work without a network connection
    - should meet or exceed the recommended hardware specs
  * **Thin Clients**
    - system designed to outsource much of its work, meet minimum requirements
    - usually rely on resources from powerful servers, may need network to boot
    - often serve as single-purpose systems, store only basic apps
    - might look like a thick client, but requires fewer resources
  * **Virtualization Workstation**
    - need lots of RAM, often maximum the system can accept
    - need fast CPU with as many cores as available
  * **Gaming PC**
    - GPU, fast, multicore processor, more RAM than thick client (at least 16GB)
    - high-definition sound card
    - high-end cooling system if possible
  * **Graphics/CAD/CAM Design Workstation**
    - high-end GPU, fast, multicore CPU, maximum RAM, serious storage space
    - large and high-quality monitor
  * **Audio Editing Workstation**
    - similar to those for graphics workstation
    - fast, multicore CPU, max RAM, large monitor, lots of fast storage
    - high-quality  **audio interface**
       + a high-end sound card, usually connect via USB rather than plugging into motherboard
       + to connect professional microphones and instruments
    - control surfaces: mimic the look and feel of analog mixing consoles
  * **Video Editing Workstations**
    - combine requirements of graphics and audio editing workstations
    - often use two or more color-calibrated monitors
    - powerful CPU wit as much RAM as possible
    - more intensive process than graphics or audio editing
    - multiple hard drives set up in RAID array for added storage capacity and enhanced
     read/write speed
    - custom keyboards with special labels and controls for editing software
  * **NAS (Network Attached Storage) devices**
    - for media streaming, file sharing, print sharing
    - need very fast network connection and large storage that needs to be fault tolerant
    - can use any modern OS
    - good amount of RAM and CPU
    - use two identical drives at minimum
    - must use RAID 1 at minimum
    - use four identical drives and RAID 10 if possible
*  many system builders add a small, hidden partition to the primary hard drive containing
  an image of factory-fresh version of OS
* clean installation and upgrade installation/in-place upgrade of OS

Boot Camp
---------
  * tool to install Windows on Apple machine
* Linux installers add multiboot capability by default (but the reverse is not true)

OS installation for large organizations
---------------------------------------
  * common method is to place the source files in a shared directory on a network server
  * **remote network installation**
    - connecting to the source location on the network and installing OS
    - unattended installation: automating with scripts
  * **image deployment**
    - **image**
       + complete copy of hard drive volume on which OS and any software preinstalled
       + can be stored on servers, optical discs or flash-media drives
    - Symantec Ghost Solution Suite, Clonezilla, Acronis True Image
    - dism.exe: Deployment Image Servicing and Management cli tool
* have to load drivers if installing Windows onto drives connected via RAID controller
* regional settings: language, time, currency, keyboard (Time/date,region/language setup)
* classic domain setup usually requires creating local admin account
* account setup: local user account, global account, organizational account, domain account

installing Windows on server
----------------------------
  * server usually Windows 7, 8, 8.1, 10 or Windows Server
  * on client side, need to use **PXE (preboot execution environment)**
    - use multiple protocols such as IP, DHCP and DNS to enable to boot from network location
    - need to configure BIOS for PXE boot
    - might need to change boot order to boot from network location
    - most NICs support PXE
    - can create boot media that forces PC to boot from a mapped network location

installing macOS over network
-----------------------------
  * **NetBoot**
    - tool for network installation and imaging
    - can boot a bunch of identical macOS machines remotely
    - any user-generated content will go away when machines reboot
    - can load identical images on multiple Macs, installing macOS on hard drives
    - can push specific apps to many computers at once

installation Errors
-------------------
  * media errors (eg. corrupt USB)
  * RAID array not detected (not having proper drivers for hard drive or RAID controller)
  * No boot device present (corrupt media or sytem not set to look for media)
  * graphical mode errors (hardware or driver problems, HAL, hardware abstraction layer)
  * Lockups during installation (don't give a tool as to what's causing problem)
  * disc, drive or image errors
  * Log files (Windows installation creates about 20 log files, try reset to default settings)
* patch, service packs, updates: corrective program for problems

USMT (User State Migration Tool)
--------------------------------
  * primary use in businesses as it has to be run in a Windows Server Active Directory Domain
  * to migrate large scale, many users

Windows Easy Transfer
---------------------
  * enables to migrate user data and personalizations quickly, best for a few users
  * not available in Windows 10

migration practices
-------------------
  * migrate users and data information in a secure environment
  * remove data remnants from hard drives
  * recycle older equipments

data destruction
----------------
  * Sanitizing: hard drive will still function once the data has been destroyed
  * low-level format
    - create physical marks on the disk surface so that the drive knew where to store data
    - hard drive manufacturers disabled the ability to perform low-level formats outside the
     factory
    - used to describe a zero-fill or overwrite operation
  * drive wiping utility: overwrite free space with junk data that makes original data hard to
    recover
* features to consider disabling on Windows
  * let apps use my advertising ID for experiences across apss
  * send Microsoft information about how I write to help us improve typing and writing
  * let websites provide locally relevant content by accessing my language list
  * location
  * getting to know you
`back to top <#a+top>`_

### 12. Windows


Registry
--------
  * huge database, system will not boot into Windows without it, *regedit* from command prompt
  * hives: registry files stored in \%SystemRoot%\System32\config folder and each user folder
  * organized in a tree structure
  * **subgroups or root keys**
    - HKEY_CLASSES_ROOT
       + defined standard class objects which are named groups of functions that defines what
         you can do with the object they represents
       + combines class objects from \Software\Classes under both HKEY_CURRENT_USER and
         HKEY_LOCAL_MACHINE to provide backward compatibility for older apps
    - HKEY_CURRENT_USER, HKEY_USERS
       + stores current user settings and all of the personalized information for each user
    - HKEY_LOCAL_MACHINE
       + contains all data for system's non-user-specific configs
    - HKEY_CURRENT_CONFIG
       + defines which value is currently used
       + almost never touch
  * **values**
    - String
    - Binary
    - DWORD: like binary but limited to exactly 32bits
    - QWORD: like binary but limited to exactly 64bits
  * always make a backup before changing
  * common manual registry edits is to delete autostarting programs
  * Registry Editor's Export feature enable to save either full Registry or only a single root
    key or subkey
  * *reg*
    - cli registry editing tool
    - can view registry keys and values, import and export, and even compare two different
     versions of a Registry
  * *regsvr32*
    - can modify registry in only one way, adding (or registering) dynamic link library (DLL)
     files as command components in the Registry
    - can cause problems if try to add 32bit DLL on with 64bit version of regsvr32, which is
     default
    - to edit 32bit DLL, run regsvr32.exe file in the %SystemRoot%Syswow64 folder

boot process
------------
  * BIOS: uses boot order to scan hard drive and look for MBR, which finds the boot code and
    call the bootmgr to launch the OS
  * UEFI: simply loads bootmgr directly
  * **bootmgr**
    - when starts, it reads data from a BCD (Boot Configuration Data) file
    - once an OS is selected, loads a program called winload.exe, which ready the system to
     load ntoskrnl.exe
    - by loading into memory hardware abstraction layer, system Registry and drivers
  * "bootmgr is missing" error shows when boot sector code cannot locate bootmgr
  * Windows logo comes up once the OS takes overs and lodas up all processes and systems

Processes
---------
  * when starting a program, Windows loads it into RAM as a process

Applications
------------
  * processes appearing in a window or full screen

Services
--------
  * run invisibly in background, providing a large number of necessary support roles

Task Manager
------------
  * can open with *ctrl-shift-esc*
  * **Windows 7**
    - Applications tab: use when trouble closing normally
    - Switch To: enables to bring any program to the front
    - New Task: enables to run programs if know the executable
    - a process is named after its executable file
    - all processes have a user name to identify who started it
    - all processes have a process identifier (PID), which is not shown by default
    - by default, only shows processes associated with the current user
    - Debug is grayed out, unless Windows debugger program is running
    - UAC Virtualization gives older programs that weren't written to avoid accessing protected
     folders a way to do so by making a fake protected folder
    - dump files show the status of program at the moment when clicked
    - setting any single process to Realtime priority will often slow the system
    - Set Affinity: enables to specify which PCU cores a process can run on
    - Task Manager doesn't show what processes depend on other processes
    - End Process Tree: ends not only the process but any process it depends on but will not kill
     a process tree for important system processes
    - Process Explorer: written by Mark Russinovitch shows process dependicies
    - Services: General tab provides name of the service, describes the service and enable to
     stop, start, pause or resume the service
    - **staring service from command prompt**
       + net start <service name>
    - Users tab: enables to log off any user (even current)
  * Networking tab has been merged into Performance tab
  * three new tabs
    - App history: collects recent statistics on CPU time and network usage
    - Startup: can also be used in *msconfig*
    - Details
       + list processes by executable name, and includes PID, status, the user running them,
         CPU/memory use, and a description
       + context menu also introduces a debugging option called *Analyze wait chain* to
         identify why a program is frozen
  * **Processes tab**
    - broken down into three sections: Apps, Background processes, Windows processes
    - by default, list a process description, status and resource use
  * advanced options moved to the context menus of the Details tab
  * Performance tab provides graph of overall CPU utilization by default
  * Users tab also shows the programs running under a user's account and indicates associated
    resourece use
  * **command-line utilities**
    - tasklist: enable to view running processes on a loca or remote system
    - taskkill: can kill a process using either the name or the PID
    - in PowerShell, can use *kill* command which is an alias for Stop-Process cmdlet

Resource Monitor
----------------
  * organize everything by PID number
  * tabs: CPU, Memory, Disk, Network
  * useful to get details of what process is using what resource and to close a buggy process

Perfomance Monitor
------------------
  * *perfmon.msc*, primary tool for tracking system resources overtime
  * requires an understanding of **objects and counters**
    - objects: system component that is given as set of characteristics and can be managed by
     the OS as single entity
    - counter: tracks specific info about an object
    - many counters can be associated with an object
  * **Data Collector Sets**
    - to track data long term and make reports
    - enable to choose counter objects to track and schedule when to run them

Component Services
------------------
  * COM, DCOM, COM+
  * enable to share data objects between apps or computers on a network to tweak programs
  * third-party apps can auto configure programs during the installation process

ODBC Data Source Administrator
------------------------------
  * **ODBC (Open Database Connectivity)**
    - coding standard that enable programmers to write databases and apps that use them in a
     way that they can query ODBC on how to locate and access a database without any concern
     about what app or OS is used
  * enables to create and manage entries called **Data Source Names (DSNs)**
    - points ODBC to a database
    - used by ODBC-awars apps to query ODBC to find their databases
  * rarely used unless making own shared databases
  * 64bit versions of Windows offer both 64bit and 32bit version of ODBC tool
`back to top <#a+top>`_

### 13. Groups & Permissions

* user accounts are also assigned to everything that runs programs
* Windows uses SYSTEM account when it runs programs

Authentication
--------------
  * process of identifying and granting access to some user / process of giving access a user
    access to a system
  * most commonly handled by a password-protected user account

Authorization
-------------
  * determines what an authenticated user can do to a system / process that defines what
    resources an authenticated user may access and what he may do with those resources
  * authorization for Windows' files and folders is controlled by the NTFS file system

User accounts
-------------
  * Windows call each record in the encrypted database a *local user account*
  * Linux: local user accounts rule
  * Windows & macOS: support local user accounts as well as Microsoft or Apple account
  * each user account gets unique personal folders

Passwords
---------
  * adding non-alphanumeric characters forces the hacker to consider more possible characters
    than just letters and numbers
  * most password crackers use a combination of common words and numbers to hack
  * enforce with a password expiration policy to force users to change password at regular
    intervals, but hard policy to maintain in the real world

Groups
------
  * a container that holds user accounts and defines the capabilities of its members
  * efficient way of managing multiple users
  * can assign a certain level of access for a file or folder to a group instead of single user
    account
  * Windows provide numerous built-in groups with various access levels already predetermined
  * Windows Home editions handle groups very differently than more advanced editions
  * **Administrators**
    - any account memeber of this group has complete admin privileges and grant complete
     control of machine
    - common for primary user to have the account in Admin group
  * **Power Users**
    - almost as powerful as members of Administrators but cannot install new devices or access
     other users' files or folders
  * **Users**
    - members of this group are called *standard users*
    - cannot edit the Registry or access critical system files
    - can create groups but only manage those they create
  * **Guests**
    - enables someone who does not have an account on the system to log on by using guest
     account
    - guest account remains disabled most often
* Windows machine will usually have single primary user account, a standard user and a local
  administrator account

Local Users and Groups
----------------------
  * tool to create, modify and delete users and groups, *lusrmgr.msc*
  * not available in Windows 10 Home edition
  * by default, all new user accounts are auto added to the Users group
  * good tool for dealing with local users and groups on a single Windows system

NTFS Permissions
----------------
  * authorization process use the NT File System (NTFS) as the primary tool
  * every file and folder on an NTFS partition has a list that contains **two sets of data**
    1. details every user and group that has access to that file or folder
    2. specifies the level of access that each user or group has to that file or folder
  * levle of access is defined by a set of restrictions called NTFS permissions
  * **Ownership**
    - creator is the owner of that file or folder
    - can change permissions to prevent anybody including administrators from accessing them
  * **Take Ownership permission**
    - anyone with the permission can seize control of file or folder
    - Administrator accounts have this permission for everything
    - admin blocked by Ownership permission can take away with this
  * **Change permission**
    - can give or take away permissions for other accounts
  * **Folder permissions**
    - define what a user may do to a folder
  * **File permissions**
    - define waht a user may do to an individual file
  * **standard NTFS permissions for a folder**
    - full control, modify, read & execute, list folder contents, read, write
  * **standard NTFS permissions for a file**
    - quite similar to folder permissions
    - full control, modify, read & execute, read, write
  * primary way to set NTFS permissions is through the Security tab under the Properties of
    a folder or file
  * it's considered a best practice to assign permissions to groups and then add user accounts
    to groups instead of adding permissions directly to individual user accounts
  * permissions are cumulative
  * creator of folder or file has complete control over it

Inheritance
-----------
  * determines which NTFS permissions any newly introduced files or subfolders contained in a
    folder receive
  * any new files or folders placed into a folder auto get all the NTFS permissions of the
    parent folder
  * all versions of Windows have inheritance on by default
  * deny checkbox always overrides the NTFS inheritance
  * grayed-out checkboxes can't be changed
  * insted of turning off inheritance completely, use the Deny checkbox

Permission propagation
----------------------
  * determines what NTFS permissions are applied to files that are moved or copied
  * copying creates two copies of the object, moving creates one copy of the object
    -          Same Volume                             Different Volume
    - Move     keeps original permissions              inherits new permissions
    - Copy     inherits new permissions                inherits new permissions
  * any object that is put on a FAT partition loses any permissions because FAT doesn't support
    NTFS permissions (including FAT32 or exFAT)
* have the admin create new temporary account that's a member of the Administrators group and
   make sure the admin deletes it after use
* never ask for the password to a permanent administrator account as it can backfire

permissions in Linux & macOS
----------------------------
  * Owner, Group, Everyone
  * **chown**
    - enables to change owner and group with which file or folder is associated
    - chown <new owner> filename
    - chown <owner>:<group> filename
  * **chmod**
    - change permissions
    - r (4), w (2), x (1)

sharing resources
-----------------
  * by using NTFS, Windows make private the folders and files in a specific user's personal
    folders
  * members of the Administrators group can override this behavior, but member of the Users
    group (standard users) cannot
  * files and folders can be shared through *the public libraries*, which are not visible by
    default on Windows later than 7
  * primary way to share resources on a single computer is to give users or groups NTFS
    permissions to specific folders and files
  * can also use *Sharing Wizard* that's less powerful but easier to use
  * **locating shared resources**
    - check for any unnecessary or unknown shared folders on the hard drives
    - **Computer Management console**
       + a tool in the Administrative Tools has a Shared Folders option under System Tools
       + Shares, Sessions, Open Files
  * shares added manually are called local shares
  * **administrative shares**
    - default shares
    - give local admins administrative access to shared resources, whether they log on
     locally or remotely
    - can delete them but Windows will auto re-create every reboot
    - won't affect the overall security

data encryption
---------------
  * scrambling of data through encryption techniques provides the only true way to secure data
    from access by any other user
  * Windows Home editions have basically no security features
  * **EFS (Encrypting File System)**
    - encryption scheme that any user can use to encrypt individual files or folders
    - Properties > General > Advanced
    - must have a valid password reset disk
    - if password is lost or an admin resets the password, you're locked out of encrypted files
     permanently
    - if computer dies, installing the hard drive in another system won't bring back data, even
     with identical user name (as security ID of that user account will differ)
    - encryption only stays if the file is copied to a drive with NTFS
  * **BitLocker Drive Encryption**
    - encrypts the whole drive, including every user's files, not dependent on any account
    - all data on the hard drive is safe even when stolen
    - requires special **TPM (Trusted Platform Module)** chip
       + validates on boot that the computer has not changed
       + also works in cases where the BitLocker drive is moved from one system to another
    - can use flash drive to store recovery key if TPM chip is not available
    - **BitLocker To Go**
       + enables to apply BitLocker encryption to removable dirves
       + applies encryption and password protection but doesn't require TPM chip

Security Policies
-----------------
  * rules applied to users and groups to do everything but NTFS permissions
  * Windows provides thousands of preset policies
  * **Local Security Policy**
    - tool to use policies, *secpol.msc*
    - has a number of containers that help organize the many types of policies on a system
    - each container has subcontainers or preset policies

UAC (User Account Control)
--------------------------
  * to stop unauthorized changes to Windows
  * UAC consent prompt, show as a pop-up dialog box
  * works for both standard user and administrator accounts
  * **four UAC levels**
    - Always notify
    - Don't notify when make changes to Windows settings
    - Notify only when apps/programs try to make changes to computer
    - Never notify
  * each of these options isn't a program, just a feature built into Windows
`back to top <#a+top>`_

### 14. Maintain & Optimize OS

* maintenance: jobs done time from time to keep the OS running well
* optimization: changes made to a system to make it better

patch management
----------------
  * process of keeping software updated in a safe and timely fashion
  * Windows update never touches the firmware (BIOS or UEFI) but macOS does if necessary
  * **Windows 7**
    - service pack: large bundle of updates
    - updates: individual fixes that come out often
       + Important: for security or stability issues, most critical
       + Recommended: added feature or enhancement, not critical
       + Optional: drivers, language packs and other nonessential updates, manual install
    - can choose which updates to download
    - can review installed updates by clicking View update history
  * **Windows 10**
    - major changes to Windows Update
    - cannot choose individual updates to download
    - individual users cannot turn off update, but may only pause updates for up to 35 days
    - Quality updates: classic patches, each update includes all changes from all previous
    - Feature updates
       + new versions of Windows, released in spring and fall
       + reinstall of Windows, named by year and month, marketing names (Creators update)
       + controlled by channel (Insiders, Semi-Annual, LTSB)
    - may uninstall some non-system updates

Disk Cleanup
------------
  * utility to clear junk files, Windows 10 uses *Storage sense*
  * first calculates the space that can be freed
  * will ask to clean up all the files on the computer or just current user
  * has a choice to compress old files

Registry maintenance
--------------------
  * registry tends to be clogged with entries that are no longer valid
  * need third-party utility to clean
  * all registry cleaners are risky

Error checking
--------------
  * for problems such as system freezing on shutdown, **chkdsk**

Disk Defragmenter or Optimize Drives
------------------------------------
  * reorganize files scattered into pieces on the hard drive into tight, linear complete files

Disk Utility
------------
  * for macOS, combines error checking with partitioning and other disk management tasks
* Linux disk maintenance: diagnostic tool on the installation media

Task Scheduler
--------------
  * **Windows**
    - divides taks into triggers, actions and conditions
    - used by many Windows utilities built-in scheduling options
  * macOS: *launchd* for automations
  * linux: *cron*

backups
-------
  * choose which files to backup and when the backups take place
  * **Backup and Restore (mainly Windows 7)**
    - Windows 7 will auto add a system image
  * **File History (Windows)**
    - requires a second drive and disabled by default
    - won't backup all personal files unless added to default libraries or create custom ones
    - does not replace full system backups
  * **Time Machine (macOS)**
    - create full system backups called local snapshots
    - also enables to restore deleted files and recover previous versions of files
    - requires external HDD or SSD or shared network drive
  * **Deja Dup (Ubuntu)**
    - backs up user's Home folder by default

System Configuration
--------------------
  * **msconfig**, to edit and troubleshoot OS and startup processes and services
  * General: select type of startup
  * Boot: advanced boot features
  * Services: similar to Services tab in Task Manager
  * Startup: to toggle startup programs
  * Tools: list tools such as Event Viewer, Performance Monitor, Command Prompt
* in macOS, can select or deselect any app that might load with specific user accounts

System Information
------------------
  * *msinfo32*
  * tool that collects info about hardware resources, components and software environment

MMC (microsoft management console)
----------------------------------
  * shell program that holds individual utilities called snap-ins
  * designed for administration and troubleshooting

Autorun
-------
  * a feature in Windows that enables the OS to look for and read a special file, autorun.inf
* in macOS
  * can install an app from AppStore or download and install from .dmg files
  * to remove, run the uninstaller if available or drag the app to the Trash but can leave
    various files on the system
* Performance Options
  * to configure CPU, RAM and virtual memory (page files) in Windows
  * This PC > Properties > Tasks list > Advanced system settings
  * three tabs: Visual Effects, Advanced, Data Execution Prevention
    - Advanced tab
       + processor scheduling and virtual memory
    - DEP
       + works in background to stop viruses and other malware from taking over programs
         loaded in system memory
       + only enabled for critical OS files in RAM

System Restore
--------------
  * tool that enables to create restore point, which is a snapshot of computer's config at a
    specific point of time
  * can make number of restore points automatically
  * This PC > Properties > Tasks list > System protection link
  * disabled by default in Windows 10
  * during the restore process, only settings and programs are changed, no data is lost
`back to top <#a+top>`_

### 15. CLI

* prompt: specific set of characters
* shell/cli: tool that interprets input
* every OS has the ability to interface with different types of shells
  * Linux/macOS: zsh, ksh, csh
  * Windows: PowerShell, Command
* every command prompt focuses on the working directory
* all operating systems manifest each program and data as individual files
* file format: unique method of binary organization
* one program cannot read another's files unless it can convert the other's file format

file's association/extension
----------------------------
  * tells the OS what type of program uses it's data
  * Windows GUI doesn't show file extensions by default but macOS and Linux do
  * file name and extension matter when working in cli
* path: exact location of a file

folders and file names
----------------------
  * Linux is case sensitive, Windows & macOS are not
  * may have spaces in names
  * **disallowed characters**
    - Windows: *".\[]:;|=,
    - macOS and Linux: slash /
  * files aren't required to have extensions, but the OS won't know the association type
* syntax: proper way to write a command
* no recycle bin or trash when deleting from command line

wildcard
--------
  - asterisk(*) or question mark(?)
  - every command that deals with files and folders will take wildcards
* pruning and grafting: copy or move pile of folders and files in one command

Windows
-------
  * *cmd*
  * uses backslash (\), uses drive letters, usually starts with C:
  * some commands give same result whether spaces included or not
  * ``[command name] /?``
  * ``dir``
    - lists the creation date, creation time, file size in bytes, filename, extension
    - scrollable: dir /p
    - only filenames: dir /w
  * ``cd``
  * move between drives: type the drive letter and a colon ``e:``
  * make directory: ``md`` or ``mkdir``
  * remove directory: ``rmdir`` or ``rd``
    - delete directory and subdirectories: rd /s
  * delete files
    - ``del`` or ``erase``
    - will output a response, will not remove dirs
  * copy or move: ``copy`` or ``move``, can work only in one directory at a time
  * ``xcopy``
    - similar to copy, but has extra switches
    - copy all subdirs except empty ones: xcopy /s
    - copy all empty subdirs: xcopy /e
  * ``robocopy``
    - add-on tool for Windows Server
    - copy files and folders from one computer to another across a network
    - syntax: robocopy [source] [destination] [options]
    - robocopy /mir
       + copy everthing from source and make the destination mirror
       + will delete anything in the destination that doesn't match the source
    - can copy encrypted files even if the admin account is denied access
    - wil resume copying after an interruption
  * ``chkdsk``
    - scans, detects and repairs file system issues and errors
    - fix file system-related errors: chkdsk /f
    - locate and repair bad sectors: chkdsk /r
    - needs direct acces (unlocked) to a drive
  * format volumes: ``format``
  * display computer name: ``hostname``
  * change group policies: ``gpupdate``
  * overview of all security policies applied to single user or computer: ``gpresult``
  * ``sfc /scannow``
    - System File Checker
    - scans, detects, and restores important Windows system files, folders and paths
    - finds issues and attempts to replace corrupted or missing files from cached DLLs
  * reboot: ``shutdown /r``
  * ``F1`` brings back previous command one letter at a time
  * ``F3`` brings back entire command
  * **PowerShell**
    - brings a series of powerful tools called *cmdlets*
    - Get-ChilItem

Linux/macOS
-----------
  * sudo command enables users to do root things without having the root password
  * when opening a program, Linux first look through the path, run with dot-slash (./program)
  * downloading and running command-line programs is not macOS thing
  * uses forward slash (/)
  * doesn't use drive letters, boot partition is defined as root drive, shown as slash (/)
  * ``man [command name]``
  * ``ls``
    - scrollable: ls | less
    - long list: ls -l
  * ``cd``
  * all media is mounted as a folder
  * make directory: ``mkdir``
  * remove empty directory: ``rmdir``
  * remove dir and subdirs: ``rm -r``
  * delete files: ``rm``
  * copy or move: ``cp`` or ``mv``
  * display computer name: ``hostname``
  * view and change network settings: ``ifconfig`` (considered deprecated) or ``ip``
  * change wireless settings: ``iwconfig``
  * terms for network connections
    - eth0, eth1, en0, en1: wired Ethernet NICs
    - wlan0, wlan1: wireless 802.11 NICs
    - lo: loopback
  * ``ps aux``
    - output running processes
    - USER: who is running the process, PID: process ID
    - %CPU: CPU power usage, %MEM: memory usage
    - VSZ: total paged memory in kilobytes
    - RSS: total physical memory in kilobytes
    - TTY: terminal the is taking the process's output
    - STAT: S = waiting, R = running, l = multithreaded, + = foreground process
    - START: process start time, TIME: process total running time
    - COMMAND: name of executable that created the process
  * search through text files or command output: ``grep``
  * package managers download and fully install and update software from single command
  * ``dd``
    - create an exact, bit-by-bit image of any form of block storage
    - dd if=<source block device> of=<destination image file location>
    - copying a hard drive: dd if=/dev/sda of=/dev/sdb
    - backup thumb drive: dd if=/dev/sdc of=/home/user/thumbBackup.bak
    - wiping a disk (by writing random bits): dd if=/dev/urandom of=/dev/sdb
  * ``shutdown now`` or ``shutdown -r``
  * change password: ``passwd``
`back to top <#a+top>`_

### 16. Troubleshooting OS

* when computer fails to boot, determine whether the problem relates to hardware or software
* "Operating System not found"
  * hardware problem error message
  * improper connectivity or CMOS error
  * system hasn't even started booting
* "BOOTMGR is missing"
  * error after POST complete and trying to boot to an OS
* hardware problems can give a blank screen on boot-up
* "No boot device detected"
  * boot to an incorrect device

WinPE or WindowsPE (Windows Preinstallation Environment)
--------------------------------------------------------
  * 32 or 64bit installation environment, allow GUI support not like 16bit installation in DOS
  * boot directly to the Windows media

WinRE (Windows Recovery Environment)
------------------------------------
  * also called System Recovery Options menu
  * repair tools that run within WinPE
  * accessing WinRE: boot from Windows installation media and select Repair
  * can use the Repair Your Computer option on the Advanced Boot Options (f8) menu, not
    available by default in Windows 8/8.1/10 (instead can create a recovery drive on a 16GB+
    drive by accessing the Recovery applet in Control Panel)
  * can create a system repair disc or system image
  * reasons to access WinRE from Windows installation media
    - hard drive can be messed up that it won't make it to the Advanced Boot Options menu
    - accessing WinRE using the Repair Your Computer option requires local admin password
    - using bootable disc/drive enables to avoid any malware that might be on the system

Startup Repair
--------------
  * repair corrupted Registry by accessing the backup copy
  * restores critical boot files
  * restores critical system and driver files
  * rolls back any non-working drivers
  * rolls back updates
  * runs chkdsk, runs a memory test to check RAM
  * works well if system is only on one hard drive with single partition
  * starts auto if the system detects a boot problem

System Restore
--------------
  * can go back to proper working time by restore points
  * disabled in Windows 10 by default

Force Quit
----------
  * for application problems in macOS
  * OPTION-COMMAND-ESC

System Image Recovery
---------------------
  * great tool for a set of uniform Windows 7 systems, typical in many workplaces
  * use third-party tools for Windows 10, macOS and Linux

Windows Memory Diagnostic
-------------------------
  * to test the system RAM
  * doesn't show up as an option in WinRE in Windows 8/8.1/10, access from Control Panel
  * *Basic*: runs quickly about one minute but performs only light testing
  * *Standard*: default choice, takes a few minutes and tests more aggressively
  * *Extended*: takes hours, runs aggressively
  * *Cache*: enables to set whether the tests use the CPU's built-in cache as well as override
    the default cache settings for each test type
  * *Pass Count*: sets the number of times each set of tests will run
  * can view results from Event Viewer
* WinRE command prompt is true 32 or 64bit prompt that functions similarly to regular cmd.exe
  , but lacks a large number of tools, Startup Repair tool runs many of the command-prompt
  utilities automatically

bootrec
-------
  * repairs the master boot record, boot sector or BCD store
  * ``bootrec /fixboot``: rebuilds the boot sectore for active system partition
  * ``bootrec /fixmbr``: rebuilds the master boot record for the system partition
  * ``bootrec /scanos``: looks for Windows installations not currently in the BCD store and show
    shows the results
  * ``bootrec /rebuildmbr``: looks for Windows installations not currently in the BCD store and
    gives the choice to add them to BCD store

bcdedit
-------
  * cli tool to see how Windows boot and edit boot order options
  * *Windows Boot Manager*: section that describes the location of bootmgr
  * *Windows Boot Loader*: section that describes the location of the winload.exe
  * ``bcdedit /export <filename>``: exports copy of the BCD store
  * ``bcdedit /import <filename>``: imports copy of BCD store back
  * ``bcdedit /set {current} path <new winload path>``: changes path of winload
  * supports multiple OSs
  * {ntldr}: identifier that points to a very old OS

diskpart
--------
  * full featured cli partitioning tool
  * lacks safety features built into Disk Management
  * ``list volume``: list volumes or partitions
  * ``select volume 1``: select volume to manipulate
  * can add, change or delete volumes and partitions
  * can mount or dismount volumes
  * can manipulate software-level RAID arrays
  * ``format``: to format newly created volume
  * ``clean``: wipe all partition and volume info off the currently selected disk

fsutil
------
  * cli tool for handling file systems
  * ``fsinfo``: provides detailed query about the drives and volumes
  * ``fsutil dirty <drive name>``: check if Windows consider the drive to be diry or not, may
    need to run *autochk* at the next boot if dirty
  * ``fsutil repair initiate <drive letter>``: runs basic version of chkdsk without rebooting

other options for Windows
-------------------------
  * **Uninstall Updates**
    - uninstall updates that broke the system
  * **UEFI Firmware Settings**
    - to tweak CPU or RAM timings, or change boot order
  * **Refresh and Reset**
    - Refresh in Windows 8/8.1 is equal to Keep my files in Windows 10
    - Reset in Windows 8/8.1 is equal to Remove everything in Windows 10
    - Refresh: rebuilds Windows but keep all user files and settings and any purchased apps
    - Reset: wipe all apps, programs, user files, settings and gives fresh Windows
    - use as last resort when troubleshooting

Linux boot failure
------------------
  * boot managers: GRUB, LILO (older one and doesn't support UEFI BIOS systems)
  * if GRUB corrupt or deleted, Linux won't start, will get "missing GRUB" error
  * run ``sudo grub-install`` from installatin media command line

failure to start normally
-------------------------
  * Windows can hang because of buggy device drivers, Registry problems, autoloading programs
  * corrupted user profile can block normal login
  * malware can cause Windows to fail to start normally or make it appear to be missing
  * use one of the Advanced Startup Options

device drivers
--------------
  * BSoD oly appears when something causes error that Windows cannot recover from
  * problems due to device drivers almost always take place immediately
  * buggy video drivers can result in black screen
  * Windows freeze on starup screen
    - use one of the Advanced Startup Options, covered following the Registry

kernel panic
------------
  * form of BSoD on Linux systems
  * solution is to find updated drivers or kernel modules
  * kernel panic in macOS is black or gray screen (SPoD indicates unresponsive system)

Registry
--------
  * errors may show as BSoDs that say "Registry File Failure" or "Windows could not start"
  * keep regular backup of the Registry
  * can find backup files in \Windows\System32\config\RegBack

Advanced Startup Options
------------------------
  * press f5 at boot up in Windows 7 or use Windows Advanced Boot Options
  * **Safe Mode**
    - start Windows but loads only basic, non-vendor-specific drivers
    - can use tools such as Device Manager
    - can access properties for all devices, even those that are not working in Safe Mode
    - status displayed for devices is the status for normal startup
    - can disalbe suspect device or perform other tasks such as remove or update driver
    - no safety or repair feature in any version of Windows that make the OS boot to Safe
     Mode automatically, must set the System Configuration utility to force it
  * **Safe Mode with Networking**
    - identical to plain Safe Mode but get network support
    - to test network drivers
  * **Safe Mode with Command Prompt**
    - loads cmd.exe rather than GUI Desktop
    - defrag, cli version of Disk Defragmenter, runs faster than GUI version
    - can defrag an HDD from the command prompt
    - use when the Desktop does not display at all
  * **Enable Boot Logging**
    - starts Windows normally and creates a log files of drivers as they load into memory
    - file is named Ntbtlog.txt in %SystemRoot% folder
    - last entry in the file may be the error driver
  * **Enable Low-Resolution Video**
    - starts Windows normally but only loads a default video driver
  * **Last Known Good Configuration**
    - applies specifically to new device drivers that cause failures to boot
  * **Directory Services Restore Mode**
    - applies to Active Directory domain controllers in Windows Server
  * **Debugging Mode**
    - starts in kernel debug mode
    - can connect to another computer via a serial connection that is running debuggin tool
  * **Disable Automatic Restart on System Failure**
    - stops the computer from rebooting on Stop errors
    - can check erros after stop
  * **Disable Driver Signature Enforcement**
    - Windows require that all low-level drivers (kernel drivers) must have a Microsoft driver
     signature
    - can use this option if using older driver to connect the hard driver controller or other
     low-level feature

Rebuild Windows Profile
-----------------------
  * corrupted file can block a user from loggin in and also cause slow profile load times
  * anit-malware software can sometimes corrupt a profile
  * go into Safe Mode and access cmd
  * use ``regedit`` to open Registry Editor
  * go to "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\NT\CurrentVersion\ProfilesList"
  * look for entries that start with S-1-5
  * look for user name value in "ProfileImagePath" on the right pane and set that value to 0
  * set value of "RefCount" to 0 if available
  * to create new user account
    - need an admin account
    - use "net user administrator \active:yes" and reboot and login as local admin
    - create new user account from Settings > Accounts and delete old account after reboot

syslog
------
  * tool for UNIX systems to create info placed in log files
  * third-party support for syslog Windows

Event Viewer
------------
  * Critical, Error, Warning, Information, Audit Success, Audit Failure
  * by default, logs file is stored as .evtx files in C:\Windows\System32\winevt\Logs
  * logs in Windows have limitations
  * only users with Administrator privileges can make changes to log files in Event Viewer

services
--------
  * Windows will not report all service failures
  * if less than critical service, Windows doesn't start until a program that need that
    service is used
  * service Startup types: Automatic, Manual, Disabled

system files
------------
  * Windows lives on dynamic link library (DLL) files
  * use sfc to check and replace a number of critical files, including DLL cache
* Troubleshooting applet in Windows: Programs, Hardware and Sound, Network and Internet,
  System and Security

Security and Maintenance / Action Center
----------------------------------------
  * aggregation of event messages, warnings and maintenance messages
  * only compiles info, taking data from well-known utilities such as Event Viewer, Windows
    Update, Windows Firewall and UAC
* corrupted applications can corrupt data
* when Windows clock resets, so do the BIOS time and settings

autorun.inf
-----------
  * Windows auto look for this file when installing app or media is inserted
  * can cause security issue
  * could put a malicious program on some form of media and write an autorun.inf file to point
    to the malware
* a well-behave program should always make itself easy to uninstall
* .NET: extension to the Windows OS that includes support for a number of features (powerful
  interface tools and flexible database access)

compatibility
-------------
  * Windows handles compatibility using the Compatibility tab in Properties dialog box
  * Reduced color mode: for older programs designed to run in 256 colors
  * Run in 640x480 screen resolution: for badly written older programs that assume the screen
    be at 640x480
  * Disable desktop composition (Windows 7): disables all display features such as Aero
  * Disable display scaling on high DPI settings: turn of auto resizing program windows
  * Run this program as an administrator
  * Enable this program to work with OneDrive files (Windows 8/8.1/10): provides network
    support for older apps that might not understand cloud file storage
  * Change settings for all users: apply compatibility changes made to a program for all users
* Repair app: fix the program but leaves personal data
* Reset app: remove any customizations and restore ot factory defaults

application crashes
-------------------
  * CTD: a crash to desktop
  * can lock up computer or unexpected shudown
  * can be caused by hardware driver problems, not just app problems

System Protection
-----------------
  * feature to access previous versions of any data file or folder
  * powered by Volume Shadow Copy Service (VSS), which enables the OS to make backups of any
    file, even one that is in use
  * great tool to recover previous versions of files that users accidently overwrite
  * also enables to load a restore point and to create restore points manually
  * falls into the category called file recovery software
`back to top <#a+top>`_

### 17. Display Technologies

* video card or display adapter handles all the communication between CPU and the display
* operating system needs to know how ot handle communication between the CPU and the display
  adapter

Video displays
--------------
  * flat-panel monitors, projectors, virtual reality headsets
  * PCs and Macs originally used a CRT displayed that had toxic materials inside

LCDs
----
  * liquid crystal displays
  * light waves emanate from light source in three dimensions
  * when light flows through a polarized filter, only waves of similar orientation can be seen
  * many sunglasses use polarizing filters
  * when two polarizing filters are combined at a 90-degree angle, no light passes through
  * the third filter, at 45-degree angle, twists some of the light so that it gets through
  * liquid crystals take advantage of the propery of polarization
  * the crystals act like liquid polarized filter
  * if two perpendicular polarizing filters are placed on either side of the liquid crystal,
    it will twist the light and enable it to pass
  * if the liquid crystal is exposed to electrical potential,the crystals will change their
    orientation to amtch the direction of the electrical field
  * a color LCD screen is composed of large number of tiny liquid crystal molecules, subpixels,
    arranged in row and columns between polarizing filters
  * physical pixel: each tiny distinct group of three subpixels (red, green, blue)
  * **early LCDs**
    - did not use rectangular pixels
    - use static charging, where each area as charged at the same time to create an image,
     which is still used in displays such as calculators
    - use a matrix of wires, vertical wires (Y wires) for every sub-pixel in column,
     horizontal wires (X wires) for row sub-pixels
    - there had to be a charge on both X and Y wires to make enough voltage to light a single
     sub-pixel
  * passive matrix
    - three matrices intersected very close together, above which the glass was covered with
     tiny red, green and blue dots
    - varying voltage of wires made different levels of red, green, blue, creating colors
  * thin film transistor (TFT) or active matrix: one or more tiny transistors control each
    color dot, used by current LCDs
  * **components**
    - LCD panel: creates image
    - backlight: illuminate the image
    - inverters: send power to backlights that need AC
  * **LCD panels**
    - twisted nematic (TN), in-plane switching (IPS), vertical alignment (VA)
    -Plane to LIne Switching (PLS), proprietary IPS version by Samsung
  * **backlights**
    - current LCDs use light-emitting diode (LED) technology
    - older generations used CCFL technology
    - LED uses DC, like the logic boards and panels in LCDs, consume much less electricity
     than CCFL, and give off no heat
    - edge LED backlighting: can sometimes see the edges are brighter than the center
    - direct LED backlighting: puts a bank of LEDs behind the panel, more expensive and use
     more electricity than edge LED backlighting
    - **CCFL**
       + cold cathode fluorescent lamp used by early LCDs
       + low power use, even brightness and long life
       + needed high frequency AC and required the use of an inverter
  * **Resolution**
    - describe the number of pixels on a display
    - LCD monitors are designed to run at a single native resolution
    - interpolation: edge-blurring techique used in LCD to soften the jagged corners of the
     pixels when running at lower than native resolution
    - aspect ratio: number of pixels arranged on the screen
    - **pixels per inch (PPI)**
       + combination of the resolution and physical size of a display
       + smaller, high-resolution monitor will look better than much larger monitor with same
         resolution
  * **Brightness**
    - strength of LCD monitor's backlight, measured in nits
    - average LCD panels are around 300 nits
    - 1 nit = 1 candela/m^2, 1 candela is equal to the amount of light created by candle
  * **Viewing angle**
    - LCD panels have limited viewing angle, the screen fades out when viewed from the side
    - TN panels have narrow viewing angle, as little as 70 degrees from center line
    - IPS monitors, roughly 178 degrees
  * **Response rate**
    - amount of time it takes for all of the sub-pixels on the panel to change from one state
     to another
    - measured in milliseconds (ms), the lower the better
    - black-to-white (BtW): how long it takes the pixels to go from pure black to puer white
     and balck again
    - gray-to-gray (GtG): how long it takes the pixels to go from one gray state to another
    - GtG time will always be faster than BtW time
  * **Refresh rate**
    - how often a screen can change or update completely
    - stand motion pictures have 24 times per second, which humans see things change
    - faster make the movement smoother with less lag
  * **Contrast ratio**
    - difference betweent the darkest and lightest spots that the monitor can display
    - early LCDs could not produce the color saturation or richness of contrast of a CRT
    - good ratio 450:1, range from 250:1 to 1000:1
    - dynamic contrast ratio number: measures the difference between a full-on, all-white
     screen and a full-of, or all-black screen
    - dyanmic contrast ratio doesn't affect viewing on computer monitors
  * **Color depth**
    - amount of colors LCD panels can display
    - measured in the bit-depth of the panel
    - most monitors today use an 8-bit panel, with 256 (2^8) colors per channel
    - hight depth is apparent for photo editing or video color grading
  * majority of users will go with an IPS panel with an edge LCD backlight
  * TN panels are least expensive, but gaming TN monitors are expensive

Projectos
---------
  * generate an image in one device and then use light to project it onto a screen
  * front-view projectos shoot an image out the front and a screen must be placed at proper
    distance
  * **CRT projectors**
    - used in firt generation
    - created beautiful images but were expensive, large and very heavy
  * **LCD projectors**
    - natural fit for front projection
    - light and very inexpensive compared to CRTs but lack the image quality
  * **DLP projectors**
    - Digital Light Processing technology from Texas Instruments
    - offer a softer image than LCD
    - use more electricity but not as heavy as LCD projectors
  * **Lumens**
    - amount of light given off by a light source from a certain angle that is perceived by
     the human eye
    - the greater lumen rating, the brighter the projector
    - 1000 to 1500 lumens will work well in small, darkened room
  * **Throw**
    - size of the image at a certain distance from the screen
    - expressed in terms of the distance required to project a 100inch diagonal screen
    - standard-throw projector needs to be 11 to 12 feet away to display 100inch image
    - short-throw projector needs to be 4 feet away
    - ultra-short-throw projector needs to be 15 inches away
  * **Lamps**
    - generate tremendous amount of light
    - projectors come with a fan to keep the lamp from overheating
    - the fan continues to run until the lamp if fully cooled when projector is turned off
    - lamp technologies: metal halide, LED or lasers
    - metal halide
       + has been the standard for many years
       + produce huge amount of lumens in a small form factor
       + excessive heat and fan noise, life span of ~3000 hours
    - LED-based projectors
       + use red, green and blue LEDs to provide light
       +require smaller and quieter fans as LEDs don't heat up
       + require much darker room, life span of 20,000+ hours
    - laser-based projectors
       + come from white lasers hitting color wheels to colored lasers
       + produce vibrant, high-contrast images and very little heat
       + can last 30,000+ hours

VR headsets
-----------
  * create immersive experience by mounting two high-resolution screens into a headset that
    block external visual sensory input
  * current VR headsets use organic light-emitting diode (OLED) or active matric OLED (AMOLED)
    with added transistors
  * OLED screens use organic compounds between the glass layers that light up when given an
    electrical charge, requires no backlight, can turn off completely

Monitor connections
-------------------
  * VGA
    - also known as D-shell and D-subminiature
    - oldest and least-capable type
  * DVI
    - digital visual interface
    - three connectors that look very much alike: DVI-D for digital, DVI-A for analog, DVI-A/D
     or DVI-I (interchangeable) for DVI-D or DVI-A
    - single-link DVI: maximum bandwidth of 165MHz, max resolution of 1920x1080 at 60HZ or
     1280x1024 at 85Hz
    - dual-link DVI: uses more pins to double throughput, res up to 2048x1536 at 60Hz
  * HDMI
    - high definition multimedia interface, carries both HD video and audio signals
    - a lot of smallr devices hve Mini-HDMI ports
  * DisplayPort, Thunderbolt
    - both support full-HD video and audio
    - Thunderbolt 1 and 2 uses Mini DisplayPort (mDP)
    - Thunderbolt 3 uses the USB Type-C connector
  * HDBaseT
    - enables long-range connectivity for uncompressed HD video and audio over Cat 5a or Cat
     6 network cables
    - can use HDBaseT receiver to convert for projector
* OSD (onscreen display) menu enable a number of adjustments for monitor

VESA Mounts
-----------
  * standardized bracket option for mounting the monitor on the wall or on a special stand
  * also called Flat Display Mounting Interface (FDMI) or VESA Mounting Interface Standard (MIS)
  * curved panels do not have VESA mount options

Display adapters
----------------
  * process information from the CPU and send it to the display
  * graphic processor needs its own RAM, fast connectivity between the CPU, and system RAM
  * **PCI**
    - Peripheral Component Interconnect, oldest display adapter connector type
    - limited to 32bit transfers at roughly 33MHz
    - cannot handle the video needs of current systems
  * **AGP**
    - Accelerated Graphics Port, not used anymore
    - single, special port, similar to a PCI slot, dedicated to video
    - no motherboard had two AGP slots
  * **PCIe**
    - PCI Express
    - all PCIe video cards use the PCIe x16 connector

Graphics Processor
------------------
  * handles the heavy lifting of taking commands from the CPU and translating them into
    coordinates and color info that the monitor understands and displays
  * MSI GeForce GTX 1080ti 11GB 384bit GDDR5X PCI Express 3.0
    - MSI: manufacturer
    - GeForce GTX 1080ti: graphics processor
    - 11GB 384bit GDDR5X: dedicated video RAM and connection between vido RAM and graphics
     processor
    - PCI Express 3.0: motherboard expansion slot the card requires
  * **HDCP**
    - High-bandwidth Digital Content Protection, digital anti-theft technology
    - stops audio and video copying between high-speed connections such as HDMI, DP and DVI
    - also stops playback of HDCP-encrypted content on devices designed to circumvent system
  * **Video Memory**
    - video RAM constantly updates to reflect every change that takes place on the screen
    - most of the graphics rendering and processing is handled on the card by the video
     processor chip rather than by the CPU
    - video RAM can read and write data at the same time, not like DRAM
    - Acronym  Name                                    Purpose
    - DDR3     Double Data Rate SDRAM version 3        budget graphics cards, laptop video cards
    - GDDR3    Graphics Double Data Rate version 3     similar to DDR3, but runs at faster speed
                                                       different cooling requirements
    - GDDR4    Graphics Double Data Rate version 4     upgrade of GDDR3, faster clock
    - GDDR5    Graphics Double Data Rate version 5     successor of GDDR4, double the
                                                       input/output rate of GDDR4
    - GDDR5X    Graphics Double Data Rate version 5X   successor of GDDR5
    - GDDR6    Graphics Double Data Rate version 6     successor of GDDR5X
    - HBM      High Bandwidth Memory                   competitor of GDDR5
    - HBM2      High Bandwidth Memory version2         successor to HBM, competitor of GDDR6
    - HBM2 offers very different style of memory from DDR or GDDR by using stacked DRAM
     connected via super-wide buses
    - GDDR has 32bit bush width, HBM 1024bit bus
    - huge amount of video RAM enables game developers to optimize their games and store more
     essential data on the local video RAM
  * **Integrated GPUs**
    - onboard video isn't very powerful, but common in laptops as it save space and power
    - Intel Grpahics Media Accelerator (GMA), AMD's Fusion processors
    - Intel's graphics support is gared to desktop performace, not gaming at all
    - NVIDIA's Tegra line is focused on gaming (Nintendo Switch) and automotive entertainment
     systems
    - connector types: VGA, DVI, HDMI (include mini & micro), DisplayPort (include full * mini
     ), Thunderbolt (include Thunderbolt 1 & 2, USB Type-C Thunderbolt 3)
    - connector types for interfacing with other media devices such as camcorders, projectors,
     television sets
  * issues when installing cards: long cards, proximity of nearest expansion card, presence of
    power connectors
  * many high-end video cards come as double-wide cards with built-in air vents
  * mird-range to high-end cards typicall require at least one additional PCIe power connector
  * always avoid using the built-in Windows driver as it tends to be the most dated

3D graphics
-----------
  * before early 1990s, PCs did not mix well with 3D graphics
  * 3D graphics apps would run only on expensive, specialized hardware
  * big change took in 1992 when id Software created Wolfenstein 3D, as it introduced FPS genre
  * **sprites**
    - used by early 3D games
    - fixed 2D images to create the 3D world
    - games would calculate the position of an object from player's perspective and place a
     sprite to represent the object
  * 2nd generation of 3D games began to replace sprites with **true 3D objects**
    - composed of a group of points called vertices
    - transformation: computer tracking all the vertices of all the objects in the 3D world
     including the one player cannot see
    - filling in 3D object begins by drawing lines/edges between vertices to construct the 3D
     object from many triangles (triangles make the most sense from mathematical standpoint)
    - 3D process groups triangles into various shapes called polygons
    - originally, CPU handled calculations to create triangles
    - with the introduction of GeForce 256 in 199, transform process was moved from CPU to the
     video card
  * **textures**
    - image files stored by every 3D game
    - program wraps textures around an object to give it a surface
    - work well to provide detail without using a lot of triangles
    - Intel gave AGP the ability to read system RAM to support textures
  * to create realistic movement, 3D world must refresh at least 24 times per second
  * video cards were developed with smart onboard GPUs

APIs
----
  * application programming interfaces
  * library of commands that people who make 3D games must use in their programs
  * device drivers for each card translate the API commands into instructions the hardware
    on that card will understand
  * OpenGL: for UNIX systems but been ported or compatible with other systems
  * DirectX: from Microsoft
  * Vulcan: open source
  * Metal API: from Apple
  * every API handles things differently
  * in some games, OpenGL standard might produce more precise images with less CPU overhead
    than DirectX
  * **DirectX**
    - in old days, many apps communicated directly with much of the PC hardware and it could
     crash the system
    - programmers use DirectX to control certain pieces of hardware and to talk directly to it
    - primary reason was to build a series of products to enable Windows to run 3D games
    - *dxdiag*, DirectX Diagnostic Tool to check DirectX info

Drivers
-------
  * majority of video card/driver problems are bad or incompatible drivers or incorrect settings
  * incompatible drive might cause to get BSoD
  * system with suddenly corrupted driver usually doesn't act up until next reboot
  * when boot with corrupted driver, Windows will go into SVGA mode, blank the monitor, lock
    up, or display screen with weird patterns, incorrect color patterns or distorted image
  * reboot into Safe mode and roll back or delete the driver
  * current Windows versions will go in 800x600 (SVGA mode)
* video cards fan and RAM are two components that are not durable and can break
* overheating might cause the computer to shut down

most common monitor problems
----------------------------
  * ghosting, streaking, fuzzy vertical edges
    - check cable and connections
    - caused by video cards rather than monitors
  * color missing
    - check cables, check front controls or internal driver settings or services
  * losing brightness
    - can happend as monitor ages
    - will require internal adjustment
    - use power-management functions
  * bad pixels
    - pixel that does not react the way it should
    - dead pixel: pixel that never lights up
    - lit pixel: pixel stuck on pure white
    - stuck pixel: pixel stuck on certain color
    - replace monitor, use warranty if available
    - all LCD panel makers allow certain number of bad pixels, even on brand new
  * LCD cracks
    - not reapirable and must replace monitor
  * flicerking image
    - happen in inexpensive panel with too much light bleed from backlight or dying CCFL
    - LEDs don't flicker
    - replcae backlight or monitor
  * dim image
    - usually on top or bottom half of the screen
    - dead or dying backlight
  * LCD dark
    - backlight or inverter issues
    - usually in super-thin panels
    - replace panel and backlight as a unit
  * LCD making hissing noise
    - failing inverter
  * image persistence
    - image leaving shadow after displayed for a long time
    - temporary and should go away after turning off display
    - can be permanent burn-in
* high resolution monitors offer beautiful visuals but can make screen elements smaller

cleaning monitors
-----------------
  * always use antistatic monitor wipes or at least a general antistatic cloth
  * never use window cleaners or any liquid that contain ammonia
* video cards can handle multiple monitors that differ from one another in size or resolution
  but can create problems
* extending a display can cause alignment errors
* privacy screens
  * stop wide-angle viewing of the screen and drop glare caused by external object reflection

Troubleshooting projectors
--------------------------
  * let projector cool off as lamps can produce a lot of heat
  * lamps have relatively short life, so keep a spare if possible
  * always keep extra batteries for remotes
  * check fan and filter if overheat
  * projectors might go to sleep and need to reboot
  * if image has incorrect color patterns, LCD panels might be broken
  * strange or flicker color on DLP projector might indicate failing color wheel

MicroLED
--------
  * microLED monitors form pixels using groups of microscopic LEDs
  * better energy efficiency, brightness, contrast and response time than traditional LCD

High Dyanmic Range
------------------
  * color spaces or chromaticity diagrams
    - variety of colors, luminance and contrast that human eye can see
  * computers display a fraction of what people can see and thus use more limited color spaces
  * SDR: Standard Dynamic Range used by standard 8bit panel
  * HDR: High Dynamic Range used by 10bit panel

Adaptive Sync
-------------
  * tearing
    - different parts of the screen may showing different frames when the display and
     graphics card are out of sync
  * V-sync
    - avoids tearing by using fixed refresh cycle, but higher latency
  * adaptive sync
    - enable display to synchronize its refresh rate with the graphics card's
    - G-Sync from Nvidia, FreeSync from AMD
    - minimal corss-compatibility
    - graphics card and moitor either both need to support

Display Modes
-------------
  * VGA:         640x480  , 4:3  , ancient monitors
  * SVGA:        800x600  , 4:3  , ancient monitors
  * HDTV 720p:   1280x720 , 16:9 , lowest resolution that can be called HDTV
  * HDTV 1080p:  1920x1080, 16:9 , full HDTV resolution
  * WUXGA:       1920x1200, 16:10, older widescreen monitors
  * WQHD (2K):   2560x1440, 16:9 , widescreen displays
  * 4K Ultra HD: 3840x2160, 16:9 , televisions, excellent monitors
  * 5K:          5120x2880, 16:9 , excellent monitors
  * 8K Ultra HD: 7680x4320, 16:9 , televisions

eGPUs
-----
  * external GPUs, standalone boxes with video cards for video processing and gaming
`back to top <#a+top>`_

### 18. Networking

* host: any computing device connected to a network
* server: any computer running a sharing program
* what can be shared and accessed is limited only by the ability to find the server program
  capable of sharing it and a client program that can access it

basic essentials
----------------
  * something that defines and standardizes the design and operation
  * addressing method that enables clients to find servers and enables servers to send data
  * some method of sharing and accessing shared resources
* both clients and servers need *network interface controllers (NIC)*
* before, every NIC came on an expansion card that can be added to a motherboard

MAC (media access control) address
----------------------------------
  * every NIC's built-in identifier
  * 48 bits long, represented by using 12 hexadecimal characters
  * some NICs allow to change the MAC address on the NIC

Frames
------
  * data is moved from one device to another in discrete chunks called frames
  * contain MAc address of the sender and receiver
  * most frames use a mathematical algorithm called a *CRC (cyclic redundancy check)*

Ethernet
--------
  * series of standards that defined everything necessary to get data from one computer to
    another
  * protocol developed for wired networking, but even wireless networks use as the basis
  * use a **star bus** topology
    - every individual host connects to a central box
    - the star refers to the wires leading from the box to the hosts
    - the switch (central box) provides common point of connection for devices
    - most consumer-level switches have 4 or 8 ports, but business-level can have 32 or more
    - early Ethernet used a hub (stupid repeaters, anything sent went to all other devices)
    - switches memorize the MAC addresses of all connected devices and only send out repeated
     signals to the correct host
    - segment: connection between computer and a switch
    - Ethernet segments are limited to 100m or less, cannot use a splitter to split
    - no hosts connected to a split segment will be able to communicate
  * 10BaseT (10Mbps)
    - requires 2 pairs of wires for sending and receiving
    - ran on Cat 3
  * 100BaseT (100Mbps)
    - requires 2 pairs of wires for sending and receiving
    - requires at least Cat 5
  * 1000BaseT (1000Mbps or 1Gbps)
    - needs all four pairs of wires in Cat 5e or higher
  * 10Gigabit Ethernet is common on server-to-server connections
  * **STP (shielded twisted pair)**
    - consists of twisted pairs of wires surrounded by shielding to protect them from EMI
    - only really matters in locations with excessive electronic noise
  * connect via **UTP (unshielded twisted pair) cable**
    - specified cabling for 10/100/1000BaseT
    - consists of AWG 22-26 gauge wire twisted together into color-coded pairs
    - Cat 1: standard telephone line
    - Cat 3: for 10Mbps networks; variant that used all four pairs of wires supported 100Mbps
    - Cat 5: designed for 100Mbps networks
    - Cat 5e: enhanced to handle 1000Mbps networks
    - Cat 6: supports 1000Mbps at 100m segments; 10Gbps networks up to 55m segments
    - Cat 6a: supports 10Gbps networks at 100m segments
    - Cat 6e: nonstandard term used by few manufacturers for Cat 6 or Cat 6a
    - Cat 7: supports 10Gbps at 100m segments; shielding ofr individual wire pairs reduces
     crosstalk and noise problems, not an ANSI/TIA standard
    - most installers use Cat 5e, Cat 6 or Cat 6a
    - Pin  T568A             T568B
    - 1    White/Green       White/Orange
    - 2    Green             Orange
    - 3    White/Orange      White/Green
    - 4    Blue              Blue
    - 5    White/Blue        White/Blue
    - 6    Orange            Green
    - 7    White/Brown       White/Brown
    - 8    Brown             Brown
  * **RJ-11 (registerd jack)**
    - used in telephones and telephone-based internet connection
    - supports up to two pairs of wires
  * **RJ-45**
    - standard for UTP connectors
    - connections up to four pairs and wider than RJ-11
  * **crossover cable**
    - can connect two computers directly
    - standard UTP cable with one RJ-45 connector using T568A and other using T568B
  * standard network cables use PVC for the jacket, but PVC produces noxious fumes when burned
  * Plenum-grade cable is with fire-retardant jacket, cost three to five times more than PVC
  * **Fiber Optic**
    - immune to electrical problems, signals travel 2000m or more
    - use 62.5/125 multimode fiber optic cable
    - require two cables as half-duplex, data flows in one way
    - light can be sent down as regular light or laser light
    - multimode cables
       + tansmits multiple light signals at same time, use LEDs to send signals
       + used for relatively short distances
       + multimode network runs at 10, 100, or 1000Mbps, distance usually max at ~600m
    - single-mode cabling
       + use laser light
       + have high transfer rates over long distances
       + recorded speed at 100Tbps over 100miles
     and currently quite rare
    - cabling standards: 1000BaseSX, 10GBaseSR
    - need a fiber optic switch and fiber optic network cards
  * **Coaxial**
    - used in early versions of Ethernet
    - consist of center cable (core) surrounded by insulation
    - rated using RG by impedance, RG59 and RG6 both have 75&ohm; impedance
    - RG59 is thinner and does not carry data as far as RG6
    - RG6QS (quad shield): extra shielding reduces interference and enables stronger internal
     signals
    - use BNC connector (uncommon) and F-type connector

LAN (local area network)
------------------------
  * a group of computers that are able to hear each other when one sends broadcast
  * broadcast domain: a group connected by one or more switches
  * LAN is almost always a broadcast domain
  * Ethernet over Power
    - requires specialized bridges that connect to power outlets
    - bridge: device that connects dissimilar network technologies that transmit same signal

Structured cabling
------------------
  * standards defined by ANSI/TIA
  * exposed cables
    - can cause to trip over
    - presence of electrical devices close to the cable can create interference
    - limits the ability to make any changes to the network
  * should have flexibility to enable the network to grow and upgrade as needed
  * components: telecommunications room, horizontal cabling, work area
  * **horizontal cabling**
    - a run: single piece of installed horizontal cabling
    - goes more or less horizontally form a work area to the telecommunications room
    - use Cat 5e or better UTP
    - solid core UTP
       + uses single solid wire
       + better conductor but stiff and will break if handled too often
    - stranded core UTP
       + each wire is a bundle of tiny wire strands
       + not quite good conductor but robust
    - horizontal cabling should always be solid core
  * **work area**
    - office or cubicle with a wall outlet that serves as the termination point for horizontal
     network cables
    - wall outlet consists of one or two female jacks, a mounting bracket and a faceplate
    - female RJ-45 jacks outlets have Cat ratings
    - first place to look when broken cable is suspected
  * **telecommunications room**
    - heart of the basic star
    - **equipment racks**
       + provides a safe, stable platform for all different hardware components
       + 19 inches wide, but vary in height from two to three foot models that bolt onto wall
       + evolved out of the railroad signaling racks from 19th century
       + can mount almost any network hardware component
       + aslo available rack-mounted UPSs (uninterruptible power supplies)
       + all rack-mounted equipment uses height measurement U (1.75inches)
       + a device that fits in a 1.75inch space is called a 1U
       + most rack-mounted devices are 1U, 2U or 4U
    - **patch panel**
       + a box with a row of female connectors (ports) in the front and permanent connections
         in the back
       + uses a special type of connector called 110 block or 110-punchdown block
       + UTP cables connect to a 110 block using a punchdown tool (has a blunt end that forces
         the wire into the groove)
       + prevent horizontal cabling from being moved
       + use a ANSI/TIA 606 labeling method
       + can get UTP, STP or fiber ports and some manufacturers combine several different
         types on the same patch panel
       + UTP patch panels come with Cat ratings
       + patch cables are short (2 to 5ft) stranded cables which require specific crimps
       + a good patch cable should include a boot

WAN (wide area network)
-----------------------
  * widespread group of computers connected using long-distance technologies
  * router connect LANs into WAN
  * turning a group of LANs into WAN raises issues with network traffic
  * broadcasting is unacceptable
  * routing requires routers and routing-capable protocol functions
  * routers destroy any incoming broadcast frames
  * to go beyond a LAN requires a TCP/IP network protocol
`back to top <#a+top>`_

### 19. Local Area Network

* network protocol software
  * takes the incoming data received by the NIC, keeps it organized and sends it to the app
    that needs it
  * take the outgoing data from the app and hands it to the NIC to be sent out over the network
* any network address must uniquely identify the machine and locate that machine withint the
  larger network

IP address
----------
  * in a *TCP/IP* network, IP address is the unique identification number for the system on the
    network
  * **IPv4**
    - consists of four sets of eight binary numbers (octets)
    - use dotted-decimal notation (202.34.16.11)
  * **Subnet mask**
    - used by NIC to distinguish which part of the IP address identifies the network ID and
     which part identifies the host
    - 255.255.255.0, any part that's all 255s is the network ID, any part that's all zeros is
     the host ID
    - IP: 192.168.4.33, Subnet mask: 255.255.255.0, network ID: 192.168.4, host ID: 33
    - every computer on a single LAN must have the same network ID and unique host ID
    - subnets have classes: A (255.0.0.0), B (255.255.0.0), C (255.255.255.0)
  * **IP conflict**
    -two computers having the same IP address and are not able to talk to each other and
     other computers don't know where to send data
    - can never have an IP address that ends with a 0 or 255
  * **CIDR**
    - Classless Inter-Domain Routing, works easily in binary
    - refer to the subnet mask by the number of ones it contains
    - 255.255.255.0, 11111111.11111111.11111111.00000000, has 24 ones, /24 (what twenty-four)
  * IP addressing enables interconnecting of network IDs to make WANs
  * the switch filters and forwards by MAC address
  * the router filters and forwards by IP address
  * **default gateway**
    - IP address of the LAN side of the router used by the computer to send data to outside
     the network ID
  * **DNS servers**
    - Domain Name Service, keep databases of IP addresses and their corresponding names
    - TLDs (top-level domains)
       + .com (general business), .org (nonprofit orgs), .edu (educational orgs)
       + .gov (government orgs), .mil (military orgs), .net (internet orgs)
       + .int (international), .name, .biz, .info, .tv
  * **DHCP server**
    - provides the computer with all the IP info it needs to get on the network
    - DHCP reservations: network ID of 192.168.4.x will reserve .1 for the default gateway
     and .2 through .9 for servers
* people who developed the applications decide which protocol to use

TCP (Transmission Control Protocol)
-----------------------------------
  * connection-oriented protocol used with TCP/IP
  *  HTTP is based on TCP
  * comes with communication rules that require both sender and receiver to acknowledge the
    other's presence and readiness to send and receive data
  * supply other services than HTTP
  * SSH (secure shell): can access a remote system's terminal
  * TCP/IP links any two hosts whether the two computers are on the same LAN or on some other
    network withint the WAN

UDP (User Datagram Protocol)
----------------------------
  * connectionless protocol
  * works best when there's a lot of data to send that doesn't need to be perfect
  * faster than TCP

TCP/IP tools
------------
  * ping
    - to see if able to talk to another system or not
    - ping DNS_NAME (ping www.google.com)
  * ipconfig/ifconfig
    - to quick view the network settings
    - ipconfig /all (Windows), ifconfig (Linux or macOS)
  * nslookup
    - enables to determine exactly what info the DNS server is giving about a host name
  * tracert/traceroute
    - show the route that a packet takes to get to its destination
    - handy when troubleshooting bottlenecks
    - tracert (Windows), traceroute (Linux or macOS)

APIPA or zeroconf
-----------------
  * Automatic Private IP Addressing (Windows)
  * feature that automatically assigns an IP address to the system when the client cannot
    obtain an IP address automatically
  * if the computer cannot contact the DHPC server, it randomly chooses an address in the form
    of 169.254.x.y and a 16bit subnet mask
  * when using APIPA/zeroconf, the system can communicate only with other computers on the
    same subnet
  * enabled by default if the system is configured to obtain an IP address automatically

IPv6
----
  * 32bit IPv4 offers only 4 billion addresses
  * in IPv6, address space is extended to 128bits
  * IPv4 and IPv6 differ a lot when it comes to implementation
  * uses a colon as a separator, **2001:0000:0000:3210:0800:200c:00cf:1234**
  * each group is a hexadecimal number between 0000 and ffff (field or hextet)
  * complete IPv6 address always has eight groups of four hexadecimal characters
  * leading zeros can be dropped from any group, **2001:0:0:3210:800:200c:cf:1234**
  * can remove one or more consecutive groups of all zeros leaving two colons together,
    **2001::3210:800:200c:cf:1234**, can use only once, only one "::" is allowed per address
  * uses /x prefix length naming convention, similar to CIDR, fe80::cf:0:ba98:1234/64
  * network address: first three groups (first 48bits)
  * subnet address: fourth group (49th through 64th bits)
  * device address: last four groups (last 64 bits)
  * will have multiple IPv6 addresses a single NIC
  * computers running IPv6 always have a link-local address
  * first 64bits of link-local address are always "fe80::"
  * second 64 bits of link-locak address, interface ID, are generated with 64bit random number
    or with device's MAC address (EUI-64, extended unique identifier)
  * **prefix length**
    - determine whether to send packets to a local MAC address or to the default gateway
    - last 64bits are generated by the NIC (no prefix is longer than /64)
    - RIRs give /48 prefixes to big ISPs and end users who need large allotments, ISPs and
     others will borrow another 16bits for subnetting and pass out /64 to end users
  * **global unicast address**
    - second IPv4 address to get on the Internet
    - when th computer is plugged into network, it sends router solicitation (RS) message to
     look for a router (uses ff02::2, is read only by other computers running IPv6, different
     from a broadcast address and is called multicast address)
    - the router hears the message and responds with a router advertisement (RA) that tells
     the computer its network ID and subnet and DNS server
    - once the computer gets a prefix, it generates the rest of the address just like with
     the link-local address
    - if another computer is running IPv6 and also has a global address, it can access the
     system unless there's a firewall
    - computers using IPv6 need a global unicast address to access the Internet

installing wired network
------------------------
  * need a connected NIC, properly configured IP addressing and a switch
  * **full-duplex mode**
    - used by all modern NICs
    - can send and receive data at the same time
  * NICs and switches use autosensing feature for old devices that need to run in half-duplex
    mode, which the device can send and receive data but not at the same time
  * **link lights**
    - status indicator that gives info about the state of the NIC's link
    - NICs can have between one and four different link lights with any colored LEDs
    - first items to check when troubleshooting network
    - switches also have link lights
    - multispeed devices usually have a link light that tells the speed of the connection
    - properly functioning link light is always steady on when the NIC is connected to another
     device
  * **activity light**
    - truns on when the card detects network traffic
  * solid green light: connectivity
  * flashing green light: intermittent connectivity
  * no green light: no connectivity
  * flashing amber light: collisions on the network (sometimes okay)

QoS (quality of service)
------------------------
  * enables busy networks to prioritize traffic
  * should be enabled by default

connecting to a switch
----------------------
  * unmanaged switch
    - smart and automatic
    - doesn't do more than connecting devices
  * managed switch
    - has extra features to provide added security and efficiency
    - has an IP address that can be used to configure the options
  * VLAN (managed switch)
    - virtual local area network
    - special managed switch to break up or segment a single physical network into two or
     more distinct networks
    - can only talk to computers on the same VLAN

network shares
--------------
  * file and folder level permissions affect share permissions
  * permission levels on non-NTFS volumes
    - when using Windows default Sharing Wizard: Read, Read/Write, Owner
    - when using Advanced Sharing: Read, Change, Full control
  * on NTFS drive, must set both network permissions and NTFS permissions
  * moving and copying folders and files within network shares and between network shares and
    local computers doesn't affect the file attributes

Network Organizations
---------------------
  * **Work groups**
    - most basic and simple
    - by default, all computers on the network are assigned to WORKGROUP
    - every computer on the network needs the same workgroup name to share resources
    - lack centralized control, works well for smaller networks
  * **Domains**
    - centralizes user accounts, passwords and access to resources
    - computer running Windows Server is configured as a domain controller, which stores a
     set of domain accounts (also called an authentication server)
    - modern versions of Windows use an Active Directory Domain
    - must have a computer running a version of Windows Server
    - create the Active Directory and each PC on the network needs to join the domain
    - can log onto a domain with <domain>\<domain user name>
    - Active Directory Users and Computers utility provides basic Active Directory functions
    - folders: Builtin (sic), Computers, Domain Controllers, Users
    - cannot promote a local user or group to a domain user or group, domain admin must create
     new domain account on the domain controller
    - with domain account, can add a logon script that runs every time user logs in
    - from Active Directory, can pick where to store users home folders but requires the use
     of roaming profiles rather than local profiles
    - folder redirection: users able to access home folders on a remote server rather than
     local machine and helps admins keep tighter control over network resources
    - organizational units (OUs) enable to organize users and computers by function, location,
     permissions, etc.
  * **Homegroups**
    - use the idea that people want to share data, not folders
    - skip folders completely and share Windows libraries
    - connects a group of computers using a common password
    - each computer can be a member of only one homegroup at a time
    - require IPv6 protocol
    - all homegroup data is encrypted between systems
    - fits for smaller, non-domain home networks

Problems & Troubleshooting
--------------------------
  * check cable, NIC disabled or not, check link lights on the NIC and switch
  * will get disconnect erros if not connected to a running switch
  * getting an APIPA address rather than an accurate network address points to a disconnect
    between the workstation and DHCP server
  * multiple systems failing to access the network often points to hardware problems
  * plug the system to a known goot outlet and see if it works
  * bad NIC can also generate a "can't see the network" problem
  * NIC's female controller is a common failure point
  * **loopback test**
    - sends data out of the NIC and checks to see if it comes back
    - true external loopback requires a loopback plug inserted into the NIC's port

Cable testing
-------------
  * **midrange time-domain reflectometer (TDR)**
    - measures impedance in network cabling (eg. Fluke MicroScanner)
    - if the tester measures any impedance, something is wrong with the cable
  * always include the patch cables when testing
  * **toner**
    - to trace cables
    - generic term for two separate devices
    - tone generator: connects to the cable using alligator clips, tiny hooks, or a network
     jack and sends electrical signal along the wire at certain frequency
    - tone probe: emits a sound when it is placed near a cable connected to the tone generator
    - each generator emits a separate frequency and the probe sounds different tone for each

failing to connect to a new resource
------------------------------------
  * often points to a configuration issue
  * don't have right share name, required user name/password, permissins to access
  * not on the right homegroup/domain/workgroup
  * resource isn't shared or doesn't exist

net (Windows)
-------------
  * to view a network from command line
  * ``net view``: show any shares on the machine and whether they are mapped drives
  * ``net use``: to mapp network shares

nbstat (Windows)
----------------
  * NetBIOS over TCP/IP Statistics
  * in old days, used for LAN file sharing
  * now use to resolve host names on the network when a DNS server is not available
  * also useful when troubleshooting naming issues in small workgroups
  * can also query a remote machine by IP to find out its NetBIOS name
  * can see all the names that NetBIOS has in its local cache
`back to top <#a+top>`_

### 20. Wireless Networking

* wireless networks use either radio waves or beams of infrared light (used in some smartphones
  to be used as remote control)
* radio waves are based on the IEEE 802.11 wireless Ethernet standard
* infrared light are limited to those that use the Infrared Data Association (IrDA) protocol

WAP (wireless access point)
---------------------------
  * centrally connects wireless network nodes in the same way as hub connects wired PCs
  * also acts as switches and Internet routers
  * **PoE (power over Ethernet)**
    - only need to plug a single Ethernet cable into the WAP to provide it with both power and
     a network connection which are both supplied by a PoE-capable switch
    - Power over Ethernet injector can extend a PoE connection up to 100m
* use the *carrier sense multiple access/collision avoidance (CSMA/CA)* networking scheme
* wireless node listens in on the wireless medium to see if another node is currently
  broadcasting data
* wireless nodes have a more difficult time detecting data collisions

Request to Send/Clear to Send (RTS/CTS)
---------------------------------------
  * enable a transmitting node to send an RTS frame to the receiving node after it determines
    the wireless medium is clear to use
  * receiving node responds with a CTS frame
  * introduces a significant overhead to the process and can impede performance

Ad Hoc mode
-----------
  * sometimes called peer-to-peer mode
  * each wireless node in direct contact with every other node in a decentralized free-for-all
  * two or more nodes form an Independent Basic Service Set (IBSS)
  * suited for small groups of computers
  * Microsoft disabled default support for ad hoc networks
  * each node be configured with the same SSID and no two nodes use the same IP address

Infrastructure mode
-------------------
  * use one or more WAPs to connect wireless nodes to a wired network segment
  * Basic Service Set (BSS): single WAP servicing a given area
  * Extended Basic Service Set (EBSS): BSS extended by adding more WAPs
  * for networks that need to share dedicated resources, SOHO WAPs and SOHO routers
  * required to be configured same SSID on all nodes and on all WAPs
  * some WAPs can be configured through a browser-based setup utility

wireless mesh network (WMN)
---------------------------
  * hybrid wirelss topology
  * nodes act like routers, forwarding traffic for other nodes
  * more static than a traditional ad hoc network
  * used in interconnecting computers in deployed US Army field units and Google Home devices

SSID (service set identifier)
-----------------------------
  * also called the network name
  * WAPs are configured to announce their presence by broadcasting the SSID to their max range
  * always change the default SSID and password
  * included in the header of every data packet broadcast
  * packets that lack the correct SSID name in the header are rejected
  * can tell the WAP not to broadcast the SSID

Security
--------
  * wireless networking offers weak security
  * methods to secure: MAC address filtering, authentication, data encryption
  * **MAC address filtering**
    - enables to limit access to wireless network based on the MAC address
    - same as creating a type of accepted users list
    - attacker can still use software to listen for the MAC addresses of nearby clients and
     spoof the address of an accepted client
  * **Authentication**
    - **WEP (wired equivalent privacy) protocol**
       + uses a standard 40bit encryption, same level as 64bit, to scramble data packets
       + also 104bit encryption, same as 128bit, is supppported
       + traffic is encrypted with the same key
       + one user's traffic isn't protected from other members of the network
    - **WPA (wifi protected access)**
       + uses the *Temporal Key Integrity Protocol (TKIP)*, which provides new encryption key
         for every sent packet
       + Extensible Authentication Protocl (EAP): encryption key intergrity-checking and user
         authentication feature offered to WEP
    - **WPA2**
       + IEEE 802.11i standard
       + uses the Advanced Encryption Standard (AES)
    - **WPA3**
       + include encryption to protect the data of users on open (public) networks
    - **WPS (wifi protected setup)**
       + standard included on most WAPs and clients to configure secure connections easier
       + some devices use a push button, others a password or code
       + WPS-capable WAP will have an eight-digit numeric code printed on the device
       + code is easy to guess because of how it is set up
* WPA/WPA2 can be setup with Personal/Pre-Shared Key (PSK) or Enterprise
  * Enterprise assume authentication be enabled by using **RADIUS server**
    - a protocl for authenticating
    - Remote Authentication Dial-In User Service is partially encrypted and uses UDP
    - Terminal Access Controller Access-Control System Plus (TACACS+) if full encrypted and
     uses TCP, can handle authentication, authorization of resources and keep track of resources
    - original TACACS has no encryption and uses TCP or UDP and only handles authentication

speed and range issues
----------------------
  * depends on factors such as the standard the wirelss devices use, the distance between
    wireless nodes
  * dead spots: interference caused by solid objects and other wireless devices operating in
    the same frequency
  * large electrical appliances such as refrigerators block signals very effectively
  * true effective range of wireless device is about half what it is listed by manufacturers
  * can install multiple WAPs to permit roaming between one WAP coverage area and another
  * wireless repeaters/extenders can receive and rebroadcast wifi signal

wirelss networking Standards
----------------------------
  * most use radio frequency (RF) technologies
  * Wi-Fi, IEEE 802.11 wireless Ethernet standard, defines methods devices may use to
    communicate via spread-spectrum radio waves
  * some devices radio frequencies that fall in *industrial, scientific and medial (ISM)* bands
  * band: contiguous range of frequencies that is usually divided up into discrete slices
    called channels
  * cellular is famous in wide area roaming networks
  * 802.11 variations: 802.11a, 802.11b, 802.11g, 802.11n, 802.11ac
  * 802.11n standard may use both 2.4GHz and 5GHz
  * 802.11g devices can use 802.11n WAP
  * 802.11ac WAP is backward compatible with 802.11b, g and n
  * **802.11a**
    - requires 5GHz, only 802.11ac and dual-band 802.11n WAPs are backward compatible
    - less prone to interference
    - greater input than 802.11 and 802.11b at speeds up to 54Mbps
    - max range at about 150ft, not widely adopted
  * **802.11b**
    - first standard to become widely used, requires 2.4GHz ISM band
    - supports throughput up to 11Mbps
    - max range of 300ft under ideal conditions
  * **802.11g**
    - speed up to 54Mbps, range about 300ft
    - runs in 2.4GHz ISM band
    - backward compatible with 802.11b
  * **802.11n**
    - multiple in/multiple out (MIMO): feature that enable the devices to make multiple
     simultaneous connections
    - up to four antennas, speed up to 600Mbps, range 300+ft
    - transmit beamforming: multiple-antenna technology that helps get rid of dead spots
    - can run in 2.4GHz ISM band, supporting slower 802.11b/g devices
    - also support dual-band operation but need more advanced WAP that runs at both 5GHz and
     2.4GHz simultaneously
  * **802.11ac**
    - only uses 5GHz band
    - multiuser MIMO (MU-MIMO): gives a WAP to broadcast to multiple users simultaneously
    - supports dual-band operation
    - some even support tri-band, adding a second 5GHz signal
  * **802.11ax**
    - faster than 802.11ac and uses 2.4 and 5 GHz
    - also known as High Efficiency Wireless (HEW), Wi-Fi 4, Wi-Fi 5, Wi-Fi 6

Infrared wireless networking
----------------------------
  * still viable to transfer files on some older devices
  * implemented via the Infrared Data Association (IrDA) protocol
  * speed up to 4Mbps, max distance between devices is 1m
  * infrared links are direct line-of-sight and susceptible to interference
  * even bright sunlight hitting the transceiver can casue interference
  * designed to make point-to-point connection between two devices only in ad hoc mode
  * infrared devices operate at half-duplex
  * has a mode that emulates full-duplex but really just half
  * no encryption or authentication

Bluetooth
---------
  * name for tenth-century Danish king Harald Bluetooth
  * first gen (1.1 and 1.2)
    - supports speeds around 1Mbps
  * second gen (2.0 and 2.1)
    - backward compatible with first gen
    - support for more speed with Enhanced Data Rate (EDR), speed around 3Mbps
  * third gen (3.0 + HS)
    - speed up to 24Mbps
    - High Speed feature is optional
  * fourth gen (4.0, 4.1 and 4.2)
    - also called Bluetooth Smart
    - improved suitability for use in smart devices by reducing cost and power consumption
  * fifth gen (Bluetooth 5)
    - adds options to increase speed at the expense of range or by changing packet size
    - better support for IoT devices
  * bluetooth uses a broadcasting method that switches between any of the 79 frequencies
    available in the 2.45GHz range
  * hops frequencies some 1600 times per second
  - Class        Max Power Usage         Max Range
  - Class 1      100 mW                  100m
  - Class 2      2.5 mW                  10m
  - Class 3      1 mW                  1m
  * recent introduction of IP connectivity
  * comparable to other technologies
    - like infrared, bluetooth is acceptable for quick file transfers
    - speed and range make it a good match for wireless printer server solutions
  * some devices always have bluetooth enabled (eg. bluetooth headsets)
  * some devices continuously listen for a pairing request (eg. car stereo)
* cellular is not really used for local area networking

omni-directional and centered
-----------------------------
  * placing a WAP with an omni-directional antenna in the centre of the area
  * radio waves flows outward from the WAP
  * dipole antennas: straight-wire antennas that provide most omni-directional function, have
    two antenna arms or poles aligned on the antennad's axis
  * gain: ratio of increase, measured in decibles (dB)
  * typcial WAP is 2dB
  * gian antennas cause the antenna to pick up weaker signals and have an amplifying effect on
    transmitted signals
  * increasing the size of dipole will increase gain evenly in all directions
  * paraboli dish-type antennas and multi-element Yagi antennas increase gain in specific
    directions and reduce gain in other directions, the more elements a Yagi has, the higher
    the directional gain
  * polarization
    - signal having the same alignment as the antenna
    - latop or notebook usually has vertical polarization
    - orient the WAP antennas on a 45-degree angle for good connections
    - wireless signals should have the same polarization
  * Wi-Fi locators/Wi-Fi analyzers can be installed as an app on Wi-Fi smartphones

Troubleshooting
---------------
  * find out who, what and where
  * hardware problems
    - check device driver, reinstalled if needed
    - check devices seated or plugged properly
  * software problems
    - check vendor-provided drivers installed or not
    - configure utility before plugging in the device
    - update the access point's firmware
  * connectivity problems
    - wireless clients use a multi-bar graph (usually five bars) to indicate signal strength
    - check NIC's link lights or configuration utility
    - change network channel
    - move interfering devices to another area
  * config problems
    - check SSID and security configurations
    - changing the SSID of WAP will prevent the client from connecting
    - check MAC address filtering, important if NICs are swapped
    - make sure all wireless nodes and access points match
    - encryption level must match on access points and wireless nodes
`back to top <#a+top>`_

### 21. The Internet


Internet tiers
--------------
  * **Tier 1**
    - Tier 1 providers own long-distance, high-speed fiber optic networks called backbones
    - backbones interconnect at special locatins called *network access points (NAPs)*
    - must pay large amount if want to connect Tier 1
    - Tier providers do not charge each other to connect
  * **Tier 2**
    - own smaller, regional networks and must pay the Tier 1 providers
    - provide Internet access to the general public
  * **Tier 3**
    - even more regional and connect to Tier 2 providers
  * **Backbone routers**
    - connect to more than one other backbone router, creating a big, interwoven framework
     for communication
    - to provide alternative pathways for data

TCP/IP
------
  * provides the basic software structure for communication on the Internet
  * also provides the framework and common language
  * games employ TCP/IP to send information over ports reserved by the game

ISPs (Internet service providers)
---------------------------------
  * lease connections to the Internet from every Tier 1 and Tier 2 provider
  * sit along the edges of the Tier 1 and Tier 2 Internet and tap into the flow
  * consumers lease connections from ISPs
* *metropolitan area network (MAN)*: network that covers a single city

Modems
------
  * modulator/demodulator
  * enable computers to talk to each other via standard commercial telephone lines by
    converting analog signals to digital signals and vice versa
  * baud: one cycle per second, unit for phone lines speed
  * fastest rate of phone line is 2400 baud
  * modems connect to telephone cables with a four-wire connector and port
  * almost all internal modems connect to a PCI or PCIe expansion bus slot
  * external modems connect through available USB port and require no external electrial source

Connecting to the Internet
--------------------------
  * **dial-up**
    - requires hardware to dial to the ISP and software to govern the connection
    - ISP provides a dial-up telephone numbers, user name and password
    - Point-to-Point Protocol (PPP): streaming protocol developed for dial-up Internet access
    - **analog**
       + the slowest, requires a telephone line and special networking device called modem
    - **ISDN**
       + integrated services digital network, uses digital dial-up
       + B channels carry data and voice information at 64Kbps
       + D channels carry setup and config info and data at 16Kbps
       + basic rate interface (BRI): two B/one D setup
       + BRI uses only one physical line, but each B channel sends 64Kbps
       + ISDN connects much faster than modems
       + monthly cost per B channel is slightly more than regular phone line
       + has steep initial fee and need to be within about 18,000 feet of a central office
       + primary rate interface (PRI): composed of twenty-three 64Kbps B channels and one
         64Kbps D channel, also known as T1 lines
       + ISDN wall socket usually looks like standard RJ-45 jack
       + terminal adapter (TA): common interface for computer, look like regular modems
  * **dedicated**
    - DSL and cable use a modem that connects to a regular Ethernet
    - **DSL**
       + Digital subscriber line, use a standard telephone line with special equipment on each
         end to create always-on Internet connections at speeds greater than dial-up
       + at low end, speeds are generally single digits
       + xDSL technologies offer competitive broadband speeds
       + require DSL modem and need to hook up a wireless router
       + DSL microfilters remove the high-pitch screech of the DSL signal
       + receiver connects to the telephone line and the computer
       + house has to be within a short distance from a main phone service switching center,
         around 18,000 feet
       + asymmetric (ADSL): differ between slow upload speed (384Kbps, 768Kbps, 1Mbps) and
         faster download speed (3-15Mbps)
       + symmetric (SDSL): has same upload and download speeds as ADSL but cost more
    - **cable**
       + use regular TV cables to serve up lightning-fast speeds
       + upload speed from 5-35+Mbps and download speed from 15-1000+Mbps
       + start with RG-6 or RG-59 cable coming into the house, connects to a cable modem that
         connects to a small home router or NIC via Ethernet
    - **fiber**
       + fiber-to-the-node (FTTN)
         + fiber connections runs from the provider to a box somewhere in the neighborhood
         + box connects to the home or office using normal coaxial or Ethernet cabling
       + fiber-to-the-premises (FTTP)
         + runs from provider straight to a home or office, using fiber the whole way
  * **wireless**
    - most open hotspots do not provide encryption
    - **line-of-sight wireless**: up to eight miles or more, can use 2.4GHz, 5GHz or 24GHz
    - **cellular**
       + either wirelessly as mobile hotspot or directly using USB
       + most carriers charge extra to enable tethering
       + 1G, 2G, 2.5G (not 2G but also not quite 3G), 3G, 4G, 5G, just for marketing terms
       + 1G data services was analog and not at all designed to carry packetized data until
         GSM (Global System for Mobile Communications), CDMA (code division multiple access)
       + GSM evolved into GPRS and EDGE (2.5G), CDMA introduced EV-DO (3G)
       + Long Term Evolution (LTE): rolled out in early 2010s and now accepted as true 4G
       + wireless wide area network (WWAN) works similar to wireless LAN (WLAN) but connects
         multiple networks
       + 5G started development push in 2018
  * **satellite**
    - may use either a modem or a NIC
    - get the data beam to a satellite dish, a receiver handles the flow of data and send it
     through an Ethernet cable to the NIC
    - need to make sure the satellite dish points toward satellites
    - satellite modem has an RJ-45 connection
    - has upfront cost of the dish and installation, distance create a satellite latency
    - most satellite operators implement various usage caps to keep the system from overload

NAT (Network Address Translation)
---------------------------------
  * presents an entire LAN of computers to the Internet as a single machine, acts as firewall
  * anyone on the Internet only sees the public IP address
  * all computers in LAN use private addresses that are invisible to the world
  * Dynamic NAT is also called Pooled NAT
* UPnP
  * universal plug and play seet out, find and connect to other UPnP devices
  * enable seamless interconnectivity at the cost of lowered security

proxy server
------------
  * many companies use to filter employee Internet access
  * software that enables multiple connections to the Internet to go through one protected
    computer
  * apps that want to access the Internet resources send requests to the proxy server instead
    of trying to access the Internet directly
  * enables network admins to monitor and restrict Internet access
* Application Protocols
  - Application Protocol       Function                            Port Number
  - HTTP                       Web pages                           80
  - HTTPS                      Secure Web pages                    443
  - FTP                        File transfer                       20, 21
  - SFTP                       Secure file transfer                22
  - IMAP                       Incoming email (clients)            143
  - POP3                       Incoming email (clients)            110
  - SMTP                       Outgoing email (apps)               25
  - Telnet                     Terminal emulations                 23
  - SSH                        Encrypted terminal emulation        22
  - RDP                        Remote desktop                      3389
  - SIP                        Voice over IP                       5060
* Utility Protocols
  - Utility Protocol       Function                                Protocol      Port Number
  - DNS                    Allows the use of DNS naming            UDP           53
  - DHCP                   Automatic IP addressing                 UDP           67, 68
  - LDAP                   Querying directories                    TCP           389
  - SNMP                   Remote management of network devices    UDP           161, 162
  - SMB                    Windows naming/folder sharing, CIFS     TCP           445
                                                                   UDP           137, 138, 139
  - AFP                    macOS file services                     TCP           548
  - SLP                    Services discovery protocol             TCP/UDP       427
  - NetBIOS/NetBT          NetBIOS over TCP/IP                     UDP           137, 138
                                                                   TCP           137, 139

FTP (file transfer protocol)
----------------------------
  * great way to share files between systems
  * must use an FTP client such as FileZilla or browser
  * all FTP sites require to log on
  * add name to the URL to log on, ftp://username@ftp.example.com
  * send passwords and user names as clear text

Telnet & SSH
------------
  * both are terminal emulation programs
  * **Telent**
    - shares FTP's bad habits of sending passwords and user names as clear text
  * **SSH**
    - replaces telnet
    - tunneling: move files or any type of TCP/IP network traffic through secure connection

SFTP
----
  * Secure FTP, protocol for transferring files over an encrypted SSH connection
  * was written as an extension of SSH
  * built into SSH software, such as OpenSSH

VoIP (Voice over IP)
--------------------
  * to make voice calls over network
  * works with every type of high-speed Internet connection
  * is a collection of protocols that make phone calls over the data network possible
  * *Session Initiation Protocol (SIP)*: most common VoIP application protocol
  * many corporations use VoIP for their internal phone networks
  * low network latency is more important than high network speed
  * can use dedicated VoIP phones or dedicated VoIP box that can interface with existing analog
    phones
  * true VoIP phones has RJ-45 connections that plug directly into the network and offer
    features such as HD-quality audio and video calling but require complex and expensive
    network to function

Remote Desktop
--------------
  * use *Remote Desktop Protocol (RDP)* or *Virtual Network Computing (VNC)*
  * Remote Desktop Connection: Windows alternative for VNC, provides control over a remote
    server with fully graphical interface (`mstc.exe`)
  * Windows Remote Assistance enables to give anyone control of the desktop or take control of
    anyone else's desktop
  * always need both a server and client program
  * screen sharing: macOS built in feature

VPN (virtual private network)
-----------------------------
  * encrypted tunnel requires endpoints, which are the ends of the tunnel where the data is
    encrypted and decrypted
  * endpoint management server must act as an endpoint for a VPN
  * requires a protocol that uses one of the tunneling protocols and ask for an IP address from
    a local DHCP server to give the tunnel an IP address that matches the subnet of local LAN
  * tunnel endpoints must act like NICs
  * **PPTP (point-to-point tunneling protocol)**
    - advanced version of PPP, PPTP endpoints are on the client
    - Routing and Remote Access Service (RRAS): special remote access server program from
     Microsoft

File Sharing
------------
  * using legal technologies to access both legal and illegal content
  * **BitTorrent**
    - peer-to-peer file sharing program
    - each computer that downloads a portion of shared resource will by default also share
     that portion with everyone else trying to download the resource

LDAP
----
  * lightweight directory access protocol
  * enables operating systems and apps to access directories

SNMP
----
  * simple network management protocol
  * enables remote query and remote config of just about anything on a network

SMB
---
  * server message block protocol
  * Windows' network files and print sharing protocol
  * UNIX and Linux used Network File System (NFS) protocl, but has declined and now using SAMBA
  * SMB is protocol of choice for LAN file servers
  * Common Internet File System (CIFS): variation from Microsoft but now deprecated

AFP
---
  * Apple filing protocol
  * also used by macOS Time Machine for backing up macOS over the network due to its support
    for HFS+ file system
  * solid on Linux but Windows lacks out-of-box support for AFP

SLP
---
  * service location protocol
  * to advertise available services over a local network and to discover the services

IoT (Internet of Things)
------------------------
  * smart devices such as thermostats, lights, digital assistants
  * Z-Wave (proprietary) and Zigbee (open), both use a mesh networking topology to facilitate
    communications in homes, yet both aslo have hubs that act as the network interconnect

Troubleshooting
---------------
  * most Internet connection problems are network connection problems
  * no connectivity
    - check with ping or flush dns
  * limited connectivity
    - points to a DHCP problem
  * local connectivity
    - can access network resources but not the Internet
    - downed DHCP server or problem with the router
  * slow transfer speeds
    - multiple programs or devices using at the same time
`back to top <#a+top>`_

### 22. Virtualization


benefits
--------
  * **Power saving**
    - can place multiple virtual servers or clients on a single physical system
    - reduce electrical power use substantially
  * **Hardware consolidation**
    - complex desktop PCs can be replaced with simple but durable thin clients
  * **System management and security**
    - can setup new systems quickly with specific virtual machine with all of the software
     needed installed
    - host machine, hypervisor and any other VMs it runs are unaffected and uninfected
    - network VMs have security requirements similar to a physical system
  * **Research**
    - can test multiple programs on different OSs with one machine

Hypervisor
----------
  * supervisor: handle very low-level interaction among hardware and software
  * hypervisor: extra layer of sophisticated programming, manage more complex interactions
  * **VMware Workstation**
    - released in 1999 for Windows and Linux
  * **Microsoft Hyper-V**
    - comes with Windows Server and desktop Windows starting with Windows 8 Pro, free
  * **Oracle VM VirtualBox**
    - powerful and runs on Windows, macOS and Linux, free
  * **VMware Fusion and Parallels Desktop**
    - popular hypervisors for macOS but not free
  * **KVM**
    - for Linux, free
  * virtualization takes the hardware of the host system and allocates portion of its power to
    individual virtual machines
  * hypervisor creates a virtual machine that acts exactly like the host system
  * hypervisors simply pass the code from the virtual machine to the actual CPU
  * **bare-metal hypervisor**
    - improve performance by removing the host OS and install nothing but the hypervisor
    - no other software between the hypervisor and the hardware
    - callded Type-2 hypervisor (Type-1: applications such as VMware Workstation)
    - ESX: introduced by VMware in 2001
    - ESXi: successor of ESX, free hypervisor powerful enough to replace the hose OS,
     everything is done through a Web interface
  * a host running its hypervisor from flash memory can dedicate all of its available disk
    space to VM storage

Emulation
---------
  * emulator is software or hardware that converts the commands to and from the host machine
    into an entirely different platform
  * requires hardware several times more powerful than the platform being emulated

Client-side virtualization
--------------------------
  * running a virtual machine on local system
  * regardless of whether the VM file might be stored locally or on a central server
  * creating VM
    - setup system hardware to support VMs and verify it meet the requirements
    - install hypervisor
    - create new VM
    - start the VM and install new guest OS

Hardware support and requirements
---------------------------------
  * every hypervisor will run better if hardware virtualization support is enabled
  * every Intel-based CPU since the late 1980s is desinged to support a supervisor for
    multitasking
  * hardware virtualization support: Intel's VT-x and AMD's AMD-V
  * each VM needs just as much RAM as a physical one
  * every snapshot or checkpoint made requires space
  * basic recommendations
    - plenty of space for all the VMs
    - protect VM files with RAID arrays and regular backups
    - store VMs on SATA or NVMe SSD for better performance

Network requirements
--------------------
  * every hypervisor can connect each of its VMs to a network
  * **internal networking**
    - no Internet connection, just VMs thinking they are the only computers in existence
    - set their respective virtual NICs to an internal network
    - useful when testing with networking tool
  * **bridge networking**
    - VM's virtual NIC bridge the real NIC to get out to the network
    - VM with a bridged network connection accesses the same network as the host
    - subject to all the same security risks as a real computer
    - default settings when creating a new VM
  * virtual NIC can take DHCP (dynamic host configuration protocol) info just like a real NIC
  * hypervisors make *virtual switches*
* when installing VM on Windows, first enable Microsoft's Hyper-V by going into the Programs
  and Features Control Panel applet and select *Turn Windows features on or off*
* virtual desktop more commonly refers to the multiple desktop environments that can be
  accessed in modern OSs

Cloud
-----
  * service is the key to understanding the cloud
  * **IaaS (Infrastructure as a Service)**
    - providers use virtualization to minimize idle hardware, protect against data loss and
     donwtime, and respond to spikes in demand
    - no longer need to purchase expensive, heavy hardware
    - not responsible for the hardware, but still for configuring and maintaining the OS and
     software of any VMs created
    - have flexibility to tune it for needs, but also requires knowledge of the underlying
     OS and time to manage
  * **PaaS (Platform as a Service)**
    - providers gives programmers all the tools they need to deploy, administer and maintain
     a Web app
    - infrastructure underneath is largely invisible to the developer
    - provider is aware of their infrastructure but the developer cannot control it directly
     and doesn't need to think about its complexity
    - just a place to deploy and run applications
    - eg. Heroku, one of the earliest PaaS providers, creates a simple interface on top of
     the IaaS offerings of AWS
  * **SaaS (Software as a Service)**
    - best examples are the Web applications
    - users of the Web apps don't own the software, no installation media or can download
    - SaaS model provides access to necessary apps wherever Internet is available
    - almost anything that can be accessed on the Internet could be called SaaS
    - third-pary SaaS often have to trade strict control of user data
  * in cloud computing, there are trade-offs between cost, control, customization and privacy
  * **Public Cloud**
    - software, platforms, and infrastructure delivered through networks that the general
     public can use
  * **Private Cloud**
    -internal cloud that have some of the flexibility of the cloud and complete ownership of
     the data
    - can also contract a thrid party to maintain or host it
  * **Community Cloud**
    - more like a private cloud paid for and used by more than one organization
    - used by group of ogranizations with similar goals or needs
  * **Hybrid Cloud**
    - not all data is crucial and not every document is a secret
    - connects some combination of public, private and community clouds, allowing
     communication between them
    - cloud bursting: an app growing into a public cloud instead of grinding to a halt
    - can also integrate services across different types of cloud
  * features
    - virtualization, shared resources
    - rapid elasticity: can easily expand number of servers, even spread them out across world
    - on-demand: easy to setup app to add or reduce capacity based on demand
    - resource pooling: consolidating systems' physical and time resources
    - measured service: pay for the time every single one of virtualized servers is running
    - metered service: charge by the amount of processing resources used, such as CPU usage
    - has careful monetizing of resources used
  * usage
    - Cloud-based apps: Office 365
    - Cloud-based virtual desktops: interacting with them over the network adds a little lag
    - Cloud file storage services: include synchronization apps with their desktop versions
`back to top <#a+top>`_

### 23. Portable Computing

* trade-offs for portability
  * price, weight, size, battery life, computing power
  * input devices, ports, drives, support for hardware upgrade
  * storage capacity, durability, quality of any warranty/support programs

Chromebook
----------
  * runs Google's Linux-based Chrome OS
  * focus on Web apps by making use of cloud and SaaS

Pointing devices
----------------
  * early portables used trackballs
  * some use IBM's TrackPoint device, a joystick in the center of the keyboard
  * most recent touchpads are usually capable of detecting and ignoring accidental input
* additional microphones can help noise-cancellation

Display types
-------------
  * mostly between 10.1inch to 17.3inch diagonally
  * aspect ratio: comparison of the screen width to the height, mostly 16:9 or 16:10
  * **matte finish**
    - industry standard for many years
    - trade-off between color richness and glare reduction
    - wash out a lot in bright light
  * **high-gloss finish**
    - offers sharper contrast, richer colors, wider viewing angles
    - pick up lots of reflection from nearby objects
  * **anti-glare screen**
    - LCD panels with LED backlight

Wireless
--------
  * 802.11g or 802.11n standard is common on older laptops
  * newer portables use 802.11ac
  * antenna for laptop Wi-Fi is located in the screen portion of the laptop
* smaller laptops and hybrids usually leave out wired Ethernet

ExpressCard
-----------
  * epansion slot standard before USB dominated
  * 34mm (ExpressCard/34) and 54mm (ExpressCard/54)
  * used for SD card, cellular card, wireless cards
  * connect to Hi-Speed USB 2.0 bus or PCIe bus
* Docking stations offer legacy and modern single and multi-function ports

Batteries
---------
  * Nickel-Cadmium (Ni-Cd), Nickel-Metal Hydride (Ni-MH), Lithium-Ion (Li-Ion)
  * **Li-Ion**
    - will explode if overcharged or punctured
    - have built-in circuitry to prevent accidental overcharging
    - cna only power systems designed to use them
  * **lithium polymer (LiPo)**
    - variation of Li-Ion, solid polymer shape electrolyte instead of organic solvent
    - enables batteries to take on unusual forms beyond the simple cylinder or rectangle
    - traditional Li-Ion electrolyte packed in polymer bags instead of rigid case
  * always store batteries in cool place
  * keep battery charged, at least 70-80 percent
  * never drain a battery all the way down
  * rechargable batteries have limited number of charge-discharge cycles
  * never handle a battery that has ruptured or broken
  * always recycle old batteries
* power management: process of cooperation among the hardware, BIOS and OS to reduce power

Low-Power Modes
---------------
  * Mechanical off mode
    - system and all components, except real-time clock (RTC), are off
  * Soft power-off mode
    - system is mostly off except for components necessary for keyboard, LAN, or USB devices
     to wake the system
  * sleep or standby mode
    - normal sleep mode: can wake quickly, also called suspend to RAM
    - hibernate: deeper sleep mode, wake more slowly, also called suspend to disk, saves more
     power and won't lose its place if the device loses power

Power options
-------------
  * OS settings override CMOS settings
  * Advanced Power Management (APM), Advanced Configuration and Power Interface (ACPI)
  * many CMOS versions has settings to determine wake-up events such as Wake on LAN
* even a little moisture inside the portable can toast a component
* putting a laptop on soft surface, such as pillow on lap, creates heat-retention system
* auto-switching power supplies detect voltage at the outlet and adjust accordingly

Disassembling
-------------
  * document and label every cable and screw location
  * organize any parts extracted from the laptop
  * refer to the manufacturer's resources
  * use appropriate hand tools (iFixit toolkit)

RAM
---
  * older systems use 200pin DDR and DDR2 SO-DIMMs
  * current systems use 204pin DDR3 SO-DIMMs or newer 260pin DDR4 SO-DIMMs
  * shared memory
    - video card takes control of a portion of RAM
    - some systems allow to turn shared memory on or off

Mass Storage
------------
  * most laptops use SATA drives in 2.5inch drive format
  * most only have room for drives 7mm or 9.5mm thick
  * ancient laptops might have PATA drive
  * SSD will use a lot less electricity than an HDD
* jsut because a card will fit in the slot does not mean it will work
* fixing DC jack might be tricky as it is soldered to the main board
* when laptop won't power on, verify AC power, check outlet or remove any peripherals such as
  USB, FireWire or Thunderbolt devices
* LCD cutoff switch: small nub near the screen hinge that shuts the monitor off when lid close
* some latops have physical switch along front, rear or side edges for wireless
* ghost cursor: display shows a trail of cursors behind the real cursor as it moves, aging
  display or improperly configured refresh rate
* pointer drift: cursor moves erratically or drifts slowly in a steady direction, if reboot
  doesn't fix, the touchpad has been damaged
`back to top <#a+top>`_

### 24. Mobile Devices

* aspects that make a device mobile
  * lightweight, usually less than 2 pounds
  * small, designed to easily be carried (in hand or pocket)
  * touch or stylus interface, no keyboard or mice
  * sealed unit lacking any user-replaceable parts
  * non-desktop OS

PDA (personal digital assistant)
--------------------------------
    * earliest types of mobile device
    * had the basic features of today's mobile devices but lacked cellular connectivity

Modern smartphone
-----------------
  * elements that define a modern smartphone
    - multi-touch interface as the primary input method
    - well-standardized application programming interface (API)
    - consolidation of celluar data to the device, enable any app to exchange data over Internet
    - synchronization and distribution tools that enable users to install new apps and sync or
     backup data
  * infrastructure and technologies that connect smartphones with a mesh of networked data and
    services must be fast, robust and secure
  * typically have no user-replaceable or field-replaceable components

Purpose-built mobile devices
----------------------------
  * usually have apps for performing the same task
  * justification tends to get less obvious over time (mp3 players dominated by smartphones)
  * **E-readers**
    - such as Amazon Kindle
    - use e-paper, low-power grayscale screen technology, and a simple interface
  * **Wearable devices**
    - very small, almost always under a pound
    - small interfaces, screens less than 2 inches
    - light OSs to perform subset of  functions of mobile device OS
    - limited hardware, pairs with a host device
    - not designed to replace smartphone but to augment by handling some of simpler functions
    - smart watches: minimizes the effort of frequent smartphone tasks such as notifications,
     time, messages, weather, controlling music, fitness tracking
    - fitness monitors: count steps using accelerometers, register heart rate through sensors,
     use GPS network to track exercise (comes in fobs that clip to the body and wristbands)
  * **VR/AR headsets**
    - augmented reality: additional elements added to what user can see or hear, does not
     immerse in something artificial but can add details to physical input devices
     Google Glass, one of the first but some public placesbanned the use of it
    - new devices focus more on specific hobbies or skill groups such as enabling cyclists to
     track speed, pedaling cadence, heart rate , distance traveled, calories burned, elevation

Screen technologies
-------------------
  * most use some type of LCD panel
  * less expensive ones use TN (twisted nematic) and others use IPS (in-plane switching)
  * some use OLED (organic light-emitting diode) and AMOLED (active matrix OLED)
* mobile devices commonly have more than one microphone to enable noise-cancelling

Digitizers
----------
  * in electrical engineering, digitizers are components that transforms analog signals to
    digital
  * in mobile devices, digitizers refer to components that provide the touch part of a
    touchscreen
  * digitizer's fine grid of sensors under the glass detects the finger and signals the OS its
    location on the grid

GPS
---
  * Global Positioning System
  * customized versions of GPS are designed for SCUBA diving, hiking, hunting, etc.
  * geotracking: track and record location for an extended amount of time
  * cell phone companies and government agencies can use the ID or MAC address to pinpoint
    location any time
* closed-source products may not be interoperable with other products
* development model reflect underlying company philosophy and goals
* do not assume a device is itself open source just because the mobile OS it uses is
* Launchers
  * enable users to customize Android devices extensively
  * can change nearly every aspect of the GUI, including icon size, animations, gestures
* accelerometer: measure movement in space
* gyroscope: maintain proper orientation of up and down
* Wi-Fi calling: capability to make regular phone calls over Wi-Fi networks

Software Development Kit (SDK)
------------------------------
  * can use to create custom apps or add features to existing apps
  * Apple's rigid development model makes it such that an app must pass a rigorous testing
    program before it is allowed into the App Store
  * APK (Android application package): a package file containing the assets compiled from an
    Android app's code
* Emergency capabilities
  * old 911 system relied on the Public Switched Telephone Networks (PSTN) to trace a call and
    determine its location
  * Enhanced 911 (E911) system uses GPS and cellular networks to triangulate the location of
    a phone by its distance from cell towers

Radio firmware
--------------
  * **Preferred Roaming List (PRL) updates**
    - priority-ordered list of the other carrier networks and frequencies it should search
     for when the phone can't locate its home carrier's network
    - also called baseband updates or over-the-air updates through firmware updates
  * **Product Release Instrucion (PRI) updates**
    - modify a host of complex CDMA device settings
  * **IMEI**
    - International Mobile Equipment Identity
    - 15-digit number used to uniquely identify a mobile device and to block that device from
     accessing the carrier's network
    - use GSM (global system for mobile communications)
  * **ICCID**
    - Integrated Circuit Card Identifier identifies a subscriber identity module (SIM)
    - SIM contains information unique to the subscriber
  * **IMSI**
    - International Mobile Subscriber Identity
    - included on the SIM, but represents the actual user associated with the SIM
    - not normally accessible from the phone
  * MDM software
    - mobile device management collects the identifiers along with other device info  (such
     as MAC address or phone number) during the provisioning process and stores them in device
     inventory

VPN
---
  * in a client VPN setup, the host has client VPN software specially configured to match the
    corporate VPN server or concentrator configuration
  * site-to-stie VPN uses VPN devices on both ends of the connection configured to communicate
    only with each other
  * when using L2TP/IPsec method, UDP port 1701 is used and must be opened on packet-filtering
    devices
  * SSL/TLS-based VPN uses the standard SSL/TSL port, TCP 443, and typically used through a
    client's Web browser but don't give the client the same direct access to resources on the
    corporate network
* SoC (system on a chip): combine CPU, GPU and other support logic onto single silicon die
* iCloud Key Chain
  * builds on the Key Chain feature in macOS to synchronize user info, passwords, payment info
  * can store amy non-Apple credentials as well
* data roaming
  * jump from cell tower to tower and from the provider to another provider without notice
  * no big deal within own country where competing providers have inexpensive roaming agreement

E-mail
------
  * iOS: iCloud, Android: Gmail account, Windows: Exchange Online e-mail
  * neither POP3 nor IMAP4 is suggested in Apple
  * Android's Gmail can talk to other, non-Gmail e-mail services for setting up Exchange, POP3,
    or IMAP4 accounts
  * latest versions of Android simply query the e-mail server to configure port numbers and
    security types automatically
  * POP3: TCP port 110, IMAP4: TCP port 143, SMTP: TCP port 25
  * many servers block default ports
  * secure POP3: TCP port 995, secure IMAP4: TCP port 993, secure SMTP: TCP port 465 or 587
  * Secure/Multipurpose Internet Mail Extensions (S/MIME): use to configure digital signature
    settings for e-mail and contacts

Exchange ActiveSync (EAS)
-------------------------
  * Microsoft protocol used to synchronize Microsoft Exchange e-mail, contacts and calendars
  * originally developed as synchronization protocol for Microsoft Exchange corporate users

Synchronization methods
-----------------------
  * in old days, mobile devices were synchronized to a desktop
  * now, can skip the desktop and sync even large amounts of data to the cloud
  * have to be careful of syncing over insecure public wireless networks
  * single sign-on (SSO): process for using active authenticated session with one the common
    services to sign into other services
  * **issues**
    - most issues are connectivity, device or remote infrastructure problem that leaves data
      partially synced
    - could result in incompletely downloaded e-mail or duplicate
    - authentication issues, OS version issues, incorrect config settings, remote end of the
     connection problem or multiple sources trying to sync the same data can cause issues
* Lightning connector
  * an 8pin connector, replaced the older 30pin dock connector on iPhones and iPads
  * licensed to toher manufacturers through the made for iPhone (MFi) program
  * contains small chip that identifies it as a true lightning connector
  * cables without the chip won't work or will only have limited use
* Typ-C
  * support USB 3.1 with fast data transfer rates up to 10Gbps
  * some devices using Type-C connector are using it with USB 2.0
* NFC (near field communication)
  * uses chips embedded in mobile devices that create electromagnetic fields when devices are
    close to each other
  * distance from few centimeters to few inches
* magnetic reader or chip reader enable quick credit card transactions via the cellular network
* infrared
  * used to create the first real personal area networks (PANs)
  * use the wireless Infrared Data Association (IrDA) standard
  * requires line of sight
  * infrared blaster in modern devices is capable of emitting but not receiving infrared signal
* Android an Microsoft mobile devices offer the ability to use removable external storage
* Qi standard from the Wireless Power Consortium (WPC) is settled as the wireless charging
  standard of choice
`back to top <#a+top>`_

### 25. Troubleshooting Mobile Devices

* various processes that power the recent out-of-focus apps may keep running if they have work
  to do, be cached until return to the app or get killed if the OS needs its resources for
  other apps
* Application Manager will let to force stop an app, also killing its background processes
* Soft reset: restarting/rebooting the device
* Hard reset: factory reset or reset to factory default will clear all user data and setting
  changes, not same with hard reboot

Dim display
-----------
  * sometimes app that need special control over brightness settings can cause the device's
    display to be too dark or bright
  * might be a sign that there's a problem with the panel
  * auto-adjustment is affected by how much light a sensor or camera on the front of the device
    can detect
  * try if soft reset returns the display to normal
* poorly installed screen protector can cause touchscreens not to work properly

Physical damage
---------------
  * drops or impacts can break internal connections
  * moisture can cause internal shorts
  * lingering liquid could cause sensor to behave in strange ways
  * liquid contact indicator (LCI): changes color when exposed to water, often on baattery or
    in the battery compartment

Apps not loading
----------------
  * the app may not be compatible
  * device doesn't meet the app's hardware requirements
  * may need third-party tools to access app log errors

Overheating
-----------
  * charging, large data transfers, frozen apps, recording HD video and other intensive tasks
    can make device hotter
  * put into airplane mode or close all running programs
  * hot enough to burn is usually caused by hardware issue (defective battery or other power
    circuit)

Slow performance
----------------
  * can be caused by almost full storage space
  * can also be caused by too many apps running at the same time, maxing out the RAM
  * hot sluggish device could be using thermal throttling to protect the device's CPU from heat
    damage by reducing its power

Managing battery
----------------
  * how long it will last on each charge
  * how long the battery can meet the needs before having to replace it
  * OLED displays use less power to display darker colors
  * using black wallpapers and configuring apps to use dark theme could reduce power usage
  * each communication technology draws varying amounts of energy under different circumstances
  * searching for signal is power intensive
  * less-accurate location methods like nearby cellullar and Wi-Fi networks use low-power
  * higher-accuracy methods like GPS use higher-power
  * main cause of swollen battery is overcharging
  * Non-OEM chargers or batteries represent an additional rist if they aren't rated for correct
    voltage and wattage
* sometimes operating system issues can cause device freezes, especially right after a device
  update

Cannot broadcast to external monitor
------------------------------------
  * check source correct on the external monitor
  * use right adapter for the device
  * check if adapter needs its own power source

No sound from speakers
----------------------
  * volume was turned down or muted through software config or an app
  * device may have separate settings for media, call volume, notifications

Connectivity & data usage issues
--------------------------------
  * primary symptoms of weak signal are dropped connections, delays, slow transmission speeds,
    frequent indications that the device is searching for a signal
  * can also be caused by performance problems
  * overloaded network is common during large public gatherings
  * data transmission over limit: problems despite a strong signal, restrictions and limits
    the carrier enforces

Location services problems
--------------------------
  * inaccuracy can be caused by the hardware a device has available
  * a Wi-Fi only tablet with no GPS can get only general location
  * incorrect OS config settings for GPS, cellular, Wi-Fi services may prevent location
    services from functioning properly

System lockout
--------------
  * occurs when too many consecutive login attempts fail
  * to protect the device from brute-force attacks
  * if the device is centrally controlled through MDM software, the organization has the
    ability to remotely unlock the device

Encryption problems
-------------------
  * e-mail messages are encrypted using standard such as Pretty Good Privacy (PGP) or
    Secure/Multipurpose Internet Mail Extensions (S/MIME)
  * a key is a string of bits used by a computer program to encrypt or decrypt data

BYOD vs Corporate-owned devices
-------------------------------
  * before Bring Your Own Device, IT assets belonged to the company, not individual
  * some companies enforce a policy prohibiting the use of persoanl devices to access corporate
    data and resources
  * others allow the use of personal devices to save money
  * how much control the corporation has versus the individual
    - if corporate data is stored on the device, the organization should have some degree of
     control over it
    - if the device also belongs to the employee, then the employee should have some control
  * who pays for the device and its use
    - best solved via formal policy and procedures
  * how to handle employee privacy
    - can the organization see private data or remotely access user personal device
  * **MDM (mobile device management) policies**
    - often combine a specialized app on the device and specialized infrastructure to deal
     with the devices
    - infrom corporate versus end-user device configuration options
    - MDM policies are a big deal at the big organization level

Profile security requirements
-----------------------------
  * profile: collection of configuration and security settings
  * created through MDM software or in a program such as Apple Configurator
  * typically text-based files in XML (extensible markup language) format
  * profiles specific to certain platforms, OSs or devices
  * profiles that are specific to different user categories or management groupings
  * group-specific profiles to external users
  * device and user-specific profiles can be very helpful in managing larger groups of users
  * when multiple porfiles are applied, some settings will conflict
  * pay attentions to profile precedence and configure the MDM server to resolve conflicts
    using criteria
  * should also develop profiles that apply to corporate-owned versus employee-owned devices

Malware
-------
  * the best malware accomplishes its objective without anyone detecting it
  * tight controls on the OS and apps make traditional malware infections almost impossible on
    iOS and Windows phone devices
  * most important part of an enterprise-level anti-malware solution is delivering timely
    updates to the devices on a routine basis
  * modern iOS and Android use full device encryption to protect the built-in storage
  * app scanner looks through the permissions requested by the apps to assess the risk they
    pose to security and privacy
  * app scanners can also tell what type of data access the app has to personal info
  * malware is just software or program and can cause resource issues

Multifactor authentication
--------------------------
  * **knowledge factor**
    - something the user knows such as user name, password, DOB, social security number
  * **ownership or possession**
    - something user has in possession such as smard card or token
  * **inherence**
    - something the user either is or something they do
  * also referred to as something the user knows, something the user has, something the user is
  * location factor
    - may be required to be at certain loation to log in the system
  * temporal factor
    - logon at a certain time of day

Firewalls
---------
  * generally, mobile devices don't use a firewall as they don't have lots of services
    listening on open ports
  * software firewall packages work at very basic level and can't contain every network threat

Unintended connections
----------------------
  * **tower spoofing**
    - setting up equipment to spoof a carrier's tower and infrastructure and
     cause a cellular device to sue it instead of the normal tower equipment
    - can eaves drop on conversations, even encrypted
    - can fool the device into turning off encryption completely and sophisticated attacks can
     even install malware
    - Stingray: device to intercept a suspect's cell traffic using tower spoofing
    - Dirtbox: aircraft-mounted version of Stingray
  * geofencing: tacking when the user enter or exit a predefined area
`back to top <#a+top>`_

### 26. Printers

* what a printer can and can't do is largely determined by the type of printer technology it
  uses, how it gets the image onto the paper
* duplex assemblies enable the printer to auto print on both sides of the paper
* print resolution: how densely the printer lays down ink on the page, mesured in horizontal
  and vertical dpi (dots per inch)
* print speed: measured in ppm (pages per minute)

Impact printers
---------------
  * create an image on paper by physically striking an ink ribbon against the paper's surface
  * daisy-wheel printers: electric typewriter attached to the computer instead of keyboard
  * impact printers are relatively slow and noisy
  * POS (point of sale) machines use special impact paper that can print receipts in duplicate,
    triplicate or more
  * **dot-matrix printers**
    -large installed base in business, can be used for multipart forms as they actually strike
     the paper
    - printwires: a grid or matrix of tiny pins to strike an inked printer ribbon on paper
    - printhead: the case that holds the printwires
    - either 9 or 24 pins
    - 9pin dot-matrix printers are called draft quality
    - 24pin printers are calle dletter quality or near-letter quality (NLQ)
    - the more pins, the higher the resolution
    - tractor-feed paper: continuous-feed paper with holes on its sides

Inkjet printers
---------------
  * uses a printhead connected to a carriage that contains the ink
  * roller grabs paper from paper tray
  * use heat to move the ink, while a few use a mechanical method
  * heat-method printers use tiny resistors or electroconductive pates at the end of each tube
  * ink is stored in small containers called ink cartridges
  * separate the ink colors into three separate cartridges, CMYK, three for CMY and one for K
  * printers using more ink cartridges produce higher-quality printed images
  * can support many print media
  * all inkjet inks are water-based and water works better than denatured alcohol to clean up

Dye-sublimation printers
------------------------
  * also called dye transfer printing
  * used mainly for photo printing, high-end desktop publishing, medical and scientific
    imaging, and where fine detail and rich color more important than cost and speed
  * snapshot printers: smaller printers that use dye-sublimation for printing photos
  * uses a roll of heat-sensitive plastic film
  * process requires one pass per page for each color
  * some use final finishing pass that applies a protective laminate coating to the page
  * continuous-tone images: image is not constructed of pixel dots but a continuous blend of
    overlaid differing dye colors

Thermal printers
----------------
  * use a heated printhead to create high-quality image on special or plain paper
  * direct thermal printers use a heating element to burn dots into the surfcae of special
    heat-sensitive thermal paper
  * many retail businesses still use it as receipt printer
  * thermal wax printers
    - work similary to dye-sublimation printers but instead of using rolls of dyed-embedded
     film, the film is coated with colored wax
    - don't require special papers
    - output not quite good
    - also need to care for the wax ribbon
  * take care of heating element, the rollers and the paper

Laser printers
--------------
  * use a process called electro-photographic imaging for high-quality and high-speed output
  * photoconductive: particles of these compounds, when exposed to light, will conduct
    electricity
  * usually use lasers as light source because of precision, lower-cost ones use LED arrays
  * color laser printers have four toner cartridges: black, cyan, magenta, yellow
  * **toner cartridge**
    - may other parts that suffer most wear and tear have been incoporated into the cartridge
  * **imaging drum**
    - aluminum cylinder coated with particles of photosensitive compounds
    - whatever electrical charge the particles may have drains through the grounded cylinder
  * **erase lamp**
    - exposes the entire surface of the imaging drum to light
  * **primary corona/charge roller**
    - located close to the photosensitive drum, never touches the drum
    - when charged with high voltage, and electric field (corona) forms, enabling voltage to
     pass to the drum and charge the photosensitive particles
    - uniform negative voltage of between ~600 and ~1000 volts
  * **laser**
    - acts as the writing mecahnism
    - when particles on the drum are struck by the laser, they are discharged and left with a
     ~100volt negative charge
    - wirtes a positive image to the drum
  * **toner**
    - fine powder made up of plastic particles bonded to pigment particles
    - negative charge of between ~200 and ~500V
    - particles of toner are attracted to the areas of photosensitive drum hit by the laser
    - black toner is usually carbon mixed into polyester resin
  * **transfer coronoa/transfer roller**
    - in old printers, a thin wire applied a positive charge to the paper, drawing the
     negatively charged toner particles onto the paper
    - newer printers use transfer roller that draws the toner onto the paper
    - static charge eliminator: removes the charge from the paper to prevent the paper from
      wrapping around the drum
    - located outside the toner cartridge
    - prone to build-up of dirt, toner and debris through electrostatic attraction
  * **fuser assemblly**
    - almost always separate from the toner cartridge
    - close to the bottom of the toner cartridge and usually has two rollers to fuse the toner
    - pressure roller: presse against the bottom of the page
    - heated roller: presse down on the top of the page, melting the toner into the paper, has
     a nonstick coating such as Teflon to prevent the toner from sticking
  * **turning gears**
    - *pickup roller* grab the paper and pass over to the *separation pad*
    - photosensitive roller is turned and the laser, or a mirror is moved back and forth
    - toner is evenly distributed and the fuser assembly squish the toner into the paper
    - paper come out and the assembly is cleaned to prepare for next page
    - gear packs or gear boxes:  discrete units gear systems are packed together in
    - most laser printers have two or three gearboxes
  * duplex printing: print on both sides of paper, available in sophisticated laser printers
  * **system board**
    - main processor, printer ROM, RAM used to store image before printing
    - many printers divide the functions among two or three boards
    - older printer may also have extra ROM chip for functions such as PostScript
  * **ozone filter**
    - coronas inside laser printers generate ozone (O3)
    - needs to be vacuumed or replaced periodically
  * **sensors & switches**
    - used to detect conditions such as paper jams, empyt paper trays, or low toner levels
  * **raster images**
    - a pattern of dots of the page, representing what the final product should look like
    - printer has to paint the entire surface of photosensitive drum before it can begin to
     transfer the image to paper
    - raster image processor (RIP): translate the raster image into commands to the laser
    - inkjet printer also has a RIP but it's part of the driver instead of hardware
    - RIP needs memory to store the data that it must process
    - insufficient memory will be indicated by a memory overflow (MEM OVERFLOW) error
  * **resolution**
    - 2400x600 dpi, 1200x1200 dpi
    - first number, horizontal resolution, is determined by how fine a focus can be achieved
     by the laser
    - second number is determined by the smallest increment by which the drum can be turned
    - laser printers produce better quality than dot-matrix printers because of resolution
     enhancement technology (RET)
    - RET enables the printer to insert smaller dots among the characters, smoothing out the
     jagged curves that are typical of printers that do not use RET
    - RET enables laser printers to output high-quality print jobs but also requires RAM
    - disabling RET will sometimes free up enough memory to complete the print job
  * most models send each page through four different passes, adding one color at each pass
  * others place all colors onto a special transfer belt and transfer them to page in one pass
  * some printers use four separate toner cartridges and four lasers for four toner colors
  * others slays down one color after the other on the same drum

steps of laser printing process
-------------------------------
  1. Processing
     * CPU processes the request and sends a print job
     * printer spooler enables to queue up multiple print job
  2. Charging
    * using the primary corona wire or primary charge roller, a uniform negative charge is
      applied to the entire surface of the drum (between ~600 and ~1000 volts)
  3. Exposing
    * laser is used to create a positive image on the surface of the drum
  4. Developing
    * particles with a lesser negative charge are positively charged relative to the toner
      particles and attract them, creating a developed image
  5. Transferring
    * transfer corona or transfer roller gives the paper a positive charge
    * the negatively charged toner particles leap from the drum to the paper
  6. Fusing
    * as the toner particles are mostly composed of plastic, they can be melted to the page
    * heated roller coated in a nonstick material and a pressure roller melt the toner to paper
    * static charge eliminator removes the paper's positive charge
    * heated roller can melt some types of plastic media, make sure to print on media designed
      for laser printers
  7. Cleaning
    * physical and electrical cleaning of the photosensitive drum
    * if residual particles remain on the drum, they will appear as random black spots and
      streaks on the next page
    * physical cleaning either deposits the residual toner in a debris cavity or recycles it by
      returning it to the toner supply in the toner cartridge
    * one or more erase lamps bombard the surface of the drum with appropriate wavelengths of
      light, causing the surface particles to discharge into the grounded drum

3D printers
-----------
  * most common 3D printers use plastic filament on spools
  * most manufacturer's 3D printing software enables to use standard 3D drawings such as STL,
    OBJ, CAD files

Virtual printers
----------------
  * a program that converts the output into specific format and saves the result to a protable
    file that looks like the printed page
  * good for saving reference copies of information found on the Web
  * print to PDF, print to Image
  * cloud and remote printing
    - sends the document over the Internet to a cloud server which ends up routing it to a
     real printer
    - eg. Google Cloud Print

Printer Languages
-----------------
  * **ASCII**
    - American Standard Code for Information Interchange
    - ASCII code 10 (0A in hex): Line Feed
    - ASCII code 12 (0C in hex): Form Feed
    - control codes are extremely limited
  * **PostScript**
    - developed by Adobe in early 1980s as device-independent printer language
    - PostScript interpreters are embedded in the printing device
    - print faster as majority of image processing is done by the printer
  * **PCL**
    - Printer Command Language developed by HP as more advanced than ASCII codes
    - text-based output, does not support advanced graphical functions
    - not a true page description language
    - commands must be supported by each individual printer model
  * **Windows GDI & XPS**
    - **Graphical Device Interface**
       + component of the OS, uses the CPU rather than the printer
       + processes the print job and sends bitmapped images of each page to the printer
    - **XML Paper Specification**
       + improvements over GDI, including enhanced color management
       + requires a drive that supports XPS
       + some printers natively support XPS

Scanners
--------
  * TWAIN: Technology Without an Interesting Name, default driver type for a long time
  * optical character recognition (OCR): a way to scan a document and have the computer turn
    the picture into text
  * primary things when choosing scanner: resolution, color, depth, grayscale depth, scan speed
  * scanner convert the scanned image into a grid of pixels
  * older scanners can create images of only 600x600 dpi
  * resolution: enhanced resolution is assisted from some onboard software
  * **color depth**
    - defines the number of bits of info the scanner can use to describe each individual pixel
    - determines color, shade, hue
    - 24bit: can save up to 256 shades, total of 2^24 color variations
    - 48bit: up to 65536 shades per subpixel, total of 2^48 color variations, twice the size
     of 24bit scans
  * **grayscale depth**
    - defines how many shades of gray the scanner can save per pixel
    - important if working with bnw images
    - 8bit, 12bit, 16bit grayscale varieties
  * **scanning speed**
    - maximum speed is hard-coded into the scanner
    - time required to complete a scan is alos affected by the parameters set
    - typical low-end scanner takes upward of 30s to scan a 4x6 photo at 300dpi

Fax
---
  * requires separate functions such as document feed and a connection to a traditional analog
    phone line
  * automatic document feeder (ADF): grab pages to copy, scan or fax

connections
-----------
  * most printers use standard USB type A on one end and smaller USB type B on other
  * typical network printer comes with its own built-in 802.11(a, b, g, n ,ac) Wi-Fi adapter
  * some include onboard network adapter that uses RJ-45
  * some printers offer Bluetooth interface for networking
  * **print server**
    - standalone network device to connect the printer to the network
    - printer connected to it may not be able to use all features
  * old printers use parallel port, serial port or SCSI
* conceptualize each function of MFD (multifunction device) as a separate device to simplify
  troubleshooting
* one printer can support multiple print devices, enabling a system to act as print server
* Microsoft suggests changing the Group Policy Driver Installation policy to allow non-admins
  to install drivers for printers

installing a local printer
--------------------------
  * most commonly used to install standalone network printers using an IP address
  * can share a standalone network printer connected to the computer via TCP/IP port using
    File and Printer Sharing
  * Apple's AirPrint can be used in conjunction with its Bonjour Print Service
* process for sharing local printer and network printer is identical because Windows consider
  both printers to be installed on the computer and user control

printer emulation
-----------------
  * using a substitute printer driver
  * some new printers do not come with their own drivers, instead emulate a well-known printer
  * may see emulation in the "I don't have the right driver!" scenario
  * printers may require to be set into emulation mode to handle a driver other than native one
* sharing a printer or MFD in a SOHO LAN is easy
* management of may printers is a pain unless map via a group policy that applies to a lot of
  computers or users
* Print Management Console: Windows tool that enables to view and modify all the printers and
  drivers on the system or connected to the network

print settings
--------------
  * Layout: control how th printer determines what to print where
  * Paper: telling printer what kind of paper will be used, especially if the printer has
    multiple paper trays and wher to find it
  * what is seen on the screen may not match what comes out of the printer unless both devices
    are properly calibrated

Calibration
-----------
  * manual or automatic process that corrects differences between how a device or component
    currently works and how it should work
  * color calibration uses hardware to generate an Internation Color Consortium (ICC) color
    profile, which is used by the OS to correct any color shifts in the monitor
  * Windows Color System (WCS)
    - help build color profiles for use across devices
    - based on color infrastructure and translation engine (CITE)

Data privacy
------------
  * it's common for modern devices to contain a hard drive or other storage media used to cache
    copies of documents the device prints, scans, copies or faxes
  * disable caching feature, schedule regular deletion of cache, or manually clear

troubleshooting
---------------
  * backed-up print queue can easily overflow or become corrupt due to a lack of disk space, too
    many print jobs, or other factors
  * print job that comes out an unexpected size usually points to a user mistake in setting up
  * misaligned or garbage prints point to a corrupted or incorrect driver
  * Materiall Safety Data Sheet (MSDS): provide detailed info about not only the potential
    environmental hazards associated with different components but also proper disposal methods
  * never lubricate the printhead
  * if the characters look chopped off at the top or bottom, the printhead needs to be adjusted
  * dark ghosting can sometimes be caused by a damaged drum
  * white vertical lines usually occur when the toner is clogged
  * blotches are commonly result of uneven dispersion of toner, especially if toner is low
  * if spots appear, the drum may be damaged or some toner may be stuck to the fuser rollers
  * embossed effect is caused by foreign object on roller or contrast control set too high
  * incorrect media cause warped, overprinted or poorly formed characters
  * run the self-test to check for connectivity and configuration problems
`back to top <#a+top>`_

### 27. Securing Computers


Unauthorized access
-------------------
  * not all unauthorized access is malicious
  * one way to gain unauthorized access is intrusion
  * dumpster diving: generic term for searching refuse for information as the trash is the
    place to look when it comes to getting info
  * shoulder surfing: observing someone's screen or keyboard for information

Social engineering
------------------
  * process of using or manipulating people inside the organization to gain access to its
    network or facilities
  * social engineering attacks are often used together
  * **Infiltration**
    - use impersonation, tailgating is common form of infiltration
    - mantrap: facilities often install at the entrance to sensitive areas
    - a small room with a set of two doors, one to the outside, unsecured area and one to the
     inner, secure area
    - outdoor must be closed before the inner door can be opened
    - often controlled by a security who keeps an entry control roster, a document which keeps
     record of all comings and goinds from the building
  * **Telephone scams**
    - most common attack
  * **Phising**
    - act of trying to get people to give their user names, passwords, or other security info
     by pretending to be someone else electronically
    - most common form done today
    - spear phising: targeted attacks, the bait can be carefully tailored using details from
     the target's life

Denial of Service
-----------------
  * uses various methods to overwhelm a system, such as a Web server, to make it nonfunctional
  * DDoS (distributed denial of service): use many machines simultaneously to assault a system

Data destruction
----------------
  * means more than just intentionally or accidentally erasing or corrupting data
  * dangerous when users are not clearly informed about the extent to which they are authorized
    to make changes

Administrative access
---------------------
  * to minimize both the number of accounts with full control and the time they spend logged in

Catastrophic hardware failures
------------------------------
  * create redundancy in areas prone to failure
  * perform important data backups

Physical theft
--------------
  * best network software security measures can be useless if fail to protect the systems
    physically

Malware
-------
  *  networks are the fastes and most efficient vehicles for transferring computer viruses
    among systems
  * **Virus**
    - program that replicate and activate
    - does not self-replicate across networks
    - needs human action to spread
  * **Worm**
    - does not need to attach itself to other programs to replicate it
    - can replicate on its own through networks
  * **Trojan Horse**
    - appears or pretends to do one thing while, at the same time, does something dangerous
    - installed Trojan horses do not replicate
  * **Keylogger**
    - recording user's keystrokes
    - as part of other malware as well
    - parental control tools use keyloggers
  * **Rootkit**
    - for malware to succeed, it needs to come up with some method to hide itself
    - rootkit is a program that takes advantage of very low-level OS functions to hide itself
    - gains privileged access to the computer
    - can strike OS, hypervisors, firmware
  * **Spyware**
    - can use computer's resources to run distributed computing apps, capture keystrokes
    - often sneaks onto systems by being bundled with legitimate software
    - can still have access to the data
  * **Ransomware**
    - encrypts all the data it can gain access to on a system
    - can even encrypt data on mapped network drives
  * **Botnet**
    - not a single type of malware
    - network of infected computers (zombies) under the control of single person or group
    - one of the most common uses if sending spam
    - use the bandwidth of millions of zombie machines spread all around the world
    - also use collective power to attack and demand a ransom
  * attack vector: the route the malware takes to get into and infect the system
  * zero-day attacks: attack on a vulnerability that wasn't already known to the developers
  * **spoofing**
    - process of pretending to be someone or something by placing false info into packets
    - any data sent on a network can be spoofed
    - MAC address, IP address, e-mail address, web address, user names
  * **man-in-the-middle**
    - in MITM attack, attacker taps into communications between two systems, covertly
     intercepting traffic thought to be only between those systems
    - would be a person using software on a wireless network to make all clients think his
     laptop is a WAP
  * **session hijacking**
    - tries to intercept a valid computer session to get authentication info
    - only tries to grab authentication info, not listening in like MITM attack
  * **brute force**
    - method where a threat agent guesses many or all possible values foor some data
    - usually an attempt to crack a password
    - can brute force a search for open ports, network IDs, user names
    - dictionary attack: a form of brute force that guesses every word in a dictionary
    - dictionary: used to attack passwords might contain every password ever leaked online
    - hashes are special as the computation that creates them is irreversible
    - hash tables: pre-computed large lookup tables of passwords
    - hash tables for passwords more than a few characters long use tons of storage space
    - rainbow tables: save space, use complicated math to condense dictionary tables with
     hashed entries, binary files, not text files
    - rainbow tables are used to reverse password hashes, only a real threat to older/legacy
     systems and useless for reversing properly salted hashes
  * some pop-up browser windows are designed to mimic similar pop-up alerts from OS
  * drive-by downloads: unwanted, unknown or unplanned file downloads
  * **spams**
    - unsolicited e-mail, accounts for a huge percentage of traffic on the Internet
    - most e-mail programs can block from specific people, can block by subject or keywords
    - e-mail is still common source of viruses and opening infected one is a common way to get
     infected
  * malware's biggest strength is its flexibility, it can look like anything
  * malware doesn't always cause system crashes, some tries to rename system files, change file
    permissions, or hide files completely
  * **symptons of malware**
    - Windows update feature stops working
    - other tools and utilities throw up an "Access Denied" block
    - lose all Internet connectivity
  * infection might overwrite hosts file, which overrules any DNS settings and can redirect
    browser to whatever site the malware adds to the file
  * **prevention**
    - only way to permanently protect is to disconnect from the Internet
    - keep systems under control patched and up to date through proper patch management
    - secure DNS can describe software or a remote DNS provider that implements additional
     filtering to block devices from visiting all kinds of malicious sites
    - all antivirus programs include terminate-and-stay-resident programs (TSRs) that run
     every time the PC is booted
  * **recovery**
    - identify and research malware symptons
    - quarantine the infected systems
       + PacketFence: auto monitors network traffic and can cut a machine off the network if
         it starts sending suspicious packets
    - disable system restore (in Windows)
       + not to let the virus to be included in any saved restore points
    - remediate the infected systems, update the anti-malware software, scan and use removal
     techniques (safe mode, preinstallation environment)
       + get to safe mode and run anti-malware software
       + replace corrupted Windows Registry files or even startup files
    - schedule scans and run updates
    - enable system restore and create a restore point (in Windows)
    - educate the end user
  * **anit-malware programs**
    - work in an active seek-and-destroy mode and in a passive sentry mode
    - scans the computer's boot sector and files for viruses
    - operate as virus shields that passively monitor a computer's activity, checking for
     viruses only when certain events occur
    - detect boot sector viruses by comparing the drive's boot sector to a standard one as
     most boot sectors are basically the same
    - executable viruses are more difficult to find because they can be on any file in drive
    - signature: code pattern of a known virus
    - the program compares an executable file to its library of signatures
    - definition file: list of virus signatures the program can recognize
  * **Polymorphic/Polymorphs**
    - attempts to change its signature to prevent detection by antivirus programs, usually by
     continually scrambling a bit of useless code
    - scrambling code itself can be identified and used as the signature
    - to combat unknown ploymorphs, have the antivirus program create a checksum on every file
     in the drive
    - checksum: a number generated by the software based on the contents of the file rather
     than the name, date or size
    - every time a program is run, the antivirus program calculates a new checksum and
     compares it with earlier calculation
  * **Stealth**
    - more of concept that an actual virus function
    - more stealth virus programs are boot sector viruses that use varous methods to hide
  * **Firewalls**
    - devices or software that protect an internal network from unauthorized access to and
     from the Internet
    - use a number of methods to protect networks, such as hiding IP and blocking TCP/IP ports
    - hardware firewalls, often built into routers, and software firewalls
    - both types can be run at the same time
    - hardware firewall protect a LAN from outside threats by filtering the packets before
     they reach internal machines, use Stateful Packet Inspection (SPI) to inspect each
     incoming packet individually
    - SPI also blocks any incoming traffic that isn't in response to outgoing traffic
    - Port forwarding enables to open a port in the firewall and direct incoming traffic on
     that port to a specific IP address on LAN
    - Port triggering enables to open an incoming connection to one computer automatically
     based on a specific outgoing connection
    - trigger port: outgoing connection, destination port: incoming connection
    - demilitarized zone (DMZ) puts systems with specified IP outside the protection of the
     firewall, opening all ports and enabling all incoming traffics
    - any PC inside the DMZ will be completely exposed to outside attacks
    - software firewall exceptions: choosing which programs and services can pass through
    - most programs installed auto add themselves to exceptions list
    - Windows network types: Domain, Private, Guest or Public
    - even with Network Discovery activated, several firewall settings can overrule certain
     connections
    - do nothing to stop interceptor hackers who monitor traffic on the public Internet
  * **Intrusion detection system (IDS)**
    - Internet app that inspects packes, looking for active intrusions
    - functions inside the network, watching for threats that a firewall might miss
    - can discover internal threats
    - some IDS offer a pop-up message, e-mail or text message
    - cannot stop the attack directly, but can request assistance from other devices
  * **Intrusion prevention system (IPS)**
    - similar to IDS, but sits directly in the flow of network traffic
    - can stop an attack while it is happening
    - network bandwidth and latency take a hit
    - if the IPS goes down, the network link might go down too
    - can block incoming packets on-the-fly based on IP, port number or app type
  * **Unified threat management (UTM)**
    - takes the traditional firewall and packaes it with many other security services such as
     IPS, VPN, load balancing, antivirus
    - helps build robust security deep into the network, protecting the data

Environmental threats
---------------------
  * a computer works best in an environment where the air is clean, dry and room temperature
  * fans will collect dust and dirt quickly
  * use compressed air or nonstatic vacuum to clean
  * make sure room is ventilated and air-conditioned and air filters are changed regularly
  * can enclose a system in a dust shield
  * most computers are designed to operate at room temperature, around area 22&degree;C
    (72&degree;F) with relative humidity in the 30-40 percent range

Access control
--------------
  * physical security: limit access to physical hardware
  * **authentication**
    - security tokens: devices that store unique info that the user carries on person, may be
     digital certificates, passwords or biometric data
    - RSA tokens: random-number generators that are used with user names and passwords to
     ensure extra security
    - most security hardware tokens come in the form of key fobs
  * **users and groups**
    - access control list (ACL): only extends to drives/cards formatted with modern file
     systems such as NTFS, HFS+, ext3/4
    - accounts should have permission to access only the resources they need
    - in all OSs, permissions of the groups are combined into the effective permissions the
     user has to access a resource
    - watch out for default user accounts and groups as they can become backdoors
    - always make sure the Guest account is disabled
    - never use default groups (Everyone, Guest, Users) unless intned to permit all to access
     a resource
  * **security policies**
    - policies as permissions for activities
    - usually applied to a user account, a computer or a group
    - every Windows client has its own local policies program
    - local policies work great for individual systems
    - set up to features of domain-based Windows Active Directory for large systems
    - use organizational units (OUs) that organize users and devices logically into a
     folder-like hierarchy
    - group policy changes may not immediately apply to all systems
    - Windows wil fetch group policy when the system boots or someone logs in
    - refresh the policy from time to time while running
    - run "gpupdate /force" from cmd to update immediately
    - group policy can set default wallpaper for every PC, make certain tools inaccessible,
     control access to the Internet, redirect home folders, run scripts, deploy software
    - common policies on Windows: prevent registry edits, prevent access to the command
     prompt, log on locally, shut down system, minimum password length, account lockout
     threshold, disable windows installer, printer browsing

Data classification & compliance
--------------------------------
  * data classification: organizing data according to its sensitivity and making certain that
    computer hardware and software stay as uniform as possible
  * common schemes: public, internal use only, highly confidential, top secret
  * most common compliance issue revolves around software

Licensing
---------
  * Commercial licensing
    - legal obligation to pay money for access to the software
  * End User License Agreement (EULA)
    - abide by the use and sharing guidelines stipulated by the software copyright holder
  * digital rights management (DRM)
    - enforce how to use commercial software
  * Non-commercial licensing
    - many programs are only free for personal use
  * open source
    - allow to take the original code and modify it
  * closed source
    - cannot modify the source code or make it part of some other software suite

Incident Response
-----------------
  * policies and procedures to deal with negative events that affect networks and systems
  * **Auditing**
    - event auditing: Windows create an entry in the Security Log when certain events happen
    - object access auditing: create an entry in when a user tries to access file or folder
  * **Incident reporting**
    - involves use of documentation changes
    - provides a record of work accomplished
    - provides a piece of info that, when combined with others, reveals a pattern or bigger
     problems
  * Acceptable Use Policy (AUP): defines what actions employees may or may not perfrom on
    company equipment
  * chain of custody: documented history of who has been in possession of the system, tracking
    of evidence/documenting process
  * **Data Loss Prevention (DLP)**
    - context-based set of rules to help companies avoid accidental leakage of data
    - works by scanning packets flowing out of the network, stopping the flow with triggers

Network encryption
------------------
  * occurs at many levels and not limited to Internet-based activities
  * one of the most complicated of all networking issues
  * modern OSs such as Windows and macOS use standard network authentication encryptions such
    as MIT's Kerberos
  * encryption methods don't stop at the authentication level
  * encryption method depends largely on by what method the communicating systems will connect
    with
  * many networks are linked together by some private WAN connection such as old T1s or Metro
    Ethernet
  * IPsec provides transparent encryption between the server and the client
  * VPN can also use IPsec, but typically use other encryption methods

Application encryption
----------------------
  * SSL (Secure Sockets Layer): security protocol used to secure Web sites
  * TLS (Transport Layer Security) is used in HTTPS, replaces TLS
  * server sends a public key to the browser so the browser knows how to decrypt the incoming
    data
  * public keys are sent in the form of digital certificate, which is signed by a trusted
    certificate authority (CA) that gurantees that key is actually from the Web server (such as
    Symantec which is formerly known as VeriSign, Comodo)
  * trusted root CAs: built-in list of trusted certificate authorities in browser

Wireless issues
---------------
  * set up wireless encryption, disable DHCP, only allot enough DHCP addresses
  * change WAP default SSID, filter by MAC address, change default user name and password
  * update firmware if needed, turn on WAP firewall settings
  * configure SOHO router NAT/DNAT settings, use SOHO router content filtering
  * consider physical security of SOHO router
`back to top <#a+top>`_

### 28. Operational Procedures

* organizations need documentations to provide continuity and structure

Network documentation
---------------------
  * provide a road map for current and future techs who need to make changes or repairs
  * **Network topology diagrams**
    - provide a map for how everything connects in organization's work
    - include switches, routers, WAPs, servers, workstations
    - describe connection types and speeds and technologies in use
    - most use Cisco icon set as a universal visual aid
* use documentations to enable cooperation among employees and coordiantion among departments
* knowledge base:  a set of documents that tell the equipments used, problems encountered and
  solutions to those problems
* incident documentation: helps current and future techs to deal with problematic hardware and
  individuals

Company policies
----------------
  * **regulatory compliance**
    - specify how organizations are supposed to manage their workplaces, workers and materials
  * **acceptable use policies**
    - AUP describe what employees can and cannot do with company property
    - often very detailed and specific documents that employees must agree to and sign as a
     step in the employment process
  * **password policies**
    - how long and complex passwords should be
    - how to compose passwords, when to use multifactor authentication
    - whether passwords should expire, whether to set a BIOS password

Inventory management
--------------------
  * **barcodes**
    - binary code that can be scanned
    - read-only
  * **asset tags**
    - can use the radio frequency identification (RFID) wireless networking protocol to keep
     track of inventory
    - most RFID tags are passive, the tag receives all the power it needs fron the scanner's
     signal
    - active RFID uses a battery or external power source to send out and receive signals
    - info stored in an RFID tag can be updated

Change management process
-------------------------
  * enables organizations to implement changes to IT infrastructure in a safe and
    cost-effective manner
  * consult documented business processes
  * determine the purpose of the change
    - need to document purposes
  * analyze the scope of the change
    - defines who and what the change will affect
  * analyze the risk involved
    - detailed assessment of possible problems that could result
    - any risk analysis will almost certainly be passed off to a security person
  * plan for the change
  * educate end users about the need, benefit and cost
    - end-user acceptance is vital for successful change
  * create a dedicated team to manage the change
    - change board consists of techs and representatives from management, IT security and
     administration who meet on regular basis
  * have a backout plan if change fails
    - defines the steps needed to return the infrastructure to its pre-change environment
  * document everything thoroughly

Power protection
----------------
  * should secure all electronic components with a surge protector or UPS
  * every server should run on its own UPS

Backup options
--------------
  * all of the choices involve trade-offs between the risk of data loss and convenience, cost
    and effort
  * keep lots of copies cost more and increase the odds of data leak
  * reduce data loss by taking separate backups in different regions or hardware
  * store backups off-site reduces the risk of complete loss
  * set automatic backups
  * storage is cheap but the cost of storing many copies of frequent backups adds up fast
  * compression, deduplication, incremental backups that only record changes since the previous
  * restoring a specific incremental backup requires an unbroken chain of incremental backups
    from the last full backup
  * **file level**
    - simple way to make sure important files and directories get backed up
    - to backup an installed app with file-level backup, need to know location of the
     application files, Registry settings, temp file locations, startup files, and configs
  * **image level**
    - backup a complete volume including the OS, boot files, all installed apps and all data
    - can restore image to a blank drive
    - images contain gigabytes of OS and program files
    - easy to backup as what to backup is not specified
    - not always restore a full image as it could contain corrupt drivers
  * **critical applications**
    - recognize the critical ones
    - determine the critical data
  * use cloud backup for critical data and local storage for daily backups
  * always verify the backups
`back to top <#a+top>`_

##

<a id="network+top"></a>
CompTIA Network+ Cert Notes
===========================

31. `OSI model`_
32. `Network Topologies`_
33. `Ethernet Frames`_
34. `Ethernet Standards`_
35. `Installing physicall network`_
36. `TCP/IP Basic`_
`back to top <#top>`_

### 1. OSI (Open Systems Interconnection) seven-layer model

* sharing host - server, accessing host - client
* powerful mental tool for diagnosing problems
* common language techs use to describe specific network functions
* a model must have at least all the major functions of the real item, but what constitutes
  a major rather than a minor function is open to opinion
* created by ISO (International Organization for Standardization)
* each layer defines an important function and the protocols that operate at that layer offer
  solutions to those functions
* protocols are sets of clearly defined rules, regulations, standards and procedures that
  enable hardware and software developers to make devices and applications that function
  properly at a particular layer
* unit of data specified by a protocol at each layer is called protocol data unit (PDU)

OSI model
---------
  other layers as possible)
  * "All People Seem To Need Data Processing"
  * Layer 7 application  / APIs
  * Layer 6 Presentation / Data conversion
  * Layer 5 sessions     / Session tracking & naming
  * Layer 4 Transport    / Segmentation & reassembly
  * Layer 3 Network      / Router
  * Layer 2 DataLink     / Switch, NIC
  * Layer 1 Physical     / Cabling, hubs

Layer 1 (Physical Layer)
------------------------
  * Layer 1 defines the method of moving data between computers
  * unshielded twisted pair (UTP)
  * anything that moves data from one system to another, such as copper cabling, fiber optics,
    even radio waves, is part of Layer 1
  * doesn't care what data goes through

Layer 2 (DataLink Layer)
------------------------
  * only layer that has sublayers
  * real magic of network starts with the <a id="nic"></a>**network interface card (NIC)**
    * serves as the interface between the PC and the network
    * many authors put NIC in Layer 2
    * media access control address (<a id="mac"></a>**MAC address**)
      - also known as physical address
      - burned onto some type of ROM chip
      - special firmware containing a unique identifier with a 48-bit value
      - always written in hex
      - first six digits represent the number of the NIC manufacturer, referred as
        Organizationally Uniqure Identifier (OUI)
      - last six digits represent manufacturer's unique serial number for the NIC, device ID
      - Windows uses dash as delimiter, Linux and macOS use colon
      - current term for numbering name space is EUI-48 (Extended Unique Identifier)
    * no two NICs ever share the same MAC address
    * request MAC address from Institute of Electrical and Electronics Engineers (IEEE)
    * if we keep two NICs speaking at the same time, only one system could speak at a time
      because all data sent by one NIC is read by every other NIC on the network
    * uses frames to restrict the amount of data a NIC can send at once
    * if NIC doesn't communicate early and doesn't know the destination MAC address, it may
      send a broadcast onto the network to ask for it
    * without knowing the MAC address to begin with, the requesting computer will us an IP to
      pick the target computer out of the crowd
    * sending and receiving jobs NIC performs are broken down into **two jobs**
      1. Logical Link Control (LLC)
           + talks to the system's operating system, usually via device drivers
           + handles multiple network protocols and provides flow control
      2. MAC
           + creates and address the frame
           + it's sublayer adds or checks the FCS
    * operates at both Layer 2 and Layer 1
  * transmit data into chunks called <a id="frame"></a>**frames**
    * encapsulates, puts a wrapper around, information and data for easier data transmission
    * PDU for Layer 2
    * all NICs on the same network must use the same frame type
    * three sections - *header*: begins with the [MAC](#mac) addresses (sender's and recipient's),
      Type field (indicates what's encapsulated in the frame),
      *payload*: Data field (contains what's encapsulated),
      *trailer*: <a id="fcs"></a>**Frame Check Sequence (FCS)**
      - FCS uses type of binary math called a cyclic redundancy check (CRC)
      - only 4 bytes long
      - if the receiving NIC's answer is the same as the CRC of sender, data is good
    * can only hold a certain amount of data, at most 1500 bytes
    * in early days, the frame go into central box, called a **hub**<a id="hub"></a>
      - electronic repeater
      - make an exact copy of the received frame
      - send a copy out of all connected ports except the received origin port
      - every frame sent on a network is received by every [NIC](#nic)
    * the hub was replaced with a smarter device, a **[switch](#switch)**
      - sends the frame only to the interface associated with the destination MAC address
      - operate at Layer 2 because it handles traffic using MAC addresses
    * any frame addressed specifically to another MAC address is called a *unicast frame*,
      one-to-one addressing scheme is called *unicast addressing*
  * any device that deals with a MAC address is part of Layer 2

Layer 3 (Network Layer)
-----------------------
  * to move past the physical [MAC](#mac) addresses and start using logical addressing require
    a **network protocol**
     has to create unique IDs for each system and a set of communication rules
  * *protocol suite*, **TCP/IP** (Transmission Control Protocol/Internet Protocol), is several
    network protocols designed to work together
    * **IP** is the primary logical addressing protocol for TCP/IP
      - makes sure that a piece of data gets to where it needs to go on the network
      - by giving each device a unique numeric id called IP address (logical address)
           + IP uses dotted decimal notation based on four 8-bit numbers
           + each 8-bit number ranges from 0 to 255
           + no two systems on the same network share the same IP address
           + routers use IP address, not the MAC address, to forward data
    * in TCP/IP, each system has two unique IDs, MAC address and IP address
  * containers called <a id="packet"></a>**packets** get created and addressed
    * PDU for Layer 3
    * each IP packet is handed to the NIC, which encloses it in a regular frame,creating a
      packet within a [frame](#frame)
    * the NIC driver, interconnection between hardware and software, knows how to communicate
      with the NIC to send and receive frames, but it can't do anything with the packet

Layer 4 (Transport Layer)
-------------------------
  * sending system does the **segmentation**
     when the server receives a request for data, it chop the data into chunks to fit into a
      [packet](#packet) (eventually into NIC's frame) and hand to the NIC for sending
  * receiving system does the **reassembly**
  * transport protocol breaks the data into segments and gives each segment sequence number
  * **Datagrams** , also created at Layer 4, are simpler and don't get broken into segments
    or get sequence numbers
  * the main job, segmentation/reassembly software, also intializes requests for packets that
    weren't received in good order
  * connection-oriented, TCP, and connectionless protocol, **User Datagram Protocol (UDP)**
    - connection-oriented requires that server and client have connection before sending
    - for connectionless, such as **Voice over IP (VoIP)**, the call is made without verifying
      first whether another device is there
  * **TCP segment**
    - a container in which a chunk of data is left after stripping away the IP addresses
      from an IP packet
    - Source port, Destination port, Sequence number, Acknowledgment number, more info, Data
  * a <a id="port"></a>**port**, has number between 1 and 65,536, is a logical value assigned
    to specific applications or services
  * **UDP datagram** <a id="udp"></a>
    - lacks most of the extra fields found in TCP segments, because it doesn't care if the
      receiving computer gets its data
    - Source port, Destination port, Length, Checksum, Data

Layer 5 (Session Layer)
-----------------------
  * handles all the sessions for a system
     initiates sessions, accepts incoming sessions, and opens and closes existing sessions
  * many OS represent a session using the combination of IP address and [port](#port)
    numbers for both sides of a TCP or [UDP](#udp) communication

Layer 6 (Presentation Layer)
----------------------------
  * translates data from lower layers into a format usable by the
    [Application layer](#layer-7-application-layer) and vice versa
  * a number of protocols function on more than one OSI layer and can include Layer 6
     encryption protocol used in e-commerce, Transport Layer Security (**TLS**) seems to
      initiate at [Layer 5](#layer-5-session-layer) and encrypt and decrypt at Layer 6

Layer 7 (Application Layer)
---------------------------
  * doesn't refer to the applications themselves but to the code built into all operating
    systems that enables network-aware applications
  * **Application Programming Interfaces (APIs)** can be used to make programs network-aware
     provides a standard way for programmers to enhance or extend application's capabilities
* *encapsulation* (includes all the steps from Layer 7 to 2) encompasses the entire process
  of preparing data to go onto a network and reverse process is *decapsulation*
* wireless access point (WAP) operate at Layer 1
`back to top <#network+top>`_

### 2. Network Topologies

* how the cables look (physical topology), how the signals travel electronically (signaling
  topology or logical topology)
* the topology alone doesn't describe all of the features necessary to enable the networks
* it's not the job of topology to define issues like what is cable made of, how long can it
  be, how do machines decided where to send data at specific moment

Bus
---
  * used a single cable that connected all computers in a line
  * data from each computer simply went out on the whole bus
  * need a termination at each end of the cable to prevent a signal sent from one computer
    from reflecting at the ends of the cable, quickly bringing down the network down

Ring
----
  * connected all computers on the network with a ring of cable
  * data traffic moved in a circle from one computer to the next in the same direction, no
    termination required
  * suffer from same problem as the bus: the entire network stopped working if the cable
    broke at any point

Star
----
  * use a central connection box for all the computers on the network
  * benefit over ring and bus by offering *fault tolerance*

Hybrid
------
  * physically, most hybrid designs look like a star but the signals acted like ring or bus
  * any form of networking technology that combines physical topology with a signaling
    topology is called a hybrid topology
  * **Star-ring**
    - most successful star-ring network was called Token Ring by IBM
  * **Star-bus**

Mesh
----
  * every computer connects to every other computer via two or more routes
  * **Partially meshed**
    - at least two machines have redundant connections
  * **Fully meshed**
    - every computer connects directly to every other computer

Network Technology
------------------
  * practical application of a topology and other critical tools that provides a method to
    get data from one computer to another on a network

Cabling and Connectors
----------------------
  * two distinct groups: *copper* and *fiber-optic*
  1. **Copper**
      - most common form, two primary types: *coaxial* and *twisted pair*
      - **Coaxial** (coax for short)
         + contains central copper conductor wire surrounded by an insulating material,
           which, in turn, is surrounded by a braided metal shield
         + shield data transmissions from **electro magnetic interference (EMI)**
           + when a metal wire encounters magnetic fields, electrical current is generated
                 along the wire
           + can shut down a network because it is easily misinterpreted as a signal by
                 devices like NICs
         + old ones use special bayonet-style connectors called **BNC connectors**
           + British Naval Connector or Bayonet Neill-Concelman
         + today primarily same type used to connect televisions to cable boxes or to
               satellite receivers and use **F connectors**
         + all coax cables have a *Radio Guide (RG) rating*
         + only important measure of coax cabling is its **Ohm rating**
           + describes the impedance, which describes a set of characteristics that define
                 how much a cable resists the flow of electricity
         + cable modems connect using one of two coaxial cable types: *RG-59*, *RG-6* (both
               cables are rated at 75 Ohms)
         + can also connect two coax cables together easily using a *barrel connector*
         + **Twinaxial** cable
           + contains two central copper conductors wrapped around a single shield
           + used as a substitute for short fiber connections, between equipment within rack
           + cheaper than fiber and associated hardware
           + also called *direct attached cable (DAC)*
         + **Twisted Pair**
           + each pair works as a team either transmitting or receiving data
           + reduces a specific type of interference called *crosstalk*
           + the more twists per foot, the less crosstalk
           + **Shielded twisted pair (STP)**
             + consists twisted pairs of wires surrounded by shielding to protect from EMI
             + six types differentiated by which part gets shielding, *STP Standard*
             + should use rather than UTP in high-EMI environments
           + **Unshielded twisted pair (UTP)**<a id="utp"></a>
             + consists twisted pairs of wires surrounded by plastic jacket
             + does not provide any protection from EMI
             + most cables come with stranded Kevlar fibers to give the cable added strength
  * several groups set the standard for cabling and networking
     International Organization for Standardization (ISO), American National Standards
      Institute (ANSI), Telecommunications Industry Association (TIA)
  * **Category (Cat) ratings**
    - to help network installers get the right cable for right network technology
    - rated in *megahertz (MHz)*
     Max Frequency | Max bandwidth  | Status with TIA
    - Cat 5:  100 MHz  | 100Mbps        | No longer recognized
    - Cat 5e: 100 MHz  | 1 Gbps         | Recognized
    - Cat 6:  250 MHz  | 10 Gbps        | Recognized
    - Cat 6a: 500 MHz  | 10 Gbps        | Recognized
    - Cat 7:  600 MHz  | 10+ Gbps       | Not recognized
    - Cat 8:  2000 MHz | 25-40 Gbps     | Not recognized
  * **bandwidth**
    - maximum amount of data that goes through the cable per second
    - *bandwidth-efficient encoding scheme*
       + squeezing more bits into the same signal as long as the cable can handle it
  * **Registered jack (RJ)**
    - used in old landline telephones
    - telephones used RJ-11, support up to two paris of UTP
    - current wired networks use four-pair 8 position 8 contact (8P8C) or refer to as *RJ-45*
  2. **Fiber-Optic**
      - transmits light rather than electricity, good for both high-EMI areas and
       long-distance transmissions
      - distances of up to tens of kilometers depending on the implementation
      - four components: glass fiber (the *core*), the *cladding* (part that makes light
       reflect down the fiber), *buffer* material to give strength, the *insulating jacket*
      - manufacturers use a two-number designator to define cables according to their core
       and cladding measurements
      - common sizes: 9/125 &micro;m, 50/125 &micro;m, 62.5/125 &micro;m
      - almost all network technologies that use fiber-optic cable require paris of fibers
       for sending and receiving
      - manufacturers often connect two fibers to create *duplex* fiber-optic cabling
      - light can be sent as regular light or laser light
      - two types of light require different fiber-optic cables
      - color coding for various fiber types
      - **multimode fiber (MMF)**
         + uses LEDs, standard prefix is "OM"
         + can use a form of laser called *vertical-cavity surface-emitting laser (VCSEL)*
         + transmit 850-nm or 1300-nm wavelengths
         + color code: OM1 and OM2 "orange", OM3 and OM4 "sport aqua", OM5 "lime green"
      - **single-mode fiber (SMF)**
         + uses lasers, standard prefix is "OS"
         + prevents a problem unique to multimode fiber optics called *modal distortion*
         + transmit 1310-nm or 1550-nm wavelengths
         + color code "yellow"
      - varous connector types, main four: **ST, SC, LC, MT-RJ**
         + ST and SC connectors have unique ends
         + LC and MT-RJ are always duplex
         + no consensus about initials but,
         + ST (bayonet-style): *straight tip* or *snap and twist*
         + SC (push-in): *subscriber connector* or *standard connector* or *Siemon connector*
           or *stick and click*
         + LC (little): *local connector* or *Lucent connector* or *little connector*
      - **fire ratings**
         + to reduce the risk of cables burning and creating noxious fumes and smoke
         + most common are **poly vinyl cholride (PVC)** and **plenum**
         + PVC ratings has no significant fire protection
         + plenum-rated create much less smoke and fumes, but cost about three to five times
           as much as PVC-rated
         + third type: *riser* provides less protection than plenum
  * IEEE 1284 committee set standards for parallel communication
  * IEEE 802 committee set standards for networking
`back to top <#network+top>`_

### 3. Ethernet Frames
* Brief history
  * developed by Xerox in 1973, standard based on bus topology (where all connected computers
    could see all traffic)
  * used a single piece of coaxial cable with speed up to 3Mbps
  * then came Digital/Intel/Xerox (DIX) standard, speed increased to 10Mbps
  * **IEEE 802.3(Ethernet) standards**
    - wired network standards that share same basic frame type and network access method
    - 802.3i 10Mbps over twisted pair (1990)
    - 802.3ab Gigabit over twisted pair (1999)
    - 802.3by 25 Gigabit over fiber (2016)
    - 802.3cm 400 Gigabit over multimode fiber (2020)
    - 802.3cu 100 Gigabit and 400 Gigabit over singlemode fiber using 100Gbps lanes (2021)
  * **CSMA/CD** (*carrier-sense multiple access with collision detection*)
    - method to determine which machine should access the wire at any given time
    - each node using the network examined the cable before sending data frame
    - mapped to IEEE 802.3
    - disabled in modern full-duplex networks
    - **multiple access**: all machines have equal access to the wire
    - **collision**
       + two machines simultaneously decided the cable is free and send a frame
       + both node stop transmitting
       + each generate a random number to determine how long to wait before trying again
       + lowest number began retransmission first
    - **collision domain**
       + a group of nodes that have the capability of sending frames at the same time as each
         other, resulting in collisions

Issues of frames
----------------
  * prevent any single machine from monopolizing the shared bus cable
  * make the process of retransmitting lost data more efficient
* Transmission of a frame starts with a **preamble** and can also include come extra filler
  called a **pad**
  * **Preamble**
    - 7byte series of alternating ones and zeroes followed by a 1byte start frame
     delimiter or an 8byte series of alternating ones and zeroes
    - gives a receiving NIC time to realize a frame is coming and to know exactly where
     the frame starts
* `MAC Address`_

Type
----
  * helps the receiving computer interpret the frame contents at a very basic level
  * does not tell if the frame carries higher-level data

Data
----
  * contains whatever payload the frame carries
  * will include extra info such as IP address of source and destination if it carries IP
    packet

Pad
---
  * extra data added by NIC if a frame is less than minimum 64 bytes
  * used all the time in modern networking
* `FCS`_

Early Ethernet Standards
------------------------
  * Bus Ethernet
    - original Ethernet networks employed a true bus topology
    - use hybrid star-bus since early 1990s
  * **10BASE-T**
    - consist of two or more computers connected to a central [hub](#hub), star-bus topology
    - 10: speed 10Mbps, BASE: signaling type baseband, T: type of cable used twisted pair
    - required the use of Cat3 or higher two-pair [UTP](#utp)
    - also introduced the **RJ-45** connector (8P8C)
       + connects to a single wire inside the cable
       + numbered from 1 to 8
       + also called a *crimp*
       + installing a crimp onto the end of UTP cable is called *crimping*
       + tool used to secure a crimp is called a *crimper*
       + each pair of wires consists of solid-colored and striped wire: blue/blue-white,
         orange/orange-white, brown/brown-white, green/green-white
       + TIA/EIA defines **termination standard** for correct crimping of four-pair UTP
         + 568A: GW,G,OW,B,BW,O,BrW,Br <a id="568a"></a>
         + 568B: OW,O,GW,B,BW,G,BrW,Br <a id="568b"></a>
    - 10BASE-T use pins 1 and 2 to send data, 3 and 6 to receive data
    - device that was connected to a hub could not send and receive simutaneously
    - **Limits**
       + distance between hub and computer could not exceed 100m
       + hub could not connect more than 1024 computers
  * **10BASE-FL**
    - maximum length up to 2km
    - better security
    - use multimode 62.5/125 &micro;m (OM1) fiber-optic cable, immune to electrical
     interference, and employ SC or ST connector (two fiber connectors, send and receive)
    - hub could not connect more than 1024 computers
  * can use media converter to interconnect 10BASE-T and 10BASE-FL

Extending Ethernet Networks
---------------------------
  * **Issues with hubs**
    - in 10BASE-T network with hub, busy network become slow because all computers share
     the same collision domain
    - adding another hub enable to add more devices or connect networks using a **bridge**
       + acts like a repeater to connect two networks
       + filter and forward traffic between networks based on the MAC addresses
  * **Switches**<a id="switch"></a>
    - sometimes called *Layer 2 switches*
    - effectively create point-to-point connections between two conversing computers
    - gives every converstaion between two computers full bandwidth of the network
    - when first turned on, it acts like a hub
    - as it forwards all frames, copies the source MAC addresses and creates a tables of MAC
     addresses of connected computers called *MAC address table*
    - difference with hub is in the repeating of frames during normal use
    - filter by MAC address once completed port mapping
    - each port is in its own collision domain
    - a switch can **buffer incoming frames**
       + two nodes connected to the switch can send data at the same time and the switch will
         hanlde it without any collision
    - unicast messages always go only to the intended recipient
    - sends all broadcast messages to all ports (except origin)
    - **broadcast domain**
       + to contrast switch to the hub-based networks with their collision domains
    - a switch becomes a single point of failure if every node connects to the it
    - can connect switches via *uplink port* or *crossover cable*
       + **Uplink Ports**
         + enable to connect two switches using a *straight-through* cable
         + modern switches do not have dedicated uplink port, instead auto-sense when another
           switch is plugged in
         + *auto-medium-dependent interface crossover (MDI-X)*
       + **Crossover Cables**
         + reverse the sending and receiving pairs on one end of the cable
         + one end with [T568A](#568a) and other end with [T568B](#568b)
         + modern switches with auto-sensing ports don't require crossover cable
         + can use to connect two computers with no switch between
         + answer for interconnecting ancient switches (10BASE-T/10BASE-FL)
    - **switching loops or bridge loops**
       + redundant connections in a network
       + would crash the network in early days
    - **Spanning Tree Protocol (STP)**
       + to eliminate the problem of accidental switching loops
       + enabled by default
    - **Bridge Protocol Data Units (BPDUs)**
       + used to communicate with other switches to prevent loops
       + **Loop-free topology**
         + established by configuring BPDUs
         + one root switch act as center of STP and used by other switches as reference point
         + have redundant links but certain ports will be placed in *blocking* state
         + ports in the blocking state stil hear the configuration BPDUs
    - **Topology Change Notification (TNC) BPDU**
       + another type of BPDU used by STP when a link or device goes down
       + enables the switches to rework themselves around the failed interface or device
    - STP settings can be manually changed
    - switch port directly connected to a PC should never participate in STP, but instead
     use a setting called **PortFast**
       + a setting that enables the interface to come up right aways without normal latency
       + also prevent TCN BPDUs being sent out of the switch every time a PC is on and off
         (causes all switches to flush their source address table and relearn MAC addresses)
       + *BPDU guard* will move a port configured with PortFast into an errdisable state
         (erro occurred, disabled) if a BPDU is received on that port
       + after errdisable state, requires an admin to manually bring the port back up
       + PortFast ports should never receive a BPDU, which could start a switching loop
       + *root guard* will move a port into root-inconsistent state
    - **Rapid Spanning Tree Protocol (RSTP)**
       + replace SPT in 2001
       + offers significantly faster convergence time following some kind of network change

Troubleshooting Switches
------------------------
  * two problem categories: Obvious physical damage, Dead ports
  * similar diagnostic pattern for both
    1. recognizing a switch have problem because plugged in device can't connect to network
    2. examine switch for obvious damage
    3. look for link lights, try different port if no flashing
    4. check cables (bent, broken or stepped on), replace if needed
    5. use tried-and-true method of replacing the switch or the cable with good ones
`back to top <#network+top>`_

### 4. Ethernet Standards


100BASE-T
---------
  * standard to replace 10BASE-T
  * 100BASE-T4: use Cat 3, disappear later
  * 100BASE-TX: use Cat 5 and Cat 5e, later became dominant
  * **baseband** signal type, 100m between switch and node, max 1024 nodes per hub/switch
    - baseband: only one single signal travels over the wires at one time
    - broadband: can get multiple signals to flow over the same wire at the same time
  * use star-bus topology, Cat 5 or better UTP or STP cabling with RJ-45/8P8C connectors
  * **upgrading to 100BASE-T from 10BASE-T**
    - need Cat 5 or better, had to replace all 10BASE-T NICs with 100BASE-T NICs
    - had to replace 10BASE-T hub or switch with 100BASE-T one
  * **multispeed, auto-sensing NICs and hubs/switches** made the upgrade easier
    - auto negotiate with the hub/switch to determine the other device's highest speed
    - all modern NICs are multispeed and auto-sensing
  * some NICs had extra link lights to show the speed 100BASE-T or 10BASE-T
* UTP cabling cannot meet the needs of every organization
  * 100m distance limitation
  * lack of electrical shielding
  * easy to tap, inappropriate choice for high-security environments

100BASE-FX
----------
  * uses **multimode fiber-optic cabling**, generally OM1
    - comes in varying core sizes and capabilities
    - optical multimode (OM1, OM2, OM3, OM4)
  * subscriber connector (SC) or straight tip (ST) connectors
  * maximum cable length of 2km, max 1024 nodes per hub/switch, star-bus topology

100BASE-SX
----------
  * short-distance, LED-based alternative to 100BASE-FX
  * ran on OM1 or OM2 fiber at 850nm and use ST, SC or LC connectors
  * backward compatible to 10BASE-FL
  * deprecated, not used anymore
* by late 1990s, most 100BASE-T cards could auto-negotiate for **full-duplex**
  * doesn't increase network speed directly, but doubles network bandwidth
  * all NICs today run full-duplex

Gigabit Ethernet
----------------
  * most common type found on new NICs
  * **1000BASE-T**
    - 802.3ab standard under IEEE, most widely implemented
    - uses four-pair UTP or STP, Cat 5e or 6, four-pair/full-duplex with RJ-45
    - max cable length of 100m
    - connections and ports look exactly like 10BASE-T or 100BASE-T
  * **1000BASE-X**
    - 802.3z standard
    - **1000BASE-SX**
       + uses multimode fiber-optic cabling, OM2 or better
       + max cable length of 220 to 500m
       + 850nm wavelength LED
       + look similar to 100BASE-FX devices
       + commonly use LC while 100BASE-FX use SC
    - **1000BASE-LX**
       + uses 1300nm lasers on single-mode cables, long distance carriers, OS1 or OS2
       + distance up to 5km
       + use special repeaters to increase distance to 70km
       + some large carriers are beginning to adopt 1000BASE-LX as the Ethernet backbone
       + connectors look like 1000BASE-SX
       + commonly use LC and SC
  * ST connectors are relatively large, twist-on, requiring the installer to twist the cable
    when inserting or removing it
  * SC connectors snap in and out but large, more popular than STs
  * **SFF (small from factor) connectors**
    - MT-RJ (Mechanical Transfer Registered Jack), popular with Cisco
    - LC (local connector), particularly popular in US
  * fiber industry has no standard beyond ST and SC connectors
  * different makers of fiber equipment may have different connections

connection variations
---------------------
  * **PC (physical contact)**
    - standard connector type today
    - two pieces of fiber touch when inserted
    - highly polished and slightly spherical, reducing the signal loss at the connection point
  * **UPC (ultra-physical contact)**
    - polished extensively for superior finish
    - reduce signal loss significantly over PC connectors
  * **APC (added physical contact)**
    - 8-degree angle to the curved end, lowering signal loss
  * UPC and APC connection does not degrade from multiple insertions

dedicated media converters
--------------------------
  * to connect any type of Ethernet cabling
  * SMF to UTP/STP, MMF to UTP/STP, Fiber to coaxial, SMF to MMF
  * GBIC (gigabit interface converter): standard for modular ports
  * SFP (small form-factor pluggable): smaller modular transceiver used by switches
  * transceiver or optic: removable module that enables connectivity between a device and cable
  * media converter: enables connection between two different types of network

10Gigabit Ethernet (10GbE)
--------------------------
  * has a number of fiber standards and two copper standards
  * copper 10GbE often pair excellent performance with cost savings
  * most data centers use mix of fiber and copper
  * to maintain the integrity of Ethernet frame and to figure out how to transfer those frame
    at such high speed
  * **SONET (Synchronous Optical Network)**
    - perfectly usable ~10Gbps fiber network used for WAN
  * *factors* that define 10GbE standards
    - type of fiber used, wavelength of lasers
    - physical layer signaling type, maximum signal distance
  * standards do not define the type of connector and let the manufacturers decide
  * **10GBASE-xy (fiber based)**
    - x: type of fiber and wavelength of laser signal
    - y: physical layer signaling standard (always R for LAN or W for SONET/WAN-based signaling)
    - 10GBASE-Sy
       + uses short-wavelength (850nm) signal over multimode fiber, max fiber length
         depend on th etype of multimode fiber used (OM1 for 33m, OM4 for 400m)
       + 10GBASE-SR: multimode 850nm, LAN-based signaling, max signal length 33-400m
       + 10GBASE-SW: multimode 850nm, SONET/WAN-based signaling, max signal length 33-400m
    - 10GBASE-Ly
       + uses long-wavelength (1310nm) signal over single-mode fiber
       + 10GABSE-LR: single-mode 1310nm, LAN, 10km, most popular and least expensive
       + 10GBASE-LW: single-mode 1310nm, SONET/WAN, 10km
    - 10GABSE-Ey
       + uses extra-long wavelength (1550nm) signal over single-mode
       + 10GBASE-ER: single-mode 1550nm, LAN, 40km
       + 10GBASE-EW: single-mode 1550nm, SONET/WAN, 40km
    - 10GBASE-L4
       + uses four lasers at 1300nm wavelength over legacy fiber
       + can support up to 300m on multimode cable
    - 10GABSE-LRM
       + uses long-wavelength signal of 10GBASE-LR but over legacy multimode fiber
    - 10GBASE-ZR
       + not part of the IEEE standards
       + uses 1550nm wavelength over single-mode fiber
       + up to 80km, work with both Ethernet LAN and SONET/WAN infrastructure
  * **10GBASE-T (copper based)**
    - looks and work exactly like slower versions of UTP Ethernet
    - on Cat 6 has a max length of only 55m
    - Cat 6a standard enables it to run at standard distance of 100m
  * multisource aggreements (MSAs): agreements among multiple manufacturers to make interoperable
    devices and standards
  * SFP+ (enhanced small form-factor pluggable): transceiver used in 10GbE
  * WDM (wave division multiplexing): to differentiate wave signals on single fiber
  * BiDi (bidirectional transceiver)
    - have only single optical port
    - corresponding BiDi must be installed on the other end
    - cost less to deploy, can use existing fiber runs to double the capacity of network
    - most use SFP+ connectors, 40GbE BiDi use quad small form-factor pluggable (QSFP)
* multispeed Ethernet network works best as reasonable speed and cost
* high-speed switches maintain a backbone multispeed network
* need switches with separate, dedicated, high-speed ports on every floor to connect to the
  backbone network

40GbE and 100GbE
----------------
  * use the same frame as the slow-by-comparison earlier versions of Ethernet
  * primarily used when lots of bandwidth is needed (connecting servers, switches, routers)
  * **40GbE**
    - common 40GbE runs on OM3 or better multimode fiber, uses laser light and various
     four-channel connectors (QSFP+, enhanced quad small form-factor pluggable)
    - other 40GbE run on single-mode and distances up to 10km, some run on Cat 8 UTP for 30m
  * **100GbE**
    - employ both MMF and SMF with various connectors
    - typically QSFP28 (aka QSFP100 or 100G QSFP) that has four channels of 25Gb each
`back to top <#network+top>`_

### 5. Installing physical network

* structured cabling system
* a run: single piece of installed horizontal cabling

horizontal cabling
------------------
  * all cables run horizontally from the telecommunications room to the work area (often an
   office or cubicle)
  * cable is Cat 5e or better UTP for copper-based standards such as 1000BASE-T
  * fiber-based standards such as 1000BASE-SX use multimode fiber-optic cabling
  * should always be solid core
  * most cable installers recommend using highest Cat rating, at least Cat 6
  * four-pair UTP is common (Cat 6, 6a, 7 all use four-pair UTP)

solid core UTP
--------------
  * each wire uses single solid wire
  * better conductor, but stiff and will break if handled too often

stranded core UTP
-----------------
  * each wire is a bundle of tiny wire strands
  * not as good conductor but can be handled without breaking
  * require specific crimps

telecommunications room
-----------------------
  * also called intermediate distribution frame (IDF)
  * **equipment racks**
    - all racks are 19inches wide but vary in height, from 2 to 3 feet
    - can mount any network hardware component into a rack
    - equipments are measured in 'U' (1U = 1.75inches), most rack mounted devices are 1U,
      2U or 4U, typical full-size rack is 42U
    - two-post rack, used for only patch panels, switches and routers
    - four-post or server rail rack, to install big servers
    - PDU (power distribution unit) for centralized power management
    - proper air flow is important when planning rack system
    - locking racks and locking cabinets: chassis plus a door with locking mechanism
  * **patch panel**
    - a box with a row of female ports in the front and permanent connections in the back to
      connect horizontal cables
    - 110 block or 110-punchdown block: most common type of patch panel used today, introduce
      less crosstalk than 66 blocks
    - make sure to insert the wires in same standard
    - still common to find a group of 66-block patch panels separate from network's 110-block
      patch panels
    - all panels have space in the front for labels
    - available in wide variety of configs that include different types of ports
    - UTP patch panels, like UTP cables, come with Cat ratings
    - Cat 6 panel offers lower crosstalk and network interference
  * **patch cables**
    - short straight-through UTP cables
    - use stranded core, most come with reinforced connector for multiple handling
  * **BIX block**
    - proprietary networking interconnect
    - installed on a wall rather than in a rack
  * **Krone block**
    - Krone LSA-PLUS connector, alternative to 110-punchdown block
    - enable networking as well as audio interconnections
  * patch bay: dedicated block with A/V connections

work area
---------
  * a wall outlet that serves as termination point for horizontal cables
  * female RJ-45 jacks in the wall outlets have Cat ratings
  * use same connectors in the wall outlets as the patch panels
  * label the outlet to show the job of each connector
  * most use patch cable to connect the PC to the outlet
  * simplest part of system but source of most network failures
* TIA/EIA 568 specify only UTP cable lengths of 90m, as other 10m is patch cable distance
  between switch and PC, total of 100m
* telephone installation: mostly use one or more strands of 25-pair UTP cables running to a
  66 block in the telecommunications room on each floor

Demarc
------
  * demarcation point, physical location of the connection and marks the dividing line of
    responsibility for the functioning of the network
  * located in the service-related entry point for a network
  * equipment supplied by ISP is a network interface unit (NIU) that serves as demarc between
    home network and ISP
  * early systems used an NIU called smartjack to set up remote loopback
  * NIU, NIB or NID: terms to describe devices that often mark the demarc in home or office
  * **CPE (customer-premises equipment)**
    - acts as the primary distribution tool for the building after the demarc, to which network
     and telephone cables connect to
  * demarc extensions: any cabling that runs from the NIU to CPE
  * vertical cross-connect: main patch panel toto which switches with cabling connect to
  * MDF (main distribution frame): room that stores all demarc and LAN cross-connects
  * smaller building may combine demarc, MDF and IDF into single room

installing structured cabling
-----------------------------
  * get a floor plan, which is a key component of organization documents, part of the physical
    network diagram of an organization
  * map the cable runs, determine where to place cable drop, the location where the cable comes
    out of the wall in the work area
  * most installers price their network jobs by quoting a per-drop cost
  * "drop" is also used to define new run coming through a wall outlet
  * decide to run the cables in or outside the walls
  * **determine the location of telecommunications room**
    - distance, power (put the room on its own dedicated circuit if possible)
    - humidity, cooling, access, expandability
  * installers document and label each cable run when connecting both ends of each cable to the
    proper jacks
  * all 100-punchdown connections have a color code

issues when connecting the patch panels
---------------------------------------
  * patch cable management
  * overall organization of patch panel (can organize according to the physical layout or the
    logical layout of the network)
  * need to document everything clearly and carefully
* verify that the cable run can handle the speed of the network
* typical network admin/tech cannot properly test new cable run

issues
------
  * signal degradation, lack of connection, interference
  * **copper-based network**
    - long cables can degrade that it's no longer detectable
    - too much noise can degrade
    - broken or not connected in the crimp (open) wire will cause no continuity (complete
     functioning network)
    - damaged or improperly crimped cables can cause shorts
  * **fiber-based network**
    - various imperfections in the glass fiber cause signal loss over distance
    - damaged cables or open connections stop signals
    - SFP or GBIC can have problems, need to check both connector and cable
    - dirty connector/optical cables can cause serious signal loss
    - small connector mismatch in either cladding or the core can cause loss
    - modal dispersion: wave signals traveling too far without help and causing attenuation
     and dispersion
    - every piece of fiber has certain bend radius limitation
    - light leakage: part of signal goes out the cable
    - wavelength mismatch will stop the transmission
    - fiber-optic runs don't experience crosstalk or interference
    - OTDR (optical time-domain reflectometer): determine continuity and tell exactly how far
     if there is a break
    - fusion splicer: enables the tech to combine two fiber-optic cables without losing quality
* low end cable tester can only test for continuity
* better testers can run a wire map test, which will pick up shorts, crossed wires and more
* can use a multimeter to test continuity

TDR (time-domain reflectometer)
-------------------------------
  * medium-priced tester (~$400)
  * can test continuity and wire map and additional capability to determine cable length
* higher-end testers can detect things such as crosstalk and attenuation

crosstalk
---------
  * when sending a signal down one of UTP four pairs, the other pairs pick up some of the signal
  * every piece of UTP generates crosstalk
  * poor-quality crimp creates so much crosstalk that a cable run won't operate at its speed
  * measured in decibels (dB)
  * **NEXT (near-end crosstalk)**
    - electronic detector connected on the same end of the cable as the end emanating the signal
  * **FEXT (far-end crosstalk)**
    - listen the signal on the other pairs on the far end of the connection

attenuation
-----------
  * signal getting weaker as it progresses down the wire
  * attenuation increases as the cable run gets longer and the signal becomes more susceptible
    to crosstalk

dispersion
----------
  * signal spreading out over long distances
* latency: delay between the time the sending machine sends a message and the time receiving
  machine can start processing the frames
* jitter: delay in completing a transmission of all the frames in a message, perfectly normal
  and modern network technologies handle jitter fine
* cable certifiers can both do the high-end testing and generate a report

NICs
----
  * all UTP Ethernet NICs use RJ-45 connector
  * using the smae model of NIC makes driver updates easier
  * PCIe NIcs usually come in either one-lane (x1) or two-lane (x2)
  * **port aggregation, bonding or link aggregation**
    - most switches enable to use multiple NICs for a single machine
    - doesn't increase the speed but adds another lane of equal speed
    - use identical NICs and switches from the same companies to avoid incompatibility
    - LACP (link aggregation control protocol): controls how multiple network devices send
     and receive data as a single connection
* NICs and switches can have between one and four different link lights and LEDs can be any color
* multispeed devices usually have a link light to show the speed of connection
* activity light turns on when the card detects network traffic

diagnosing physical problems
----------------------------
  * try to eliminate software errors
  * multiple systems failing to access network points to hardware problems, check link lights
  * NIC's female connector is a common failure point
  * loopback test sends data out of the NIC and checks to see if it comes back
  * true external loopback requirs a loopback adapter or loopback plug
  * always include the patch cables and couplers when testing cable run
* coupler: small devices with two female ports that enable to connect two pieces of cable to
  overcome distance limitaions
* online UPS: continuously charges a battery, which powers the computer components
* SPS (standby power supply): has a battery but doesn't power the computer unless power goes out
* voltage event recorder plugs into power outlet and tracks the voltage over time
* all serious telecommunications rooms should have UPS with temperature monitor
* can install environmental monitors that keep a constant watch on humidity, temperature

toner
-----
  * two separate devices, tone generator and tone probe
  * tone generator: connects to the cable using clips and sends electrical signal along the wire
  * tone probe: emits a sound when it is placed near a cable connected to the tone generator
`back to top <#network+top>`_

### 6. TCP/IP Basics

* TCP/IP protocol suite operates at Layers 3-7
* IP (Internet Protocol) works at the Network layer to take data from the Transport layer, add
 addressing and creates IP packet
* packet is handed to the Data Link layer for encapsulation into a frame

TCP/IP protocols
----------------
  * IPv4, IPv6
  * **ICMP, ICMPv6 (Internet Control Message Protocol)**
    - IP error reporting and diagnostics
    - software auto uses ICMP as needed without direct user action

IPv4 header
-----------
  * simplified: Ver, IHL, DSCP, ECN, Total Length, TTL, Protocol, etc.,
  * full IPv4 packet header has *14* different fields
  * Ver
    - Version, defines IP address type
  * Total Length
    - total size of IP packet in octets, includes IP header and payload, 16bits
  * TTL (Time to Live)
    - prevent packet from indefinitely spinning through the Internet by using a counter
    - cannot start higher than 255, many OS start at 128
  * Protocol
    - either TCP or UDP
    - identifies what's encapsulated inside the packet
* Transport Layer Protocols
  * connection-oriented
    - when data moving between two systems must get there in good order
    - Transmission Control Protocol (TCP)
  * connectionless
    - when missing a bit or two of data is not a problem
    - User Datagram Protocol (UDP)
  * people who develop an application decide which protocol to use
  * sometimes even design one or more protocols for the application

TCP (Transmission Control Protocol)
-----------------------------------
  * used by most TCP/IP apps
  * gets an application's data from one machine to another reliably and completely
  * require both the sending and receiving machines to acknowledge the other's  presence and
   readiness to send and receive data
  * chops the data into segments
  * receiving system must requesst the missing segments
  * *TCP three-way handshake*: SYN, SYN-ACK, ACK
  * TCP header
    - Source port, Destination port (range from 1 to 65,535)
    - Sequence number, Acknowledgment number (to keep track of various pieces of data)
    - Flags (give detailed info about the state of connection)
    - Checksum (recipient can use to check the TCP header for errors such as bits flipped or
     lost)

UDP (User Datagram Protocol)
----------------------------
  * UDP header: Source port, Destination port, Length, Checksum
  * works best when sending a lot of data that doesn't need to be perfect
  * fast compared to TCP
  * DNS (Domain Name System) and DHCP (Dynamic Host Configuration Protocol) uses UDP
  * UDP datagrams don't get chopped up at the Transport layer, but just get a header
* at the LAN level, every host runs TCP/IP software over Ethernet hardware
* IP packet is completely encapsulated inside the Ethernet frame

ARP (Address Resolution Protocol)
---------------------------------
  * process and protocol used in resolving an IP address to an Ethernet MAC address
  * to get Computer B's MAC address, Computer A sends ARP request to universal MAC address,
    FF-FF-FF-FF-FF-FF, for broadcast
  * the switch forwards the broadcast to every connected node

IP addresses
------------
  * not a fixed part of the NIC
  * group together sets of computers into logical networks
  * computers can communicate with each other across all of the LANs that make up a WAN
  * consists of a *32-bit* value, which is broken down into four groups of eight, separated by
    a period
  * each of 8-bit values is converted into decimal number between 0 and 255
  * no two computers on the same network may have the same IP address
  * **IP numbering system**
    1. create network IDs
    2. interconnect the LANs using routers and provide routers a way to use the network ID to
     send packets
    3. use a subnet mask to give each computer on the network to recognize a packet is for the
     LAN or WAN
  * **network IDs**
    - needed by each LAN for a WAN to work
    - to differentiate LANs from one another, each computer on a single LAN must share a very
     similar, but not identical IP address
    - 202.120.10.x: network ID is 202.120.10.0, host ID is x
    - no individual computer can have an IP address that ends with 0 as it's reserved for
     network IDs
    - very flexible as long as no two interconnected networks share the same ID
    - enable to connect multiple LANs into a WAN
  * **interconnecting LANs**
    - every TCP/IP LAN that wants to connect to another TCP/IP LAN must have a router connection
    - **default gateway**
       + both the router's interface on a LAN and the router itself that route traffic out to
         other networks
       + in the same network ID as the host
       + most network admins give the LAN-side NIC on the default gateway the lowest or highest
         host address in the network
       + if network ID is 22.33.4.x, router is configured to 22.33.4.1 or 22.33.4.254
    - **routing table**
       + actual instructions that tell the router what to do with incoming packets and where
         to send them
  * **subnet mask**
    - tells the sending computer whether the destination IP address is local or long distance
    - always totaling exactly 32 bits
    - portion of the IP address that aligns with the ones of the subnet mask is the network ID
    - portion that aligns with the zeroes is the host ID
    - IP address     192.168.5.23    11000000.10101000.00000101.00010111
    - Subnet mask    255.255.255.0   11111111.11111111.11111111.00000000
    - Network ID     192.168.5.0     11000000.10101000.00000101.x
    - Host ID        x.x.x.23        x.x.x.00010111
    - before a computer sends out any data, it first compares its network ID to the
     destination's network ID
    - most people represent subnet masks using shorthand called **CIDR notation**
       + 11111111111111111111111100000000 = /24 (24 ones)
    - all computers on the same network have the same subnet mask and network ID
    - IANA (Internet Assigned Numbers Authority): track and disperse IP addresses
    - RIRs (Regional Internet Registries): parcel out IP addresses to ISPs and corporations
    - ICANNN (Internet Corporation for Assigned Names and Numbers): indirectly manages IANA
    - PTI (Public Technical Identifiers): perform IANA's techinal work
    - ways to send a packet
       + broadcast: every computer on the LAN hears the message
       + unicast: one computer sends a message directly to another
       + anycast: multiple computers share a single address and routers direct messages to the
         closest computer
       + multicast: single computer sends a message to a group of interested computers
    - **Class IDs**
       +           First Decimal Value     Addresses                     Host per Network ID
       + Class A   1-126                   1.0.0.0-126.255.255.255       16,777,216
       + Class B   128-191                 128.0.0.0-191.255.255.255     65,534
       + Class C   192-223                 192.0.0.0-223.255.255.255     254
       + Class D   224-239                 224.0.0.0-239.255.255.255     Multicast
       + Class E   240-255                 240.0.0.0-255.255.255.255     Experimental
       + multicast blocks are used for one-to-many communication such as streaming video
         conferencing
       + Class A: first binary octet always begins with a 0 (0xxxxxxx)
       + Class B: first binary octet always begins with a 10 (10xxxxxx)
       + Class C: first binary octet always begins with a 110 (110xxxxx)
       + Class D: first binary octet always begins with a 1110 (1110xxxx)
       + Class E: first binary octet always begins with a 1111 (1111xxxx)
       + ip class blocks waste addresses as it had to take a single network block
    - **CIDR & subnetting**
       + foundation of CIDR is a concept called subnetting, taking single calss of IP addresses
         and chopping it up into multiple smaller groups called subnets
       + CIDR is also referred to as classless address
       + CIDR makes it possible to extend the subnetting approach to the Internet as a whole
       + subnetting enables to separate a network for security or for bandwidth control
       + take an existing /8, /16 or /24 subnet and extend the subnet mask by replacing zeroes
         with ones
       + start with the given subnet mask and move it to the right until having the number of
         subnets needed, forget the dots, cannot subnet without first converting to binary
       + classful subnets were always /8, /16 or /24
       + number of hosts = 2^x - 2, x = number of zeroes in the subnet mask
       + all subnetting begins with a single network ID
       + primary tool for subnetting is the existing subnet mask
       + **subnetting 192.168.4.0/24**
         1. convert to binary      11111111111111111111111100000000
         2. move one bit to right  11111111111111111111111110000000
         3. insert periods         11111111.11111111.11111111.10000000
         4. convert to decimal     255.255.255.128
       + number of subnets that can be created = 2^y, y = number of bits added to subnet mask
       + goals of subnetting: efficiency and making multiple network IDs from a single ID

Static IP addressing
--------------------
  * give the default gateway the first or last IP address in the network ID
  * try to use the remaining IP addresses in sequential order
  * try to separate servers from clients
  * document which computers use which ranges
  * Windows
    - use TCP/IPv4 Properties dialog or Edit IP Settings dialog
  * macOS
    - use the Network utility in System Preferences
  * Linux
    - ip addr add 192.168.4.10 dev eth1
    - ``ip`` command will not be permanent
  * always verify with ping after setting static ip
  * making changes to the network is difficult when using static

Dynamic IP addressing
---------------------
  * via DHCP (Dynamic Host Configuration Protocol)
  * any network using DHCP consists of a DHCP server and DHCP clients
  * most networks have a single DHCP server
  * often built into a router for SOHO networks
  * DHCP server may be running on rack-mounted server in enterprise
  * **DHCP four-way handshake or DORA**
    - Discover, Offer, Request, Acknowledgment
    - when a DHCP client boots up, it auto broadcasts a **DHCP Discovery message**
    - DHCP server hears the request and sends the DHCP client a **DHCP Offer message**
    - DHCP client sends out a **DHCP Request**, to verify the offer is still valid, DHCP request
     is important as it tells the network that the client is accepting IP info from this and
     only this DHCP server
    - DHCP server sends a **DCHP ACK (Acknowledgment)** and lists the client's MAC address as
     well as IP info given to the DHCP client in a database
  * after DORA, DHCP client gets a DHCP lease from the pool of available leases
  * DHCP lease is set for a fixed amount of time, often one to eight days
  * near the end of lease time, the DHCP client sends another DHCP Request message
  * DHCP servers uses UDP port 67 and DHCP clients use port 68
  * every host is preconfigured as a DHCP client by default
  * **DHCP server requirements**
    - a pool of legitimate IP addresses to be passed out to clients
    - the subnet mask for the network
    - the IP address for the default gateway for the network
  * **DHCP scope**
    - range or pool of IP addresses
    - scope options: default gateway, DNS server, Network Time server, etc.,
  * home router assumes that it is the default gateway
  * there should only be one DHCP server on a small LAN
  * **DHCP relay**
    - DORA relies on broadcasting to work but all routers block broadcast traffic
    - when a client renews its DHCP lease, everything's unicast as the client already has a
     valid IP address and knows the DHCP server's IP
    - DHCP relay enables a router to accept DHCP broadcasts from clients and use UDP forwarding
     to send them on via unicast addresses directly to the DHCP server
    - the relay device must have the IP address of the real DHCP server, also known as the IP
     helper address or UDP helper address
  * devices such as routers, switches, file servers, printers and cameras should never use
    dynamic IP addresses
  * set aside IP addresses for devices and subdivide the set-aside addresses into chinks for
    each kind of device
  * can reserve the IP addresses to keep DHCP from assigning the devices and configure each
    host with static address
  * can knock out some of the addresses inside the pool by creating an IP exclusion range
  * can use DHCP server to set up MAC reservations, enabling DHCP to lease the same address to
    the same host each time
  * DHCP is completely invisible to users when it works, but user may be clueless when it breaks
  * systems that use static IP addressing can never have DHCP problems
  * IPv6 always generates IP addresses automatically
  * No DHCP Server
    - operating system will post some form of error
    - DHCP client will have a strange address generated by zeroconf or APIPA (Automatic Private
     IP Addressing) in Windows
    - DHCP clients are designed to auto generate an APIPA address if they do not receive a
     response to a DHCP Discover message
    - client only generates the last two octets of an APIPA address
    - APIPA cannot issue a default gateway
  * computer gets confused and won't grab an IP address
    - force the computer to release its lease
    - ipconfig /release, ipconfig /renew
    - ifconfig eth0 down, ifconfig eth0 up
  * **multiple DHCP servers**
    - bigger networks, enterprises, run more than on DHCP server and it doesn't matter which
     server answers
    - network ID: 172,13.14.0
    - DHCP Server 1:Scope 172.13.14.200-172.13.14.255
    - DHCP Server 2:Scope 172.13.14.226-172.13.14.250
    - each DHCP server would still use the same subnet mask and default gateway
    - running two independent DHCP servers doubles the administrative load
    - **DHCP failover**
       + only two DHCP servers work together to provide DHCP for the network
       + in Windows Server 2012, DHCP failover pair consists of primary and secodary servers,
         the pair shares a single scope, end users never notice a thing
       + failover is quite common in large networks
  * rogue DHCP server
    - DHCP client will accept IP info from the first DHCP server that responds
    - it's easy to add another DHCP server to network to pass out incorrect IP info to clients
    - unintentional rogue server is easy to detect
    - check for rogue DHCP server if some users can access resources and some cannot
    - malicious rogue DHCP servers can be hard to detect if they give out IP addresses in the
     same scope as the legitimate server

Special IP addresses
--------------------
  * loopback address (127.0.0.1)
    - send data to 127.0.0.1 to tell that device to send the packets to itself
    - entire 127.0.0.0/8 subnet is reserved for loopback addresses
  * private IP addresses
    - IETF set out specific ranges for networks that aren't connected to the Internet or for
     networks that wants to hide
    - all routers block private IP addresses
    - can never be used on the Internet
    - anyone can use private IP addresses but useless for those that need access to the Internet
    - 10.0.0.0-10.255.255.255 (1 Class A network block)
    - 172.16.0.0-172.31.255.255 (16 Class B network blocks)
    - 192.168.0.0-192.168.255.255 (256 Class C network blocks)
`back to top <#network+top>`_
