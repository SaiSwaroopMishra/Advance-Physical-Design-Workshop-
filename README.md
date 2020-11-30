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

### Chip Floorplaning
1) Define width and height of core and die based on standard cell dimensions
- Place all standard cells inside the core
- Aspect Ratio = Height/Width
- Utiliztion Factor = Area occupied by the netlist/Total Area of the core 
2) Define location of pre-placed cells
- All of them can be implemented once and can be instantiatd multiple times on to a netlist
- The arrangement of the cells (IP's, blocks, modules) is referred as floorplanning
- pre-placed cells are placed in user-defined locations before automated place and route
- Examples of preplaced cells are memories, clock gating cells, comparator, mux. 
3) Surround pre-placed cells with decoupling capacitors.
- Noise margin(NMh for '1' and NMl for '0')
- NMh = Voh - Vih
- NMl = Vil - Vol
- Vdd or Vss could drop without decoupling caps between Vdd and Vss;
- Decoupling capacitors are huge capacitors between Vdd and Vss and need to be placed near the circuit/block.
4) Power planning
- If many blocks discharges from '1' to '0' at same time in a single ground cause a bump (Gnd bounce). If from '0' to '1' a voltage droop (for single Vdd).
- Istead of single supply lines, use multiple arrays of power supply (Vdd and Vss points). 
5) Pin placement
- Connectivity is described using VHDL or verilog;
- try to put pins near blocks;
- bigger PADs for clock pins.
6) Logical Cell placement blockage --> add PAD ring blocks.

### Placement 
1) Before placement is required to bind netlist with physical cells. 

So, we need a **library**, that is a collection of blocks/cells with different flavors, sizes, Vt(Threshold Voltage), etc.

2) Place the cells

3) Optimize the placement
 - Check signal integrity
 - Add buffers for long paths

### Cell Design Flow (standard cells)

**Inputs:** PDKs, rules (DRC e LVS) and tech files, SPICE models, libraries and user defined specifications (supply voltages, layers, cell sizes, etc).

**Design steps:** circuit design, layout design, characterization (timing, noise, power, .libs, functions, etc).
- GUNA software for characterization
- Timing characterization is based on threshold definitions, propagation delays and transition times.

**Outputs:** CDL, GDSII, LEF, extracted SPICE netlist (.cir)

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

## Day 3

### SPICE Simulations (pre-layout)
- SPICE deck / netlisting
- ngspice intro with simulation commands (source, run, setplot, display, plot)
- Static behaviour evaluation (example of CMOS INV robustness)
 1. Switching thereshold (Vm)
- Dynamic behaviour evaluation
 2. Propagation delay (rise and fall)
 
### CMOS Fabrication Process

A 16-mask CMOS process is used as example to show how all layout regions are related to the fabrication process.

### LAB:

When Pin Arrangement is set as 1

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/when%20pin%20arrangement%20is%20set%20as%201.PNG)

When Pin Arrangement is set as 2

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/when%20pin%20arrangement%20is%20set%20as%202.PNG)

After git cloning vsdstdcelldesign:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/after%20git%20cloning.PNG)

Any one can git clone this by using the git link: https://github.com/nickson-jose/vsdstdcelldesign.git

Opening the inverter layout in magic:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/Inverter%20layout.PNG)

We can use grid on command to enable the grid

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/grid%20on.PNG)

Selecting nmos

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/selecting%20nmos.PNG)

Selecting pmos

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/selecting%20pmos.PNG)

Source of nmos connected to ground

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/source%20of%20nmos%20connected%20to%20ground.PNG)

Source of pmos connected to VDD

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/source%20of%20pmos%20connected%20to%20vdd.PNG)

Drain of pmos connected to drain of nmos

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/drain%20of%20pmos%20connected%20to%20drain%20of%20nmos.PNG)

When poly crosses n-diffusion, it is nmos...

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/when%20poly%20crosses%20n-%20diffusion%20it's%20nmos.PNG)

When poly crosses p-diffusion, it is pmos...

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/when%20poly%20crosses%20p-%20diffusion%20it's%20pmos.PNG)

Spice Extraction:

command: extract all

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/for%20extracting%20write%20extract%20all(step%201).PNG)

sky_130_inv.ext file created after extraction

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/sky_130_inv.ext%20file%20created%20after%20extraction(step%201).PNG)

Extracting all the parasitic capacitances and resistances

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/command%20to%20extract%20all%20the%20parasitic%20capacitances%20and%20resistances(step%202).PNG)

