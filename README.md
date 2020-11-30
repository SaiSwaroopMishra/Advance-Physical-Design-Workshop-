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

Inside openlane (Package step):

Command: package require openlane 0.9

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/openlane.PNG)

Preparation step:

command: prep -design picorv32a

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/preparation%20complete.PNG)

Synthesis step: 

command: run_synthesis

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/synthesis%20is%20successful.PNG)

Synthesized netlist present inside results directory

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/synthesised%20netlist%20present%20inside%20results.PNG)

Part of the synthesized netlist

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/part%20of%20the%20synthesised%20netlist.PNG)

All the timing reports generated inside reports directory after synthesis

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/all%20the%20timing%20reports%20generated%20inside%20the%20reports%20directory.PNG)

The marked directory is the synthesis statistics report

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/the%20marked%20directory%20is%20the%20synthesis%20statistics%20report.PNG)

Inside Statistics file...

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/Inside%20statistics%20file.PNG)

Design Statistics report

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/flop%20ratio(statistics).PNG)

Flop Ratio:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/flop%20ratio(calculated).PNG)

In the mcq given as assignment the answer was 0.0912 which was very near to this.

Buffer Ratio:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/buffer%20ratio.PNG)

Chip Module Area:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/chip%20module%20area.PNG)

Part of the opensta timing reports

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%201/part%202%20of%20opensta%20timing%20report.PNG)

## Day 2

### LAB:

After running placement:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/after%20placement.PNG)

Zooming in we can see how the standard cells are placed

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/after%20zooming%20in%20we%20can%20see%20how%20the%20standard%20cells%20are%20placed.PNG)

Opening magic:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/when%20magic%20opens.PNG)

Use ctrl+S to select the design and use ctrl+V to move it to the center

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/ctrl%20S%20to%20select%20and%20ctrl%20V%20to%20move%20to%20the%20center.PNG)

Die area inside the def file

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/die%20area%20inside%20the%20def%20file.PNG)

Calculation of the die area:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/calculation%20of%20die%20area.PNG)

Changing the clock period inside config.tcl file

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/changing%20the%20clock%20period%20inside%20conig.tcl%20of%20the%20design.PNG)

But when we echo the clock perod we get is the same

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/still%20showing%20the%20same%20value%20of%20clock.PNG)

Using -overwrite command during the preparation stage

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/overwrite%20the%20design%20files%20using%20-overwrite%20command.PNG)

Getting the changed clock period after using -overwrite

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/getting%20the%20changed%20clock%20period%20after%20using%20overwrite.PNG)

But the problem with this is that we loss all steps that we have done. That means again we have to do synthesis. An easier method to change the clock period inside openlane is by using the set env(CLOCK_PERIOD) command.

Changing the clock period inside openlane:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/using%20set%20env%20to%20change%20the%20clcok%20period.PNG)

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%202/change%20clock%20period%20to%2010.000.PNG)




















#Day 4
Press 'g' to activate the grid. And to deactivate press 'g' again.
the arguments required by grid is x spacing y spacing x origin and y origin
We need to converge the grid definitions to track definitions
The ports are at intersection of horizontal and vertical tracks ensures that the routing can reach the ports from x as well as y direction.
Width of the standard cells must be odd multiples of x pitch of that layer
