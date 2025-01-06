Uses [[Logic Design Basics]].

## High Level Overview
*This introduction is taken from [this YouTube video](https://www.youtube.com/watch?v=1U4v_2J0Qwk) and may not accurately mirror the RISC-V ISA.*


The CPU essentially cycles through the fetch-decode-execute cycle

1. It fetches an instruction from memory
2. It decodes the instruction
3. It executes it, and starts over with the next instruction

![[Pasted image 20241219162245.png]]

The program counter (PC) holds the address of the next instruction, and the instruction register (IR) contains the current instruction.

The cycle may have another part: Store.
However, because write operations take much longer than arithmetic, it may be considered not a part of the fetch-decode-execute cycle.

![[Pasted image 20241219162555.png]]


In broad strokes, these are the units responsible for each phase:
- The control unit to decode
- ALU to execute
- RAM or other parts of the [[Memory Hierarchy|memory hierarchy]] to store data.

![[Pasted image 20241219162643.png]]

#### A Little More Detail

Here are the four cycles, fetch, decode, execute and store, respectively, with a little more detail of the CPU.

![[Pasted image 20241219164001.png]]

The boxes in this diagram are the functional parts of the CPU, and the arrows show how data flows between them. More detail about the [[#Datapath|datapath]] to follow.

Let's look at some of the components:
- Memory: This isn't really in the processor. This is the circuitry that interacts with memory.
- [[Arithmetic Logic Unit (ALU)|ALU]]: Can do arithmetic and logical operations.
- [[#Register File]]: Holds the registers of the processor.

The four columns in this picture:
- Fetch phase: Uses the address in PC (the green component) to fetch the instruction from memory. While that's happening, it's also adding to PC, so that the PC will contain the address of the next instruction.
- Decode phase: Abstraction hidden in the tall green box. It tells the registers which ones will be used, and it will figure out what operation the ALU should do when the instruction gets to the execute phase.
- Store phase: A memory access phase. While this is where we write into memory, it is also where we read from memory. That means that each instruction can either read or write, but it can't do both.
  If we read data, it will go right into the register file (think of `lw x1, offset(x2)`).

There is also a whole section only dedicated to the program counter. 
This adder's sole purpose is dedicated to calculating the next PC and get it ready for the next cycle:

![[Pasted image 20241219165442.png]]

Note that we use multiple [[Logic Design Basics#Multiplexer|multiplexers]] in this design, denoted MUX.
In the next figure, there is a multiplexer whose
- First input comes from the adder that incremented the program counter
- Second input comes from the output of the execute phase

![[Pasted image 20241219165628.png]]

The reason we need that multiplexer is that there is some sort of jump or branch instruction.
In that case, the instruction decoder will pull the address, that we are supposed to jump to, out and pass it forward.
Then the execute stage will pass it forward. That is the other input to the multiplexer:

![[Pasted image 20241219170049.png]]

The instruction decoder will also pass forward a control line, telling the multiplexer which new PC to pick.

More on pipelined processors: [[Pipelined Processors]].
## Performance

The performance of a computer is given by three key factors:
- Instruction count (determined by the compiler and instruction set architecture)
- Clock cycle time (determined by the processor)
- Clock cycles per instruction (CPI - determined by the processor)

## Examining a Simple RISC-V Implementation

We will look at a single-cycle design, with a simple subset of RV32I:
- Memory reference: `lw` (load word)  and `sw` (store word)
- Arithmetic-logical instructions: `add`, `sub`, `and` and `or`
- Control transfer: `beq`

From this subset, we will build a simple datapath.

## Datapath

From Wikipedia:
> A data path is a collection of functional units such as [[Arithmetic Logic Unit (ALU) | arithmetic logic units (ALUs)]] or multipliers that perform data processing operations, registers, and buses. Along with the control unit it composes the central processing unit (CPU). A larger data path can be made by joining more than one data paths using multiplexers.
 
> A **data path** is the ALU, the set of registers, and the CPU's internal bus(es) that allow data to flow between them.

In RISC-V, many instructions require a data path that allows for the following things to happen:
- Send the program counter (PC) to the memory that contains the code and fetch the instruction from that memory.
- Read one or two registers, using fields of the instruction to select the registers to read. For the `lw` instruction, we need to read only one register, but most other instructions require reading two registers.

Depending on instruction class:
- Use ALU to calculate
  - Arithmetic result
  - Memory address for load/store
  - Branch comparison
- Access data memory for load/store
- PC <- target address or PC + 4

### Building the Datapath Incrementally

#### Instruction Fetch

An abstract overview. An [[Logic Design Basics#Adder|adder]] is used to increment PC by 4. We will soon look at what happens when we need to increment by another value.
![[Pasted image 20241219125206.png]]

#### R-Format Instructions

Recall the R instruction format from the RISC-V specification:
![[Pasted image 20241219125526.png]]

#### Register File

A state element that consists of a set of registers that can be read or written by supplying a register number to be accessed.

R-format instructions have three register operands, so we will need to read two data words from the register file and write one data word into the register file for each instruction. For each data word to be read from the registers, we need an input to the register file that specifies the *register number* to be read and an output from the register file that will carry the value that has been read from the registers.
To write a data word, we will need two inputs: one to specify the register number to be written and one to supply the data to be written into the register. The register file always outputs the contents of whatever register numbers are on the Read register inputs. Writes. however, are controlled by the write control signal, which must be asserted (1) for a write to occur at the clock edge. Figure a below shows the result; we need a total of three inputs (two for register numbers and one for data) and two outputs (both for data). The register number inputs are 5 bits wide to specify one of the 32 registers ($32 = 2^5$), whereas the data input and two data output buses are each 32 bits wide.

In short, R-format instructions (such as `add`) are used to
- Read two register operands
- Perform arithmetic logical operation
- Write register result

![[Pasted image 20241219142205.png]]
**The two elements needed to implement R-format ALU operations are the register file and the ALU.** *The register file contains all the registers and has two read ports and one write port. The register file always outputs the contents of the registers corresponding to the Read register inputs on the outputs; no other control inputs are needed. In contrast, a register write must be explicitly indicated by asserting the write control signal. Remember that writes are edge-triggered, so that all the write inputs (i.e. the value to be written, the register number and the write control signal) must be valid at the clock edge. Since writes to the register file are edge-triggered, our design can legally read and write the same register within a clock cycle: The read will get the value written in an earlier clock cycle, while the value written will be available to read in a subsequent clock cycle. The inputs carrying the register number to the register file are all 5 bits wide, whereas the lines carrying data values are 32 bits wide. The operations to be performed by the ALU is controlled with the ALU operation signal, which will be 4  bits wide, using the ALU designed in Appendix A (of COD). We will use the Zero detection output of the ALU shortly to implement conditional branches.*

#### Load/Store Instructions

To implement instructions like `lw x1, offset(x2)` and `sw x1, offset(x2)`, we will need two units:
- A data memory unit
  - Has an input for the address, an input for the write data and a Read data output.
  - There are separate read and write controls, although only one of these must be asserted at a time
- An immediate generation unit, using an ALU that sign-extends the offset to 12 bits.
- Load: Read memory and update register
- Store: Write register value to memory

![[Screenshot from 2024-12-19 14-44-05.png]]

#### Branch Instructions

- Read register operands
- Compare operands
  -  Use ALU, subtract and check Zero output
- Calculate target address
  - Sign-extend displacement
  - Shift left 1 place (halfword displacement)
  - Add to PC value

![[Screenshot from 2024-12-19 14-48-38.png]]

### Composing the Elements

- First-cut data path does an instruction in one clock cycle
  - Each datapath element can only do one function at a time
  - Hence, we need separate instruction and data memories
- Use multiplexers where alternate data sources are used for different instructions

#### Encoding vs Datapath

![[Pasted image 20241219175219.png]]

#### Full Datapath

![[Pasted image 20241219175305.png]]


file:///home/a/Downloads/slides-13.pdf
