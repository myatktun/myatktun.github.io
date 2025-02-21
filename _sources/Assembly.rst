========
Assembly
========

1. `RISC-V`_
2. `References & External Resources`_

`back to top <#assembly>`_

RISC-V
======

* `RISC-V Basics`_, `RISC-V Modes`_, `RISC-V Registers`_, `RISC-V Instruction Set`_, `RISC-V Directives`_

RISC-V Basics
-------------
    * open-sourced ISA based on RISC principles
    * can be used academically, and deployed in any hardware or software without royalties
    * RV32 for 32-bit and RV64 for 64-bit implementations
    * byte: 8 bits, halfword: 16 bits, word: 32 bits, double word: 64 bits
    * unsigned numbers are usually used for addresses, pointers and bit vectors
    * unsigned double words are never necessary for memory addresses, loop counters, etc.
    * unused upper bits of singed values will be filled with sign bit, and zeros for unsigned
      values
    * **Address Alignment**
        - every byte in the memory has a unique address
        - halfword-aligned address: divisible by 2, and ends with bit 0, or 0, 2, 4, 6, 8, a,
          c, e in hex
        - word-aligned address: divisible by 4, and ends with bits 00, or 0, 4, 8, c in hex
        - double word-aligned address: divisible by 8, and ends with bits 000, or 0, 8 in hex
        - all instructions must be halfword aligned
        - Load and Store with unaligned addresses are implementation dependent, works fine,
          exception occurs, or undefined behaviour
    * **Code Relocation**
        - benefit of PC relative code, where it can be anywhere in memory and still works
        - the distance between jump/call and the target must be constant
        - useful when dynamically loading the code

RISC-V Modes
------------
    * privileged instructions can only be executed in Machine and Supervisor modes
    * **Machine Mode**
        - highest, and initial mode after power-on or reset
        - code such as timer interrupts require the handler to run in machine mode
    * **Supervisor Mode**
        - mode run by all kernel code
        - when switching from kernel code to user code, kernel registers are saved, and will
          be restored once kernel code is entered again
    * **User Mode**
        - mode run by all user code
        - running privileged instructions in this mode will cause trap and abort

RISC-V Registers
----------------
    * 32 general purpose registers ``x0-x31``, and 1 program counter register ``pc``
    * 4,096 control and status registers (CSRs) cannot be accessed by user mode
    * 6 special purpose, 7 temporary (volatile, data does not persist through function calls),
      12 saved (non-volatile), and 8 argument
    * arguments more than 8 or larger than register size must be passed onto the stack
    * registers have separate identifiers based on calling convention
    * **zero: x0**
        - hard-wired zero
        - can also be used as a destination to discard the result
    * **ra: x1**
        - return address, saver: caller
        - ``CALL``: does not push return address onto stack, but saves it in ``x1``
        - ``RET``: jumps to the address in ``ra`` or ``x1``
        - ``CALL`` and ``RET`` are fast as they do not need to access memory
        - but nested calls require additional instructions to save and restore from memory
    * **sp: x2**
        - stack pointer, saver: callee
        - not a special stack pointer register, but used as convention only
        - some compressed instructions assume ``x2`` is the stack pointer
    * **gp: x3**
        - global pointer
        - points to global/static variables area, and makes addressing easier
    * **tp: x4**
        - thread pointer
        - points to area of variables such as thread ID, threat parameters, and global/static
          variables local to the thread
    * **t0-2: x5-7**
        - temporaries, saver: caller
    * **s0/fp: x8**
        - saved register/frame pointer, saver: callee
    * **s1: x9**
        - saved register, saver: callee
    * **a0-1: x10-11**
        - function arguments/return values, saver: caller
        - by convention, function returns the result in ``a0``
    * **a2-7: x12-17**
        - function arguments, saver: caller
    * **s2-11: x18-27**
        - saved registers, saver: callee
    * **t3-6: x28-31**
        - temporaries, saver: caller
    * **pc**
        - holds the address of next instruction to execute
        - 32 or 64 bits accordingly, but may be smaller and implementation depends on maximum
          memory size
    * **mhartid**
        - hard-wired core/hart id
        - during startup, id is copied into ``tp`` and the register will never change in the
          kernel
    * ``mstatus`` & ``sstatus``: status register
    * ``mtvec`` & ``stvec``: trap vector or address of the invoked handler when a trap occurs
    * ``mepc`` & ``sepc``: exception pc, value of previous pc when a trap occurs
    * ``scause``: trap cause code
    * ``stval``: bad address or instruction
    * ``mscratch`` & ``sscratch``: work register to be used by trap handlers
    * **satp**
        - address translation pointer, points to the page table and initially 0
        - after initialisation, VAT is always on in supervisor and user mode
        - whenever ``satp`` is updated, all TLBs (translation lookaside buffers) must be flushed
          with ``sfence.vma`` or ``sfence_vma()``
    * ``mie`` & ``sie``: interrupt enable, to enable interrupt selectively
    * ``sip``: interrupt pending
    * ``medeleg``: exception delegation, from machine to supervisor mode
    * ``mideleg``: interrupt delegation, from machine to supervisor mode
    * **pmpcfgX & pmpaddrX**
        - physical memory protection, configuration word and address
        - limit physical memory access for code in supervisor or user mode
        - mainly for secure bootstrapping and hypervisor

