# Project 3 README

Student 1: Calvin Yee 1 (123456789)
Student 2: Anvi Kalucha (123456798)

## Project Status
This the current status of the project ...
Adder15 circuit done, made from 3 5-bit CLA adders.
ALU15 circuit done.
REG15 done, Flags8 done, IMM15 Done, Decoder Done. 
CPU working with U, R, I type instructions. 

## Known Issues
The project fails to function properly in ...
Some issues with control flow. 

## References
Mainly textbook used as ref. 
https://www.binaryhexconverter.com/hex-to-binary-converter for getting instr.


## Generative AI Use

### Prompt 1
What does Oscillation Apparent mean in Logisim Evolution?

### Response 1
In Logisim Evolution, "Oscillation Apparent" is a warning that indicates a potential problem in your circuit related to signal instability or unintended feedback loops. This occurs when the simulation detects that a signal is rapidly changing states (oscillating) in a way that suggests there might be a logical error, such as:

Feedback Loops: When a signal is fed back into itself without proper timing or control, causing it to rapidly flip between states.

Unstable Combinational Logic: If combinational logic has paths that create loops without any delay elements (like flip-flops), the circuit might oscillate, as the output continuously affects its own input.

Race Conditions: These happen when the timing of signals causes conflicts, leading to unpredictable behavior. This can result in parts of the circuit repeatedly switching states in response to the same signal change.

Consequences
Performance Issues: The simulation might slow down significantly because of the rapid changes it has to compute.
Incorrect Behavior: The circuit might not perform as expected, leading to wrong outputs or failure to stabilize in a correct state.
Troubleshooting Oscillations
To address oscillation warnings, you can:

Check for Feedback Loops: Ensure that any feedback paths in your circuit are properly controlled by flip-flops or other sequential logic elements to break any potential for oscillation.

Add Delay Elements: If necessary, introduce flip-flops or latches to control the timing of signals in feedback loops or to break combinational cycles.

Use Stable Logic Design: Ensure that your combinational logic is well-designed, with clear, stable outputs that don’t depend on rapidly changing inputs.

Simplify Your Circuit: Sometimes simplifying the logic or breaking down complex combinational logic into smaller, more manageable parts can resolve oscillations.

Example
If you have a circuit where an output directly feeds back into an input without any delay or synchronization, it might cause the output to oscillate rapidly between 0 and 1. Adding a flip-flop in the feedback path could stabilize the circuit by ensuring that the output changes only at clock edges, avoiding oscillation.

By understanding and addressing the cause of oscillation, you can ensure that your circuit functions correctly and efficiently in Logisim Evolution.
### Changes 1
This did not help me debug, and eventually i found the issue on my own. 

### Prompt 2

### Response 2

### Changes 2

### Adder15
Used an ADDER5 circuit to make the 5 bit cla adder, then chained 3 of them together. 

### ALU15
Used the adder to perform ADD/ADDI, and used a NOT gate + 1 to do subtraction. 
AND, OR, XOR, all just 15 bit gates. 
Shifting done with splitters and muxes based on shift bits. 

### DECODE15
Decoder only uses ROMs to encode the opcode -> output. 1 rom for every multi-bit output, while the lw, swi, and sw are all hardcoded. 

### Imm15 
Uses splitters to get immediate bits from the instruction. OPcode helps 
decide which immediate bits to grab. 

the multiple cases at the end of the pdf are handled by a 2nd mux, whereas the 1st 3 general cases can be selected by the 2MSB of the opcode. 

### REGS15
regs15 is done via 8 different registers, with x0 hardwired to 0, and the rest
enabled to be written by the W input. 
SelID is used as the select line for regs. 

Data is written directly to each reg except for x0. 
A and B are similarly just choosing which of the 8 via the 3 bits. 

### Flags8
Flags are done by storing each flag in a register, and write enabling with the WM, using the NF as input. 

Bits are combined using a splitter, with the 2 empty slots as 0 always. 

### CPU15

The cpu has a T ff as the main states, with fetch-decode and writeback. 
It starts in Fetch mode, so the 1st instruction is gotten. 

The decoder serves as the select lines for REGSRC, the src for A/B, and the Flagsrc. 

A and B are selected by the rs1 and rs2 bits of the instruction, and so is rd to the SelD REG15 input. 

They each have multiple inputs such as IMM15, A/B of REG, and PC. 

The ADDR output is written to by the PC on Fetch, and during writeback is set to the ALUResult (for load/store)

RE is enabled on fetch or LW, while WE is only enabled on SW AND Writeback. 

the PC is a counter, which is loaded with the ALURes when a branch or jump is encountered, and increments normally otherwise. 

IB is populated only on fetch and is written directly from the DATAIN. 

Registers are only written to on writeback, and have sources such as IMM, ALURes, or PC + 1, or DATAIN (for lw)

DATAOUT is written to only when SW is active, and is tri state buffered otherwise. IRQ and I/E flags not yet implemented. 