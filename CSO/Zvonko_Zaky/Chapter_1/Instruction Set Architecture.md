# Instruction Set Architecture 

## Memory Location and Address

Accessing the memory to store/access any single item of information, either a word or a byte, requires the particular address. It is customary to use numbers from 0 to 2<sup>k</sup> − 1, for some suitable value of k, as the addresses of successive locations in the memory. Thus,  the memory can have up to 2<sup>k</sup> addressable locations. The 2<sup>k</sup> addresses constitute the *address space* of the computer.

We know there are three types of memory units in a computer: bit, byte and word. It is not practical to allocate address to each and every bit, so we consider byte as the lowest level. Address is allocated to each and every byte, and a system which does so has *byte addressable  memory* .

### Big Endian and Little Endian

* Big Endian: Left most bit is least significant bit
* Little Endian: Left most bit is most significant bit

The commonly used notation is Big Endian. 

### Word Alignment

In case of a 32 bit word, the natural boundary occurs at 0, 4, 8 and so on. We say a location has *aligned* addresses if the address starts at a multiple of the number of bytes in the first one. 

## Memory Operations

There are two basic operations which are needed for everything, *memory read* and *memory write*. 

The read operation transfers a copy of the contents of a specific memory location to the processor. The memory contents remain unchanged. 

The write operation transfers an item of information from the processor to a specific memory location, overwriting the former contents of that location.

## Instruction and Instruction Sequencing

A computer must have instructions capable of performing four types of operations:

* Data transfer between memory and processor registers
* Arithmetical and Logical operations
* I/O transfer
* Program sequencing and control

### RISC and CISC Instruction set

There are two fundamentally different approaches in the design of instruction sets for modern computers. 

One popular approach is based on the principle that each instruction should not occupy more than one word in memory, and all operands needed to execute a given arithmetic or logic operation specified by an instruction are already specified in the processor register. This approach is conductive to an implementation of the processing unit in which the various operations needed to process a sequence of instructions are performed in "pipelined" form to overlap activity to reduce total execution time. The restriction on memory length of each instruction requires that the instructions are simple and basic, hence this type is called *Reduced Instruction Set Computers* ( RISC )

An alternative to the RISC approach is to make use of more complex instructions which may span more than one word of memory, and which may specify more complicated operations. Computers based on this idea are called *Complex Instruction Set Computers* (CISC).

### RISC

The main principles it follows are:

* All the memory locations can only be accessed by Load instruction.
* All the required operands for a particular operation must already be stored in the processor register or must be provided by the instruction itself.

For example, to do a simple C = A + B, we have to do 

```
load r2, A
load r3, B
add r4, r2, r3
store r4, c
```

## Addressing Modes

In general, a program operates on data that resides in the computer's memory. This data can be organized in a variety of ways that reflect the natures of the information and how it is used. Programmers use data structures such as lists and arrays for organizing the data used in computations. 

When translating a high-level language program into assembly language, the compiler generates appropriate sequences of low-level instructions that implement the desired operations.

The different ways for specifying the locations of instruction operands are known as *addressing modes*.

![](D:\CSO\Notes\Images\Addressing_Modes.png)

### Implementation of Variables and Constants

The program uses only two modes to access a variable, namely:

* Register Mode: Here, the Register contains the required operand, and the name of the register is given.
* Absolute Mode: Here, the operand is stored in main memory at location LOC, which is given in the instruction.

Constants are represented using Immediate mode only, where it can be written as

```
mov r2, #100
```

Here, the constant 100 is stored in register R2.

#### Indirection and Pointers

A register is called as a *pointer* when the value stored within it points to a particular location in the memory, that is, it stores the address of a particular location in the memory.

Indirect Mode: The effective address of the operand is stored in a register which is specified in the instruction.

Indirect Addressing through memory, though possible in CISC type architecture, is not allowed in RISC type architecture. 

A program for finding the sum of 10 elements stored consecutively in the memory.

```assembly
load r2, N					# Size of the list
clear r3					# sum = 0
mov r4, #NUM				# store the address of the first number
Loop:
Load r5, (r4)				# Load the new value
add r3, r3, r5				# sum = sum + a[i]
add r4, r4, #4				# increment i by 1
sub r2. r2. #1				# decrement counter by 1
cmp r2, #0					# conditional branching
jg Loop
store r3, SUM				
```

#### Indexing and Arrays

Index Mode: The effective address of the operand is generated by adding a constant value to the contents of a register.

The registered used in such a case is called *index register*.

The effective address of the operand is then denoted by

EA = X + [R]

In an assembly-language program, whenever a constant such as the value X is needed, it may be given either as an explicit number or as a symbolic name representing a numerical value.

An effective address can be represented in two ways

* X stores the offset and [R] stores the base address
* [R] stores the base address and X stores the offset.