RISC-V Instruction Set
----------------------
    * **Notation**
        - three register instruction structure: ``<opcode>    <dst>,<src1>,<src2>     # cmt``
        - only ``sw`` has destination listed last, ``sw   <rs1>,<rd>     /* multi-line cmt */``
        - one instruction per line
        - can have optional label: ``<label>: <opcode> <rd>,<rs1>,<rs2>``
        - value of the label is the address of next thing in the file, e.g. in ``hello: add``,
          value of ``hello`` will be the address of ``add`` instruction
        - example opcode: ``add``, ``call``, ``c.add``, ``.word`` which is an assembler directive
        - operands can be register to register, or register to 12-bit immediate value, which
          can be in decimal, hex, symbolic or expression
    * **Full-Sized Instruction Set**
        - mandatory on any core
        - every instruction is a word in size, 32 bits or 4 bytes, regardless of RV32 or RV64
    * **Compressed Instruction Set**
        - optional and will not be included on some processors
        - to reduce code size and increase execution speed, e.g. ``c.add``
        - every instruction is halfword in size, 16 bits or 2 bytes
        - every compressed instruction is same as a single full-sized instruction, but not
          vice versa
    * **Option Letter Codes**
        - RV32I and RV64I basic instructions work on entire register, e.g. ``add`` performs
          32-bit addition on RV32, and 64-bit addition on RV64
        - ``C``: compressed instruction set
        - ``M``: multiply and divide instructions
    * **Arithmetic & Logic**
        - ``add, sub, and, or, xor``
        - ``addi, andi, ori, xori, slli, srli, srai, slti, sltui``
        - ``sll``: shift left logical, rs1 << rs2
        - ``srl``: shift right logical, rs1 >> rs2
        - ``sra``: shift right arithmetic, rs1 >>> rs2, used for signed values
        - ``slt``: set if less than, (rs1 < rs2) ? 1 : 0
        - ``sltu``: set if less than unsigned, (rs1 < rs2) ? 1 : 0
        - no subtract immediate instruction in RISC-V, use ``addi`` with negated value
    * **Load & Store**
        - ``lb  rd,imm12(rs1)``: load byte, upper bits will be sign extended
        - ``lh``: load halfword, upper bits will be sign extended
        - ``lw``: load word
        - ``lbu`` and ``lhu`` will be zero extended for upper bits
        - ``sb  rs2,imm12(rs1)``: copy 8 bits into memory
        - `` sh`` for 16 bits, and ``sw`` for 32 bits
        - ``lwu, ld, sd`` for double word in RV64
    * **Conditional Branching**
        - syntax: ``<opcode>    rs1,rs2,imm12   # if condition(rs1,rs2) goto PC+imm12``
        - target address to jump to is PC relative
        - ``beq``: ==, ``bne``: !=, ``blt``: <, ``bge``: >=
        - unsigned condition testing with ``bltu, bgeu``
    * **Pseudo Instructions**
        - assembler will translate them into one or more machine instructions
        - ``j  label``: jump to address
        - ``call  label``: save return address in ``ra``
        - ``ret``: go to address in ``ra``, translated to ``jalr  zero,0(ra)``
        - ``li  rd,imm``: load immediate into destination, translated to ``addi  rd,zero,imm`` for
          12 bits, and ``lui  rd,upper(imm)`` and ``addi  rd,rd,lower(imm)`` for >12 bits
        - ``la  rd,label``: load address into destination
        - ``neg  rd,rs1``: negate, translated to ``sub  rd,zero,rs1``
        - ``mov  rd,rs1``: move, translated to ``addi  rd,rs1,0``
        - ``not  rd,rs1``: logical negate, translated to ``xori  rd,rs1,-1``
        - ``jr  reg``: indirect jump to target, translated to ``jalr  zero,0(reg)``
        - ``bgt  rs1,rs2,addr``: branch if greater than, translated to ``blt  rs2,rs1,addr``
        - ``ble  rs1,rs2,addr``: branch if less than or equal, translated to ``bge  rs2,rs1,addr``
        - ``bgtu`` and ``bleu`` for unsigned comparisons
        - ``beqz``: branch if equal to zero, translated to ``beq  rs1,zero,addr``
        - ``bnez, bltz, blez, bgtz, bgez`` are also translated similarly
    * **Jump & Link**
        - ``jal  rd,imm20``, should use ``call`` and ``j`` instead
        - save return address in destination register ``rd``, and perform PC relative jump where
          ``pc = pc + imm20``
        - to make instruction halfword-aligned, ``imm20`` is extended with additional 0 bit
        - ``jal  ra,addr``: same as ``call``
        - ``jal  zero,addr``: same as ``j``
    * **Jump & Link Register**
        - ``jalr  rd,imm12(rs1)``
        - save return address in destination register, ``rd``, and jump to ``pc = rs1 + imm12``
        - ``jalr  ra,0(t5)``: indirect ``call``
        - ``jalr  zero,0(t5)``: indirect ``j``
        - ``jalr  zero,0(ra)``: same as ``ret``
    * **Load Upper Immediate**
        - ``lui  rd,imm20``, where ``rd = imm20<<12``
    * **Long Jump & Long Call**
        - if target address requires 32 bits, use 2 instructions by breaking address into
          20 + 12 bits
        - long jump: ``lui  t0,upper20`` and ``jalr  zero,lower12(t0)``
        - long call: ``lui  to,upper20`` and ``jalr  ra,lower12(t0)``
    * **Add Upper Immediate to PC**
        - ``auipc  rd,imm20``, where ``rd = pc + (imm20<<12)``
        - used to make long jumps and calls with PC relative
        - long jump: ``auipc  t0,upper20`` and ``jalr  zero,lower12(t0)``
        - long call: ``auipc  t0,upper20`` and ``jalr  ra,lower12(t0)``
    * **Multiply**
        - in multiplication, lower bytes of signed and unsigned are same, but the upper bytes
          differ
        - number of result bits = 2x number of operand bits
        - ``mul  rd,rs1,rs2``: lower half of the result
        - ``mulh  rd,rs1,rs2``: multiply high, upper half of the result
        - ``mulhu``: unsigned, upper half of the result
        - ``mulhsu  rd,rs1,rs2``: ``rs1`` is signed, and ``rs2`` is unsigned
    * **Division**
        - number of result bits = number of operand bits
        - ``div  rd,rs1,rs2``: divide
        - ``rem  rd,rs1,rs2``: remainder
        - ``divu, remu`` for unsigned operands
        - RISC-V mandates truncated division for negative operands
        - overflow for ``max_negative / -1``: max_negative for quotient, and 0 for remainder
        - division by zero: all bits set to 1 for quotient, and dividend for remainder
    * **32-bit Operations for RV64**
        - ``addw, sub2, addiw, sllw, srlw, sraw, slliw, srliw, sraiw`` for word size
        - ``mulw`` for 32-bit multiply on RV64
        - ``divw, remw, divuw, remuw`` for 32-bit division on RV64
        - result is stored in lower 32 bits of ``rd``, and upper 32 bits of ``rd`` contain
          sign-extension of the lower
        - shift amount is from 0 to 31, taken from only lower 5 bits
    * **Linux-based Syscall**
        - set ``a7`` to the syscall number, ``a0..`` to the arguments for the syscall
        - invoke ``ecall`` to call the kernel
        - can just load null terminated argument and use ``call  printf`` if standard library is
          linked
        - can use ``.asciz`` to auto null terminate

        .. code-block:: s

           # can also use addi like the second ecall
   
           .global _start
   
           _start:
               li  a0,1                # arg1: stdout
               la  a1,helloworld       # arg2: buff
               li  a2,13               # arg3: buff length
               li  a7,64               # write syscall
               ecall
   
               addi a0,zero,0          # arg1: status
               addi a7,zero,93         # exit syscall
               ecall
   
           helloworld:
               .ascii "Hello, World\n"



