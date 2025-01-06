*The processor is explained [[The Processor|here]]. This article is taken from [Wikibooks](https://en.wikibooks.org/wiki/Microprocessor_Design/Pipelined_Processors).* 

## Pipelining Introduction

Let us break down our microprocessor into 5 distinct activities, which generally correspond to 5 distinct pieces of hardware:

1. Instruction fetch (IF)
2. Instruction Decode (ID)
3. Execution (EX)
4. Memory Read/Write (MEM)
5. Result Writeback (WB)

Any given instruction will only require one of these modules at a time, generally in this order. The following timing diagram of the multi-cycle processor will show this in more detail:

![[Pasted image 20241219172622.png]]

This is all fine and good, but at any moment, 4 out of 5 units are not active, and could likely be used for other things.

### Pipelining Philosophy

Pipelining is concerned with the following tasks:

- Use multi-cycle methodologies to reduce the amount of computation in a single cycle.
- Shorter computations per cycle allow for faster clock cycles.
- Overlapping instructions allows all components of a processor to be operating on a different instruction.
- Throughput is increased by having instructions complete more frequently.

We will talk about how to make these things happen in the remainder of the chapter.

## Pipelining Hardware

Given our multi-cycle processor, what if we wanted to overlap our execution, so that up to 5 instructions could be processed at the same time? Let's contract our timing diagram a little bit to show this idea:

![[Pasted image 20241219172658.png]]

As this diagram shows, each element in the processor is active in every cycle, and the instruction rate of the processor has been increased by 5 times! The question now is, what additional hardware do we need in order to perform this task? We need to add storage registers between each pipeline state to store the partial results between cycles, and we also need to reintroduce the redundant hardware from the single-cycle CPU. We can continue to use a single memory module (for instructions and data), so long as we restrict memory read operations to the first half of the cycle, and memory write operations to the second half of the cycle (or vice-versa). We can save time on the memory access by calculating the memory addresses in the previous stage.

![[Pasted image 20241219172727.png]]

The registers would need to hold the data from the pipeline at that point, and also the necessary control codes to operate the remainder of the pipeline.

Our resultant processor design will look similar to this:

![[Pasted image 20241219172746.png]]
*CPU further explained [[The Processor#A Little More Detail|here]].*

If we have 5 instructions, we can show them in our pipeline using different colors. In the diagram below, white corresponds to a NOP, and the different colors correspond to other instructions in the pipeline. Each stage, the instructions shift forward through the pipeline.

![[Pasted image 20241219172925.png]]

Pipelined processors generate the same results as a one-instruction-at-a-time processor does when running the same software -- they just generate those results much more quickly. People who build pipelined processors sometimes add special hardware -- [[Processor Design - Hazards#Forwarding|operand forwarding]]; pipeline interlocks; etc. -- in order to get the same results "as if" each instruction is fetched, evaluated, and its results committed before the next instruction is fetched (non-overlapped) -- even though pipelined processors actually overlap instructions.

The **throughput** of a processor is the number of instructions that complete in a span of time. Many processors are designed to have a typical throughput of one instruction per clock cycle, even though any one particular instruction requires many cycles -- one cycle per pipeline stage -- from the time it is fetched to the time it completes.

## Superpipeline

Superpipelining is the technique of raising the pipeline depth in order to increase the clock speed and reduce the latency of individual stages. If the ALU takes three times longer than any other module, we can divide the ALU into three separate stages, which will reduce the amount of time wasted on shorter stages. The problem here is that we need to find a way to subdivide our stages into shorter stages, and we also need to construct more complicated control units to operate the pipeline and prevent all the possible **hazards**.

It is not uncommon for modern high-end processors to have more than 20 pipeline stages.

![[Pasted image 20241219174140.png]]
