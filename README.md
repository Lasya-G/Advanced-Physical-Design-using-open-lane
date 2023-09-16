# Advanced-Physical-Design-using-open-lane

### Table of Contents
- [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
- [Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
- [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
- [Day 4 - Pre-layout timing analysis and importance of good clock tree](day-4---pre-layout-timing-analysis-and-importance-of-good-clock-tree)
- [Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA](#day-5---final-steps-for-rtl2gds-using-tritonroute-and-opensta)
- [References](#references)

### Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
<details>
<summary>
How to talk to computers
</summary>    

The typical block diagram of a Arduino Microcontroller chip is shown here:  
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/553e99e1-a364-4c64-9e1a-5d61875c29c5">  

The package QFN-48 is shown below:  
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/609997e6-bf91-4950-8418-a2e7846337fe">  

The interface of the chip with package and the pads, core, die is shown here:
<img width="500" alt="Screenshot from 2023-09-05 19-16-57" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/2ef42a7a-2c0d-4cd1-b7bb-3a44c9f6ac1a"> <img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/cf5a139e-e506-45a7-96ee-23751897edb5">  
PADS: They are the medium through which the signals are sent to the chip and vice-versa.

- Inorder for a program to run on the procssor, it first needs to get converted into an Assembly language which which finaaly gets converted into machine level language i.e; Binary.  
- The COMPILER converts the High-level language to Assembly level and the ASSEMBLER converts the Assembly level language into the Binary format.  



</details>

<details>
<summary>
SoC Design and openLAN
</summary>

The design of ASIC requires 3 main elements:  
<img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/a1d87924-47f5-4b99-b224-63a885a06366">  

The simplified ASIC design flow is shown below:  
<img width="450" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/1ff9041d-9dfe-4a04-b814-d8c18dd0c583">  

- **Synthesis**: This converts RTL into a circuit using the components from the standard cell library. The resultant circuit is described in HDL and is usually referred as gate-level netlist, which is a functional equivalent of RTL. Each cell has a different view depending on the tool used.
<img width="450" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/76df67ee-686b-438c-bd43-10d7ed8d6cd7">

- **Floor and Power planning**: The objective is to plan the silicon area and robust power distribution network to power the circuits.
  - Chip-Floor Planning: Partition the chip die between different system building blocks and place the I/O pads.
  - Macro-Floor Planning: We define the macro dimensions and its pin locations. We also define row definitions which is used in placement process.
  - Power PLanning : It is the process of managing and distributing electrical power within an IC to ensure proper functionality, performance, and reliability while minimizing power consumption.
<img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/1ce74b69-b92e-4275-8f02-57eb9ccae251">
<img width="200" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/1438aeca-4f1f-4396-86f0-6b965a85fcbc">
<img width="200" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/770361da-3faf-4993-8bd4-ff692b42e75b">

- **Placement**: Place the cells on the floorplan rows aligned with the sites. It is usually done in 2 steps:
  - Global placement : Finds the optimal positions for all cells, which can involve cell overlapping
  - Detailed placement : Positions are minimally altered to their fixed positions.
<img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/e3977563-aae6-4fe4-a0a8-45bec22d3799">


- **Clock Tree Synthesis**: It is used to create a clock distribution network inorder to deliver clock to all sequential elements with minimum skew and minimum latency, and in a good shape. It usually looks like a tree.
<img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/56fedd15-636f-4ceb-8cca-9699a8980766">

- **Routing**: Implement the interconnect using the available metal layers. These metal layers tracks form a routing grid. As routing grid is huge, divide and conquer approach is used for routing. First, Global routing generates the routing guides and then the Detailed routing uses the guide to implement actual wiring.

- **Sign Off**: It undergoes **Physical Verification** which includes Design Rules Checking and Layout vs Schematic, and **Timing Verification** which includes Static Timing Analysis.

<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/7b760c4f-2dd0-40c1-9533-dbb1b13737f8">  

**OpenLane**

- It started as an Open-source flow for a true Open source tape-out experiment.
- Strive is a family of open everything SoCs. <img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/6bbdf602-9033-4594-ab31-5eafb0b70018">
- The main goal of OpenLane is to produce a clean GDSII with no human intervention.
- It is tuned for Skywater 130nm Open PDK, also supports XFAB180 and GF130G.
- It has 2 modes of operation: Autonomous and Interactive.

**OpenLane ASIC Flow**:  
<img width="700" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/f02bc1db-eee4-4c52-aa64-98c0a7577b01">   

</details>

<details>
<summary>
Get familiar to open-source EDA tools
</summary>

Follow the below steps for installation of OpenLane:  

```
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
cd OpenLane
make
make test
cd /home/ASIC/OpenLane/designs/ci
cp -r * ../
```
Use the following commands to invoke OpenLane and run synthesis:  

```
cd ASIC/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```

<img width="700" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/bbe46115-0395-47a2-abce-93d9a9c80714">  

To view the netlist, use the following commands:  
```
cd designs/picorv32a/runs/RUN_2023.09.12_06.50.19//results/synthesis/
gedit picorv32.v
```

<img width="700" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/51d96007-594e-4b3f-9bca-d07bc582eb30">  

Synthesis report can be seen by using these:
  ```
cd designs/picorv32a/runs/RUN_2023.09.12_06.50.19//results/synthesis/
gedit 1-synthesis.AREA_0.stat.rpt 
```

<img width="700" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/a40f673b-6dfd-4ba6-9e69-c1814e013057">  

Flop ratio = Number of flops/Total number of cells = 1596/10104 = 0.1579 

</details>  

### Day 2 - Good floorplan vs bad floorplan and introduction to library cells
<details>
<summary>
Chip Floor Planning considerations
</summary>

There are 2 important factors to consider in Floorplanning: **Utilization Factor** and **Aspect Ratio**. They are defined as follows:
```
Utilisation Factor =  Area occupied by netlist
                     __________________________
                        Total area of core
```
When Utilization factor is 1, it means the core is completely utilised and there is no space for extra logic. So, we typically maintain the utilization factor as 0.5 or 0.6.  

```

Aspect Ratio =  Height
               ________
                Width

```
The Aspect ratio of 1 implies that the chip is square shaped. Any value other than 1 implies rectanglular chip.  

<i> **Pre-Placed Cells** </i> : They refer to specific logic cells or standard cells that are manually or algorithmically placed in predefined positions on the chip's layout before the automated placement and routing tools are applied to place and connect the rest of the logic cells. The locations of these pre-placement cells should be well defined because once placed, they cannot be altered. Pre-placement cells must always be surrounded by de-coupling capacitors.  

<i> **De-Coupling Capacitors** </i> : When we connect the circuit with wires, there will be some voltage drop as every physical thing has some resistance. This voltage supplied after the drop must always be in the safe range of noise margin. De-coupling capacitors are huge capacitors charged to power supply voltage and placed close the logic circuit. Their role is to decouple the circuit from power supply by supplying the necessary amount of current to the circuit. They pervent crosstalk and enable local communication.  

<img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/9c866db4-70a9-46b1-a5ca-d7bae4870c0b">  

<i> **Power Planning** </i> : 
- Ground Bump is a transient effect that can occur during the operation of the circuit where the voltage level of the ground (GND) signal temporarily rises or "bounces" above its reference voltage due to the switching of digital logic gates or other high-current activities. This condition arises when several blocks or cells try to dissipate power at the same time.
- Voltage droop, also known as voltage sag or voltage drop, refers to a temporary reduction in the power supply voltage at a specific point on the chip when a high current demand occurs. This condition arises when several blocks or cells try to draw power at the same time.

When this ground bump or voltage droop violates the noise margin range, then the desired output is not achievable. Inorder to avoid this, we place ground and power ports in horizontal and vertical positions so that these blokcs draw power or dissipate then to nearest associated port.  
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/948ce2d8-5b19-4f80-8c1e-50727e6be2af">  

<i> **Pin Placemnet** </i> : The connectivity information between the gates is coded using Verilog or VHDL language and is defined as the "Netlist". The Pin infromation is stored in between the Die and Core. The ordering of these pins is random as it depends on the placement of the cells. The clock cells is always bigger in size because it needs to drive most of the design blocks and must possess lease resistance.  
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/b82518b2-3968-4974-9785-7a1968b310b6">  

<i> **Floor Planning** </i>  

Floorplan of picorv32a is done using the below command:  
```
run_floorplan
```

Post the floorplan run, a .def file will have been created within the "results/floorplan" directory. We may review floorplan files by checking the "floorplan.tcl". The system defaults will have been overriden by switches set in "conifg.tcl" and further overriden by switches set in "sky130A_sky130_fd_sc_hd_config.tcl".  

To view the floorplan, we invoke magic.
```
cd ASIC/OPenLane/designs/picorv32a/runs/RUN_2023_09.16_05.35.04/results/floorplan
magic -T home/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```

<img width="700" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/83f02543-8742-4ef6-914c-bb0bdff2672d">  

</details>

<details>
<summary>
Library binding and placement
</summary>

<i> **Netlist Boinding and initial place design** </i> : First we need to bind the netlist with physical cells. We have shapes for OR, AND and every cell for pratice purpose. But in reality we dont have such shapes, we have give an physical dimensions like rectangles or squares weight and width. This information is given in libs and lefs. Now we place these cells in our design by initilaising it.  
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/ab168232-ad2b-42b9-8329-da28a22e3ff7">  

<i> **Placement Optimization** </i> : The next step is placement. Once we initial the design, the logic cells in netlist in its physical dimisoins is placed on the floorplan. Placement is perfomed in 2 stages:

- Global Placement: Cells will be placed randomly in optimal positions which may not be legal and cells may overlap. Optimization is done through reduction of half parameter wire length.
- Detailed Placement: It alters the position of cells post global placement so as to legalise them. Legalisation of cells is important from timing point of view.

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/d31aa82f-df6c-4c26-8da3-5c1319b75136">  

Optimization is stage where we estimate the length and capictance, based on that we add buffers. Ideally, Optimization is done for better timing.  

<i> **Congestion aware placement using RePlAce** </i> :  
Use the below command to run placement
```
run_placement
```

<img wudth="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/9b9947f0-3404-46d5-9c2e-79f4e1834c65">  

</details>

<details>
<summary>
Cell design and characterization flow
</summary>

<i> **Inputs for cell design flow** </i>  : Library is a place where we get information about every cell. It has differents cells with different size, functionality,threshold voltages. There is a typical cell design flow steps.
- Inputs : PDKS(process design kit) : DRC & LVS, SPICE Models, library & user-defined specs.
- Design Steps :Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power).
- Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files

<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/feb4c3dd-7c7c-4613-9067-cc1381341540">  

<i> **Standard Cell Characterization Flow** </i> : Characterization refers to the process of gathering and analyzing electrical and performance data for a specific cell or library element. The goal of characterization is to provide accurate and comprehensive information about how the cell behaves under various operating conditions. This information is essential for designing and optimizing digital circuits using these cells.  
A typical standard cell characterization flow includes the following steps:  
- Read in the models and tech files
- Read extracted spice netlist
- Recognise behaviour of the cell
- Read the subcircuits
- Attach power sources
- Apply stimulus to characterization setup
- Provide necessary output capacitance loads
- Provide necessary simulation commands he opensource software called GUNA can be used for characterization. Steps 1-8 are fed into the GUNA software which generates timing, noise and power models.

Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing characterization, power characterization and noise characterization.  

<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/49271f4d-884e-46c1-8ee3-7e57659b5997">  

</details> 

<details>
<summary>
General timing characterization parameters 
</summary> 
  
<i> **Timing threshold definition** </i> :

Timing Definition | Value
------------ | -------------
slew_low_rise_thr  | 20% value
slew_high_rise_thr |  80% value
slew_low_fall_thr | 20% value
slew_high_fall_thr | 80% value
in_rise_thr | 50% value
in_fall_thr | 50% value
out_rise_thr | 50% value
out_fall_thr | 50% value    

<i> **Propagation delay and Tranistion time** </i> :   

- Propagation Delay: Propagation delay refers to the time it takes for a change in an input signal to reach 50% of its final value to produce a corresponding change in the output signal to reach 50% of its final value of a digital circuit.
```
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```

- Transition Time : The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.
```
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)

Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```

A poor choice of threshold points leads to neative delay value. Therefore a correct choice of thresholds is very important.  

</details>

### Day 3 - Design library cell using Magic Layout and ngspice characterization
<details>
<summary>
Labs for CMOS inverter ngspice simulation
</summary>

<i> **I/O Placer Revision** </i> :  PnR is a iterative flow and hence, we can make changes to the environment variables in the fly to observe the changes in our design. Let us say If I want to change my pin configuration along the core from equvi distance randomly placed to someother placement, we just set that IO mode variable on command prompt as shown below:  
```
set ::env(FP_IO_MODE) 2
```

<i> **Spice deck creation & Simulation for CMOS Inverter** </i> :  
Before performing SPICE simulation, we have to create a SPICE Deck that contains the information about the following:
- Component connectivity - how the components are connected
- Component values - values of each component present in the circuit
- Nodes - number of nodes and the elements connected between the nodes
- Simulation type and parameters - type of simulation to be performed, say operating point, AC analysis or DC Analysis etc
- Capacitance load - value of the capacitance connected at the load
- Model description - model files that should be included in the simulation
- Netlist description

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/870c74b3-b2fa-4af5-acef-a85781a5c5d4">   
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/1ae8949d-ab0c-4774-96b8-28e94a5614cf">  

<i> **Switching threshold** </i> : The switching threshold of a CMOS inverter is the point on the transfer characteristics where Vin equals Vout(=Vm). At this point, both PMOS and NMOS are in ON state which gives rise to a leakage current.  

<i> **Steps to gitclone vsdstdcelldesign** </i> : The Magic layout of a CMOS inverter will be used so as to intergate the inverter with the picorv32a design. To do this, inverter magic file is sourced from vsdstdcelldesign by cloning it within the <i> home/OpenLane </i> directory as follows:
```
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
This creates a vsdstdcelldesign named folder in the openlane directory. Now, we can view the layout of inverter in magic using the below command:  
```
magic -T libs/sky130A.tech sky130_inv.mag &
```
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/10f0c189-fb59-41bd-81b3-77635d335deb">  

</details>
<details>
<summary>
Inception of Layout A CMOS fabrication process
</summary>

<i> **16 Mask CMOS process** </i> :  
The 16-mask CMOS process consists of the following steps:
1. Selection of subtrate: Secting the body/substrate material. <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/498cf18e-c26d-4d56-85e4-eebb95c9926a"> 
2. Creating active region for transistors: Isolation between active region pockets by SiO2 and Si3N4 deposition followed by photolithography and etching. <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/c3658e64-e1b2-4c4b-a15b-2e88f066b362">  
3. N-well and P-well formation: Ion implanation by Boron for P-well and by Phosphorous for N-well formation. <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/cdfe9703-6187-40a5-85cb-c737759e9a1e">  
4. Formation of gate terminal: NMOS and PMOS gates formed by photolithography techniques. <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/05e4dcf3-665e-41b3-b268-08f2b7ace7b9">
5. LDD (lightly doped drain) formation: LDD formed to prevent hot electron effect. <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/84af76f3-696d-4c6b-b1b0-8fbe6e89cca0">
6. Source & drain formation: Screen oxide added to avoid channelling during implants followed by Aresenic implantation and annealing.  <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/1e1df456-0e5b-4004-8237-87f144ee0384">
7. Local interconnect formation: Removal of screen oxide by HF etching. Deposition of Ti for low resistant contacts.  <img width="300" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/8ad5c234-8df4-4de6-9b1e-40b7203ed78f">  
8. Higher level metal formation: CMP for planarization followed by TiN and Tungsten deposition. Top SiN layer for chip protection.
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/a8e3ec3e-9dc4-4426-888a-a85f3a5b68d2">

The 16 masks used in the above process are:  

- *Substrate Mask (Mask 1):* This mask defines the active regions on the silicon wafer where transistors and other devices will be formed. It specifies the boundaries of the N-well and P-well regions.
- *Threshold Voltage Adjustment Mask (Mask 2):* This mask adjusts the threshold voltage of the transistors by defining the regions where threshold voltage implants are required.
- *Gate Oxide Mask (Mask 3):* This mask defines the areas where gate oxide will be grown or deposited. The gate oxide acts as an insulator between the gate electrode and the silicon substrate.
- *Poly-Silicon Gate Mask (Mask 4):* This mask defines the gate electrodes for both N-channel and P-channel transistors. It outlines the shape of the gates.
- *N+ and P+ Diffusion Masks (Masks 5 and 6):* These masks define the source and drain regions for the N-channel and P-channel transistors, respectively. These regions are typically doped with impurities to create the necessary electrical characteristics.
- *Contact Mask (Mask 7):* This mask defines the openings for contacts, which allow the metal layers to connect to the underlying silicon.
- *First Metal Layer Mask (Mask 8):* This mask defines the first layer of metal interconnects that connect various components on the chip, such as transistors and contacts.
- *Interlayer Dielectric (ILD) Mask (Mask 9):* This mask defines the dielectric material that insulates metal layers from each other. It also specifies the locations of vias for vertical connections.
- *Via Mask (Mask 10):* This mask defines the openings in the ILD layer for vias, which enable vertical connections between metal layers.
- *Second Metal Layer Mask (Mask 11):* This mask defines the second layer of metal interconnects, which connect to the underlying metal layer and vias.
- *Barrier Layer Mask (Mask 12):* This mask defines layers used to improve adhesion between metal and dielectric, enhancing the reliability of the interconnects.
- *Third Metal Layer Mask (Mask 13):* This mask defines the third layer of metal interconnects, which can connect to the lower metal layers through vias.
- *Passivation Layer Mask (Mask 14):* This mask defines the protective passivation layer that covers the entire chip, protecting it from external factors and contamination.
- *Bond Pad Mask (Mask 15):* This mask defines the locations of bond pads, which are used for external electrical connections and testing.
- *Test Structure Mask (Mask 16):* This mask includes various test structures used for quality control, testing, and characterization during manufacturing.

</details>

<details>
<summary>
Sky130 Tech files Lab
</summary>


<i> **Spice Extraction** </i> : Use the below commands in tkcon to achieve .mag to .spice extraction:  
```
extract all
ext2spice cthresh 0 rethresh 0
ext2spice
```
ext2spice commands converts the ext file to spice netlist. cthreh and rthresh are the switches to extract all the parasitic resistance and capacitance. The extracted spice list has to be modified as shown below to use ngspice to perform simulation:
```
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/7c203860-da34-45ef-8c01-d72d22fa5df7">

Use the following command to simulate spice netlist and plot the waveform:
```
ngspice sky130_inv.spice
plot y vs time a
```
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/0165717e-8401-4b2a-b939-9de2db8daa1e">

The spikes in the output at switching points is due to low capacitance loads. This can be taken care of by editing the spice deck to increase the load capacitance value.

<i> **Inverter Standard cell Characterization** </i> :
Four timing parameters are used to characterize the inverter standard cell:

- Rise transition: Time taken for the output to rise from 20% of max value to 80% of max value.
- Fall transition- Time taken for the output to fall from 80% of max value to 20% of max value.
- Cell rise delay = time(50% output rise) - time(50% input fall)
- Cell fall delay = time(50% output fall) - time(50% input rise)

The above timing parameters can be computed by noting down various values from the ngspice waveform:
```
Rise transition = (2.23843 - 2.17935) = 59.08ps

Fall transition = (4.09291 - 4.05004) = 42.87ps

Cell rise delay = (2.20636 - 2.15) = 56.36ps

Cell fall delay = (4.07479 - 4.05) = 24.79ps
```


</details>

### Day 4 - Pre-layout timing analysis and importance of good clock tree
<details>
<summary>
Timing modelling using delay tables
</summary>
</details>
<details>
<summary>
Timing analysis with ideal clocks using openSTA
</summary>
</details>
<details>
<summary>
Clock tree synthesis TritonCTS and signal integrity
</summary>
</details>
<details>
<summary>
Timing analysis with real clocks using openSTA
</summary>
</details>

### Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA
<details>
<summary>
Routing and design rule check(DRC)
</summary>
</details>
<details>
<summary>
Power distribution network and routing
</summary>
</details>
<details>
<summary>
Triron route features
</summary>
</details>

### References
1. https://www.vsdiat.com
2. https://github.com/Devipriya1921/Physical_Design_Using_OpenLANE_Sky130
3. https://github.com/Rachana-Kaparthi/ADVANCED-PHYSICAL-DESIGN-USING-OPENLANE-SKY130
4. https://chat.openai.com
5. https://github.com/alwinshaju08/Physicaldesign_openlane