RISC-V Directives
-----------------
    * **Memory Allocation**
        - always preceded by a label to create a variable
        - can initialise using comma-separated values
        - ``.byte  int``: allocate 1 byte and initialise with a value, often 0
        - ``.hword  int``: allocate 2 bytes and initialise
        - ``.half  int``: same as ``.hword``
        - ``.word  int``: allocate 4 bytes
        - ``.dword  int``: allocate 8 bytes
        - ``.ascii  "str"``: any Unicode or UTF-8
        - ``.asciz  "str``: same as ``.ascii`` with auto null terminated
        - ``.string  "str``: same as ``.asciz``
        - ``.skip  int``: allocate given byte count, useful for arrays
    * **Global**
        - ``.globl  symbol``
        - export and make a symbol available to the rest of the program, accessible by other
          object files when linking them together
        - assembler does not check the symbols, but the linker does
        - ``_start`` symbol must be in one of the files and exported to be the entry point of
          the program
    * ``.equ  symbol,val``: associate a value with the symbol, and the symbol can be used
      anywhere below the directive
    * ``.set  symbol,val``: same as ``.equ``
    * ``.align  int``: add enough bytes for the next instruction to be properly align
    * **Segments**
        - ``.text``: code, program usually begins with this directive
        - ``.data``: r/w data
        - can have multiple ``.text`` and ``.data`` segments in the file
        - ``.bss``: only data that evaluates to zero, object file will be optimised
        - ``.rodata``: read-only data

`back to top <#assembly>`_

References & External Resources
===============================

* Low Level. (2021). You Can Learn RISC-V Assembly in 10 Minutes | Getting Started RISC-V
  Assembly on Linux Tutorial [online]. Available at: https://youtu.be/GWiAQs4-UQ0?si=c6mdr-kJs_7gMJ9o
* Scheel, Jeff. (2024). RISC-V Technical Specifications [online]. Available at:
  https://lf-riscv.atlassian.net/wiki/spaces/HOME/pages/16154769/RISC-V+Technical+Specifications
* Borza, Juraj. (2021). RISC-V Linux syscall table [online]. Available at:
  https://jborza.com/post/2021-05-11-riscv-linux-syscalls/

`back to top <#assembly>`_
