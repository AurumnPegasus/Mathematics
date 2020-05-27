# Basic Structure of Computers

## Functional Units

### Input Unit

Normal input units like keyboard, mouse et all

### Memory Unit

#### Primary Memory

Also known as *main memory*, is basically the fast memory which operates at electronic speeds. Programs must be stored in this memory while they are being executed. This contains large number of storage cells, which are handled in groups called *words*. The memory is so organized  that one word can be extracted or stored in a basic operation. The number of bits in each word is referred to as *word length*. Each word is associated with an address so as to facilitate fast operations. 

A memory in which a word can be accessed in a short and fixed amount of time on specifying the address is called *random-access memory*. The time required to access one word is called as *memory access time*. 

#### Cache Memory

As an adjunct to the main memory, smaller, faster RAM unit, called cache, is used to hold bits of programs which are currently being executed. The cache is tightly coupled with the processor and is usually integrated on the same chip. The purpose of cache is to facilitate high instruction execution rates. 

#### Secondary Storage

Although primary memory is essential, it tends to be expensive and does not retain information when power is turned off. Thus additional, less expensive, permanent secondary storage is used when large amounts of data and many programs have to be stored, particularly for information that is accessed infrequently.

### Arithmetical and Logical Unit

Operands are brought into the processor for any arithmetical or logical operation. These are stored in high speed, temporary storage units called *registers*. Each register can store upto one word of data. Access time to a register is lesser than both cache or main memory blocks.

### Output Unit

Normal output units like desktop

### Control Unit

The operations of all the above mentioned units must be co-ordinated in a way, which is the responsibility of Control Unit. They are responsible for generation of the *timing signals* that govern the transfer of data and determine when a given action takes place. 

## Basic Operational Concepts

```
load r2, src
add r2, r4
store r2, src
```

Load here loads the integer which is stored in the address src. Then add adds the values stored in the registers in the ALU and stores it in r2, which is then stored back in src by the command store. 

<img src="./Images/Connection_bw_processor_mainmemory.png" alt="image info" style="zoom:67%;" />

As we can see, the control unit also consists of special registers like *Program Counter* ( PC ) or *Instruction Register* ( IR ), which serve very specific functions. It also has n general purpose registers, often called as *processor registers*.

Sometimes, normal execution of a program may be preempted if some device requires urgent service. In such a case, execution of the current program must be suspended. To cause this, the device raises and *interrupt* signal, which is a request for service by the processor. The processor provides the requested service by executing a program called an *interrupt-service routine*. Since such diversions alter the internal state of the processor, its state must be saved in the memory before servicing the interrupt request. 

## Parallelism

Performance can be increased by doing number of operations in parallel. 

- ### Instruction Level :  

  If we were to overlap steps of successive instruction, the execution time decreases as we do not have to wait for the first instruction to complete before starting the other one.

- ### Multicore Processor

  Multiple processing units can be fabricated in a single chip. Each processing unit is called a *core*.

- ### Multiprocessor

  Computer Systems may contain multiple processor, each containing multiple cores. Such systems are called *multiprocessor*. These either execute a number of different instructions in parallel, or subparts of the same instruction in parallel. All processors usually have access to all the memory in such systems, and the term *shared system memory* is used in such cases. 





**The ratio of time for executing of a program without cache to time for executing a program with cache is called *speedup***