Generating spice file from extraction file

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/generating%20spice%20file%20form%20extraction%20file(step%203).PNG)

Spice file created...

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/spice%20file%20created(step%203).PNG)

Measuring the grid

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/measuring%20the%20grid.PNG)

Dimension of the box is 0.01x0.01

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/dimension%20of%20the%20box%20is%200.01%20x%200.01.PNG)

So, we change the scale in the spice file to 0.01u

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/changing%20the%20scale%20to%200.01u.PNG)

Include the model files

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/including%20the%20model%20files.PNG)

Changing the output load

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/changing%20the%20output%20load.PNG)

Add VDD, VSS and other voltage sources

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/command%20for%20vdd%20vss%20and%20other%20voltage%20source.PNG)

Command to run spice in ngspice

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/command%20to%20run%20spice%20in%20ngspice.PNG)

Spice Plot

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/spice%20plot.PNG)

Expanded plot for finding the rise cell delay

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/expanded%20plot%20for%20finding%20the%20rise%20cell%20delay.PNG)

Points for finding the rise cell delay

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/points%20for%20finding%20rise%20cell%20delay.PNG)

Rise cell delay calculated

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/rise%20cell%20delay.PNG)

Calculating the slew:

20% of VDD

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/time%20at%200.66%20of%20vdd.PNG)

80% of VDD

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/time%20at%202.64%20of%20vdd.PNG)

Slew is .....

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%203/slew.PNG)

## Day 4

Day 4 is focused on Static timing analysis and some concepts of clock tree synthesis. 

First timing analysis is done using delay tables, then using ideal clocks, then using real clocks.

### Timing Modeling

Timing modeling is performed with the use of delay tables. Depending upon the input slew and output load we can get the delay for the buffer or any other gate.

In order to model timing for clock tree structures, is necessary to take care with:
- at every level, each node drive same load;
- identical buffers need to be used at same level.

### Timing analysis (with ideal clock)

If T is the clock period and td is the combinational logic delay, then T>td. 

If the setup time for the capture flipflop is S, the T > td + S. Otherwise there will be setup time violation.

If the jitter is considered then T > td + S + SU. Otherwise there will be violation.

Jitter is a temporary timing problem which can be removed if the semiconductor temperature and power noise is maintained correctly.

### Clock Tree Synthesis (CTS)

The main reason for preforming CTS is to remove clock skew. Ideally it should be 0.

Some techniques are used to achieve a good CTS
- H-Tree technique (midpoints to derive clock)
- Using buffers (since H-Tree do not avoid long paths, we need to put buffers)
- Net shielding (to avoid crosstalk/glitches)

### Timing analysis (with real clock)

For timing analysis with real clocks we will consider all the delays, even the clock delays.

### LAB:

Grid:

Press 'g' to activate the grid. And to deactivate press 'g' again. The arguments required by grid is x spacing y spacing x origin and y origin. We need to converge the grid definitions to track definitions.The ports are at intersection of horizontal and vertical tracks ensures that the routing can reach the ports from x as well as y direction. Width and height of the standard cells must be odd multiples of x pitch of that layer.

command: Help grid

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/help%20grid.PNG)

The black square shows grid activated

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/the%20black%20square%20shows%20grid%20activated.PNG)

This is how the track files look like

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/this%20is%20how%20the%20track%20files%20look%20like.PNG)

Grid command to converge with track definitons:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/grid%20command%20to%20converge%20with%20track%20definitons.PNG)

Clearly the inputs and outputs ports are at the intersection of horizontal and vertical tracks.

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/clearly%20the%20inputs%20and%20outputs%20ports%20are%20at%20the%20intersection%20of%20horizontal%20and%20vertical%20tracks.PNG)

Defining the ports

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/defining%20the%20ports.PNG)

Command to create the lef file with the same name as that of the mag file:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/command%20to%20create%20the%20lef%20file%20with%20the%20same%20name%20as%20that%20of%20the%20mag%20file.PNG)

Lef file created

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/lef%20file%20created.PNG)

Libraries:

Slow library

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/slow%20library(means%20mos%20are%20of%20max%20delays).PNG)

Fast library

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/fast%20library.PNG)

Typical library

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/typical%20library.PNG)

copying the library to the source in design

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/copying%20the%20library%20to%20the%20source%20in%20design.PNG)

Set these after preparation is complete

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/set%20these%20after%20preparaion%20is%20complete.PNG)

Running synthesis again

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/synthesis%20is%20successful%20but%20there%20is%20a%20huge%20slack%20violation%20that%20need%20to%20be%20corrected.PNG)

