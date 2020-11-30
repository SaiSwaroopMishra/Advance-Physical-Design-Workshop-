# Advance-Physical-Design-Workshop-
This is a repository on Advance Physical Design Workshop conducted by VLSI System Design(VSD) Corporation on VSD-IAT platform.

## Day 1

First day was to see the chip from the top level without diving into it. 

### Basic Intoduction of a chip
- Package: QFN-48 (QFN stands for Quad Flat No leads)
- Inside the package at the center we have the chip
- Using PADS we can send the signal inside the chip
- Foundary IPs - PLL, SRAM, ADC, DAC, etc
- Macros - RISC-V SOC, SPI, etc

### RISC-V ISA
An introduction to RISC-V architeture:
- Translation from high-level language to machine code: C -> ASM -> binary code (compiler + assembler)
- RISC-V Architecture can be considered as a specification. And RTL(picorv32 CPU core) implements this specification.
- Then RTL to layout is a standard RTL2GDS flow.
PicoRV32 - Clifford Wolf's open source implementation of a RISC-V RV32IMC.
- I stands for BAsic Integer Instruction Set.
- M stands for Hardware Multiplier adn Divider.
- C stands for compressed instrucitons(16 bit).
SoC is the complete system on a chip. Ex: Raven SOC = PicoRV32(options selected) + RAM blocks + SPI Flash Controller + UART + GPIO + I/O Address Decoder + IRQ Routing

### Design Flow
The main goal is to bring together opensource RTL Design, opensource EDA Tools and opensource PDK. 

Here we are using the openlane flow. On June 30, 2020 google had an agreement with skywater to opensource the FOSS 130nm Production PDK.

The github link for the following: github.com/google/skywater-pdk

Anyone can git clone this to use on his/her own system.

PDK: PDK is the collection of files used to model a fabrication process for the EDA tools used to design an IC. This constitutes of the following files:
- Process Design Rules: DRC, LVS, PEX
- Device Models
- Digital Standard Cell Libraries
- I/O Libraries
- .............

ASIC flow:
- Synthesis (Logic Synthesis and Cell Mapping)
- Floorplanning
- Placement
- Clock Tree Synthesis (CTS)
- Routing
- Sign-off (STA + LVS + DRC Clean).

In ASIC flow we take the RTL Design and go through all the steps mentioned above to produce the final layout or GDSII. This is also called as RTL2GDS format. And the whole process uses the sky130 PDK here. 

eFabless decided on creating a reference opensource ASIC implementation methodology and flow. We call this flow as openlane.

Example of the chips developed using openlane flow are striVe, striVe 2, etc.

Tools used here are:

- Yosys - Logic Synthesis
- abc - cell Mapping
- graywolf - Placement
- Magic - VLSI Layout tool
- Netgen - LVS
- OpenSTA - Static timing analysis tool 
- Global Route - Fast Route
- Detail Route - Triton Route

### LAB:





















#Day 4
Press 'g' to activate the grid. And to deactivate press 'g' again.
the arguments required by grid is x spacing y spacing x origin and y origin
We need to converge the grid definitions to track definitions
The ports are at intersection of horizontal and vertical tracks ensures that the routing can reach the ports from x as well as y direction.
Width of the standard cells must be odd multiples of x pitch of that layer
