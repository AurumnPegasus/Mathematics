# Dataset and FSM

## Van Neumann Model

It has three parts

* CPU : Performs operations on values stored in registers or memory
* Main Memory : Array of W words each of N bits
* Input/ Output devices 

Key idea is that the memory stores all the instructions as a sequence of instructions which must be executed. Then the CPU fetches, interprets and executes those instructions interacting with memory or input/ output devices if necessary. 

Hence the memory stores both **data and instructions**

The control unit contains Program Counter which keeps track of the instruction which has to be executed next. It then interprets those instructions and produces a sequence of control signals so as to use the datapath to execute it. 

In a Van Neumann machine, instructions are interpreted by the following steps

* Fetch
* Decode
* Memory Read
* Execute
* Memory Write
* PC Update

## Instructional Set Architecture

The ISA is a detailed functional specification of the operations and storage mechanisms and serves as a contract between the designers of the digital hardware and the programmers who will write the programs. 

It is generally hard to change to ISA when technology improves since there is a powerful economic incentive to ensure that old software can run on new machines, which means that a particular ISA can live for decades and span many generations of technology. If you ISA is successful, you will have to live with any bad choices you made for a very long time.

Why have separate registers and main memory?

* Modern programs and datasets are huge, so we will want to have a large main memory to store all the data.
* The tradeoff here is that large memories require a lot of time for access, and can only access one location at a time. Hence they don't make good storage for use in each instruction which needs to access several operands and store a result.
* If we were to use main memory, ignoring the time complexity, the space complexity would jump as well, since each encoding will require 3 32 bit addresses to refer to 2 destination values and 1 operation 
* On the other hand, if we were to use registers to hold operands and serve as destination, we can design the register hardware for parallel access and make it very fast.
* To keep the speed up we wont be able to have  very many registers, another tradeoff.

There are 3 types of instructions

* Arithmetical instructions which perform simple arithmetical operations
* Load/ Store instructions which interact with the main memory to either load or store data from registers
* Branch instructions which changes the value of PC.

**Fixed Length encoding** of instructions (which is used in ISA) are often inefficient in the sense that the same information content can be encoded using fewer bits. Here, to do better, we would need a **variable length encoding** for instructions, where frequently occurring instructions would use shorter encoding (Huffman encoding).

But hardware to decode variable-length instructions is complex since there may be several instructions packed into one memory word, while other instructions might require loading several memory words.

### ALU

![alt text](https://github.com/AurumnPegasus/Notes/blob/master/CSO/Zvonko_Zaky/Images/Annotation%202020-06-21%20001958.png?raw=true)

Here, OPCODE is a 6 bit field referring to the operation to be performed. rc is the destination register, whereas ra and rb are source register. The rest of the bits are marked as unused and should be set to 0.

Our first "feature request" is to allow small constants as the second operand in ALU instructions.

So if we replaced the 5-bit "rb" field, we would have room in the instruction to include a 16-bit constant as bits [15:0] of the instruction.

The argument in favor of this request is that small constants appear frequently in many programs and it would make programs shorter if we didn't have use load operations to read constant values from main memory.

The argument against the request is that we would need additional control and datapath logic to implement the feature, increasing the hardware cost and probably decreasing the performance.

We find that there is compelling evidence that small constants are indeed very common as the second operands to many operations. Note that we're not so much interested in simply looking at the code.

* We see that over half of the arithmetic instructions have a small constant as their second operand.

* Comparisons involve small constants 80% of the time. This probably reflects the fact that during execution comparisons are used in determining whether we've reached the end of a loop.

* And small constants are often found in address calculations done by load and store operations.

Operations involving constant operands are clearly a common case, one well worth optimizing.

Therefore, the new structure looks like :

![image-20200621061549940](C:\Users\91961\AppData\Roaming\Typora\typora-user-images\image-20200621061549940.png)

The constant is stored in 2s complement format. Therefore we lose one destination register to ensure we have the 16-bit signed constant. If there is some constant which does not fit in 16-bit signed value, we will need to store it in the main memory and load it when necessary.



### Load/ Store

The same structure is followed for load and store instructions as the one with constants used in ALU. To access memory, we'll need a memory address, which is computed by adding the value of the "ra" register to the sign-extended 16-bit constant from the low-order 16 bits of the instruction.

The store instruction is special in several ways: it's the only instruction that needs to read the value of the "rc" register, so we'll need to adjust the datapath hardware slightly to accommodate that need. Store instruction is also the only instruction which does not write the result in the register file. 

### Branches

Changing the PC depending on some condition is implemented by a branch instruction, and the operation is referred to as a "conditional branch". When the branch is taken, the PC is changed and execution is restarted at the new location, which is called the branch target. If the branch is not taken, PC is incremented by 4 and execution continues as normal. 

Say BEQ refers to the condition "Branch if equal to".  First the usual PC 4 calculation is performed, giving us the address of the instruction following the BEQ. This value is written to the "rc" register whether or not the branch is taken. 

Next, BEQ tests the value of the "ra" register to see if it's equal to 0. If it is equal to 0, the branch is taken and the PC is incremented by the amount specified in the constant field of the instruction. Actually the constant, called an offset since we're using it to offset the PC, is treated as a word offset and is multiplied by 4 to convert it a byte offset since the PC uses byte addressing. If the contents of the "ra" register is not equal to 0, the PC  is incremented by 4 and execution continues with the instruction following the BEQ.

The branches are using what's referred to as "pc-relative addressing". That means the address of the branch target is specified relative to the address of the branch, or, actually, relative to the address of the instruction following the branch. So an offset of 0 would refer to the instruction following the branch and an offset of -1 would refer to the branch itself.

 Negative offsets are called "backwards branches" and are usually seen at branches used at the end of loops, where the looping condition is tested and we branch backwards to the beginning of the loop if another iteration is called for. Positive offsets are called "forward branches" and are usually seen in code for "if statements", where we might skip over some part of the program if a condition is not true.

Now, the BEQ will let us just test equality, but what if we wanted to test inequality. That's where the compare instructions come in - they do more complicated comparisons, producing a non-zero value if the comparison is true and a zero value if the comparison is false. Then we can use BEQ and BNE to test the result of the comparison and branch appropriately.