Synthesis is successful but there is a huge slack violation. Initially the area delay and slack is ....

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/initially%20the%20area%20delay%20and%20slack.PNG)

Changing some parameters to reduce slack:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/changes%20in%20parameters%20to%20reduce%20slack.PNG)

Current optimized parameters

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/current%20optimized%20parameters.PNG)

changes that we need to make inside the config.tcl

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/changes%20that%20we%20need%20to%20make%20inside%20the%20config.tcl.PNG)

Parameters are changed and then synthesis run again.

Now the reduced slack is...

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/reduced%20slack.PNG)

But the area is increased

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/area%20of%20the%20chip%20is%20increased.PNG)

Now we run the placement

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/placement%20passed.PNG)

sky130_vsdinv present inside the merged.lef(Gives us confidence that our cell will be there in placement)

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/sky130_vsdinv%20present%20inside%20the%20merged.lef(Gives%20us%20confidence%20that%20our%20cell%20will%20be%20there%20in%20placement).PNG)

pin A declared inside lef file:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/pin%20A%20declared%20inside%20lef%20file.PNG)

pin Y declared inside lef file:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/pin%20Y%20declared%20inside%20lef%20file.PNG)

pin VPWR declared inside lef file:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/pin%20VPWR%20declared%20inside%20lef%20file.PNG)

pin VGND declared inside lef file:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/pin%20VGND%20declared%20inside%20lef%20file.PNG)

Our instance created in the placement

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/our%20instance%20created%20in%20the%20placment.PNG)

After using the expand command we can see the power rails

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/after%20using%20the%20expand%20command(the%20portion%20in%20pink%20are%20the%20power%20rail).PNG)

Labs for basic sta:

Here we run cts.

run_cts completed..

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/run_cts%20completed.PNG)

After cts we have two files inside synthesis.

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/after%20cts%20we%20have%20two%20files%20inside%20synthesis.PNG)

Checking parameters after cts..

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/checking%20parameters%20after%20cts.PNG)

Next we run sta using opensta.

slack after sta for the first time:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/slack%20after%20sta%20for%20the%20first%20time.PNG)

setting fan_out as 4

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/setting%20fan_out%20as%204.PNG)

slack reduced after changing fan_out

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/slack%20reduced%20after%20changing%20fan_out.PNG)

output of sta as fan_out changes

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/output%20of%20sta%20as%20fan_out%20changes.PNG)

setup slack increases after cts

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/setup%20slack%20increases%20after%20cts.PNG)

hold slack increases after cts

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/hold%20slack%20increases%20after%20cts.PNG)

Initially the delay was 0.72

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/initially%20the%20delay%20was%200.72.PNG)

upsizing 44322

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/upsizing%2044322.PNG)

upsizing the highlighted cell

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/usizing%20the%20highlighted%20cell.PNG)

upsizing cell 41882

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/upsizing%20cell%2041882.PNG)

The delay has decreased after upsizing

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/the%20delay%20has%20decreased%20after%20upsizing.PNG)

slack reduced further

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/slack%20reduced%20further.PNG)

clearly the area has increased after modifications due to upsizing

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/clearly%20the%20area%20has%20increased%20after%20modifications%20due%20to%20upsizing.PNG)

upsizing 47972

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/upsizing%2047972.PNG)

slack reduced on replacing cell 47972(upsizing)

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/slack%20reduced%20on%20replacing%20cell%2047972(upsizing).PNG)

upsizing 33697

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/upsizing%2033697.PNG)

slack reduced on replacing cell  33697(upsizing)

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/slack%20reduced%20on%20replacing%20cell%20%2033697(upsizing).PNG)

tns and wns

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/tns%20and%20wns.PNG)

Openroad can be opened inside Openlane:

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/openroad%20can%20be%20opened%20inside%20openlane.PNG)

Creating db

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/creating%20db.PNG)

db created

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/db%20created.PNG)

read db

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/read%20db.PNG)

reading lef

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/reading%20lef.PNG)

reading def

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/reading%20def.PNG)

read max delay library

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/read%20max%20delay.PNG)

read min delay library

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/read%20min%20delay%20lib.PNG)

read verilog

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/read%20verilog.PNG)

read sdc

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/read%20sdc.PNG)

set propagated clock

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/set%20propagated%20clock.PNG)

After this we do a report check. So finally the slack is...

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/Labs%20for%20basic%20sta/finally%20slack%20in%20the%20last%20sta%20video%20before%20cts.PNG)

Now we repeat the steps till the slack becomes positive.

