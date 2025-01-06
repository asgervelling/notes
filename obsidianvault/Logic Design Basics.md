## Overview

-  Information encoded in Binary
  - Low voltage = 0, high voltage = 1
  - One wire per bit.
  - Multi-bit data encoded on multi-wire buses.
- [[#Combinational Elements|Combinational elements]]
  - Operate on data
  - Output is a function of input
- [[#Sequential Elements|State (sequential) elements]]
  - Store information

## Elements
Logic design (and processors) have different types of elements (electronic components).

### Combinational Elements

The apparatuses inside the CPU (along the datapath) are called combinational elements.
They may include logic gates, the control unit, multiplexors and more. I have gathered the ones I know here.

### Logic Gates
See [[Logic Gates]].

#### Multiplexer
`Y = S ? l0 : l1`
Has two data inputs `l0` and `l1`, and a select input `S`, as well as an output `Y`.
It is also called a selector, since the value of `S` determines which data input becomes the output.

![[Pasted image 20241219122949.png]]

#### Adder
`Y = A + B`
![[Pasted image 20241219123309.png]]

#### Arithmetic Logic Unit (ALU)
`Y = F(A, B)`
![[Pasted image 20241219123338.png]]

### Sequential (State) Elements

#### Register
- Stores data in a circuit.
- Uses a clock signal to determine when to update the stored value.
- Edge-triggered: Updates when Clk changes from 0 to 1

![[Pasted image 20241219123911.png]]
*D: Data input. Q: Data output (for when you need to read the value)*

#### Register with Write Control
- Only updates on clock edge when write control input is 1
- Used when update is conditional

![[Pasted image 20241219124002.png]]

### Clocking Methodology
Combinational logic transforms data during clock cycles
- Between clock edges
- Input from state elements, output to state elements
- Longest delay determines clock period

![[Pasted image 20241219124410.png]]
