# Project 3 README
Developer 1: Calvin Yee

## Project Status
CPU working with S, B, U, R, I type instructions.

##
Instructions for running CPU program: 

  Pull from logisim-evolution current repository or download pre-built binary (Java) 
  Link: https://github.com/logisim-evolution/logisim-evolution/releases
  Run binary or write "gradlew run" to start Logisim-Evolution
  Open the .circ file and use Python assembler to build .dat files for loading if you want to run personalized ASM testing files. 

## 
Subcircuit implementations below:

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