current setup slack

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/current%20setup%20slack.PNG)

current hold slack

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/current%20hold%20slack.PNG)

currently our buffer list contains all the buffers

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/currently%20our%20buffer%20list%20contains%20all.PNG)

removing buf 1

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/removing%20buf%201.PNG)

Now we run cts again

cts step hung with lots of warnings

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/cts%20step%20hung%20with%20lots%20of%20warnings.PNG)

the current def is incorrect(we have to use def value post placement)

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/the%20current%20def%20is%20incorrect(we%20have%20to%20use%20def%20value%20post%20placement).PNG)

killing the cts step of openroad

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/killing%20the%20cts%20step%20of%20openroad.PNG)

confirmation of a succesfull kill

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/confirmation%20of%20a%20succesfull%20kill.PNG)

changing the current def

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/extras/changing%20the%20current%20def.PNG)

Inside openroad:

1.read lef

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/1.read%20lef.PNG)

2.read def

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/2.read%20def.PNG)

3.wiriting db again

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/3.wiriting%20db%20again.PNG)

4.read db

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/4.read%20db.PNG)

5.read verilog

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/5.read%20verilog.PNG)

6.reading library

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/5.read%20verilog.PNG)

7.link the design

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/7.link%20the%20design.PNG)

8.read sdc file

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/8.read%20sdc%20file.PNG)

9.set the propagated clocks

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/9.set%20the%20propagated%20clocks.PNG)

10.report check

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/10.report%20check.PNG)

11.setup slack

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/11.setup%20slack.PNG)

12.hold slack

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/12.hold%20slack.PNG)

13.hold clock skew

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/13.hold%20clock%20skew.PNG)

14.setup clock skew

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/14.setup%20clock%20skew.PNG)

if we insert buf 1 again.....

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/if%20we%20insert%20buf%201%20again.PNG)

Assignment:

For the improved netlist*, what’s the hold slack with default buffer list? (* improved netlist -> Netlist with post synthesis setup slack = -0.12)

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/hold%20slack.PNG)

This answer is closest to 0.4247

For the improved netlist*, what’s the hold clock skew with default buffer list?

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/hold%20clock%20skew.PNG)

For the improved netlist*, what’s the setup slack with default buffer list?

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/setup%20slack.PNG)

This answer is the closest to 4.449

For the improved netlist*, what’s the setup clock skew with default buffer list?

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%204/setup%20clock%20skew.PNG)

## Day 5

Final workshop day is focused on routing and other steps for final SoC Design.

### Routing
Explanation of Maze routing - Lee´s Algorithm
- It creates a routing grid
- It finds the best route from a source to a target
- It is an automated process

Optical Photolithography is used to build wires. So, we need light for this. 

### DRC
- DRC-Design Rule Check
- If there is a drc error we have to do drc clean
- Some critical drc checking points are signal short, via spacing, via width, wire width, wire pitch, wire spacing
- To remove signal short we usually use different metals where there is a short. Usually the above wire is wider than the below.

### Parasitics Extraction
- SPEF: Standard Parasitics Exchange Format
- IEEE 1481-1999

### LAB:

Generating power distribution network:

command: gen_pdn

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/generating%20pdn.PNG)

After running pdn

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/after%20pdn%20run.PNG)

Running routing

Routing strategy is 0 for us. It can go upto 14. More the routing strategy lesser will be the number of violations.

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/routing%20strategy%20is%200.PNG)

command: run_routing

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/run%20routing.PNG)

Routing completed

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/routing%20completed.PNG)

It takes around 25-30 minutes to complete routing.

DRC Violations after routing

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/drc%20violations.PNG)

Number of violations

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/number%20of%20violations.PNG)

SPEF Extraction:

SPEF Extractor is present inside tools

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/SPEF%20extractor%20inside%20tools.PNG)

Command to create spef file

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/command%20to%20create%20spef%20file.PNG)

spef generation completed

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/spef%20generation%20completed.PNG)

spef file present inside routing folder

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/spef%20file%20present%20inside%20routing%20folder.PNG)

All synthesized verilog files

![](https://github.com/SaiSwaroopMishra/Advance-Physical-Design-Workshop-/blob/main/Images/Day%205/all%20synthesized%20verilog%20files.PNG)

### Sign-off post-route STA

For post-route STA we need to create a new .db with the new .def (from post-route) using the verilog file from pre-route. Use the same .sdc and add a new command to read .spef file.

## Acknowledgement

- Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)
- Nickson Jose (VSD Corp. Pvt. Ltd Intern)
















