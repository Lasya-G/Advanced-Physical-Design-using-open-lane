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

<i> **Magic and DRC Rules** </i> :  
The technology file is a setup file that declares layer types, colors, patterns, electrical connectivity, DRC, device extraction rules and rules to read LEF and DEF files. Magic layouts can be sourced from ```opencircuitdesign.com``` using the command:  
```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
```

```
cd drc_tests
magic -d XR met3.mag
```

To analyse DRC errors, magic is invoked and the met3.mag file is opened either from the software as ```file-> open-> met3.mag``` or by running command in tkcon as ```magic -d XR met3```.
DRC errors can be found by selecting a component and typing: ```drc why``` in tkcon.  

The descriptions of DRC rules can be found in the [SKY130 PDK’s documentation](https://skywater-pdk.readthedocs.io/en/main/rules/).


</details>  


### Day 4 - Pre-layout timing analysis and importance of good clock tree
<details>
<summary>
Timing modelling using delay tables
</summary>

<i> **Steps to convert Grid info inti Track info** </i>:  
A requirement for ports as specified in ```tracks.info``` is that they should be at intersection of horizontal and vertical tracks. The CMOS Inverter ports A and Y are on li1 layer. It needs to be ensured that they're on the intersection of horizontal and vertical tracks. We access the tracks.info file for the pitch and direction information:  

<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/5eebb2b0-aea1-437c-82bb-8f4ad009a507">  

To ensure that ports lie on the intersection point, the grid spacing in Magic (tkcon) must be changed to the li1 X and li1 Y values. Convergence of grid and tracks can be achieved using the following command:
```
grid 0.46um 0.34um 0.23um 0.17um
```
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/41cd60d2-6db7-42b9-b981-7f1ba1a92fd1">  

<i> **Steps to convert magic layout to std cell LEF** </i> :  
Next step is extracting LEF file for the cell. However, certain properties and definitions need to be set to the pins of the cell which aid the placer and router tool. For LEF files, a cell that contains ports is written as a macro cell, and the ports are the declared PINs of the macro. Our objective is to extract LEF from a given layout (here of a simple CMOS inverter) in standard format. Defining port and setting correct class and use attributes to each port is the first step. Ports of the layout are the pins of lef file.
- Select port->Edit->text and make the following changes:
<img width="500" alt="Screenshot from 2023-09-16 14-36-42" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/587e87a8-bae9-4118-ac36-ef7b4db246f8">
The same procedure is followed for Y, VPWR, VGND:
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/809901b7-d9bd-428e-b9cb-835c593be6a0">
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/9810cfc9-2cd1-4347-bfda-a80563d89d82">
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/64d6e59d-95d5-4031-9172-d68625300d5d">

<i> **Standard cell LEF generation** </i> :
Before the CMOS Inverter standard cell LEF is extracted, the purpose of ports must be defined:
```
//Select A area

port class input
port use signal

//Select Y area

port class output
port use signal

//Select VPWR area

port class inout
port use power

//Select VGND area

port class inout
port use ground
```

LEF extraction is done using the below command:
```
lef write
```
This generates sky130_vsdinv.lef file.  

<i> **Steps to include to custome cell in design** </i> :  
To include custom cell into syntheis:  
- Copy file to picorv32a location.
- In the config.json file, make the following changes:
```

{
    "DESIGN_NAME": "picorv32",
    "VERILOG_FILES": "dir::src/picorv32a.v",
    "CLOCK_PORT": "clk",
    "CLOCK_NET": "clk",
    "GLB_RESIZER_TIMING_OPTIMIZATIONS": true,
    "RUN_HEURISTIC_DIODE_INSERTION": true,
    "DIODE_ON_PORTS": "in",
    "GPL_CELL_PADDING": 2,
    "DPL_CELL_PADDING": 2,
    "CLOCK_PERIOD": 24,
    "FP_CORE_UTIL": 35,
    "PL_RANDOM_GLB_PLACEMENT": 1,
    "PL_TARGET_DENSITY": 0.5,
    "FP_SIZING": "relative",
    "LIB_SYNTH":"dir::src/sky130_fd_sc_hd__typical.lib",
    "LIB_FASTEST":"dir::src/sky130_fd_sc_hd__fast.lib",
    "LIB_SLOWEST":"dir::src/sky130_fd_sc_hd__slow.lib",
    "LIB_TYPICAL":"dir::src/sky130_fd_sc_hd__typical.lib",
    "TEST_EXTERNAL_GLOB":"dir::/src/*",
    "SYNTH_DRIVING_CELL":"sky130_vsdinv",
    "MAX_FANOUT_CONSTRAINT": 4,
    "pdk::sky130*": {
        "MAX_FANOUT_CONSTRAINT": 6,
        "scl::sky130_fd_sc_ms": {
            "FP_CORE_UTIL": 30
        }
    }
}
```

Now, run OpenLane using the following commands:
```
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/bc730d67-71a7-4fb1-a161-4a31d85a6104">  

Now we use the below commands to run floorplan and placement:
```
run_floorplan
run_placement
```

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/749854e4-62cb-48fa-89bb-8544844bfd1b">    



<i> **Dealy Tables** </i> :  
We encounter several types of delays in ASIC design. They are as follows:Gate delay or Intrinsic delay,Net delay or Interconnect delay or Wire delay or Extrinsic delay or Flight time, Transition or Slew,Propagation delay,Contamination delay. Wire delays or extrinsic delays are calculated using output drive strength, input capacitance and wire load models. Other delays are intrinsic properties of each and every gate. Delays are interdependent on different electrical properties.Input capacitance of the logic gate is a function of output state, output loads and input slew rate, Internal timing arcs and output slew rate is a function of switching inputs, Capacitance of the wire is dependent on frequency. Lets say two scenarios, we have long wire and the cell(X1) is sitting at the end of the wire : the delay of this cell will be different because of the bad transition that caused due to the resistance and capcitances on the long wire. we have the same cell sitting at the end of the short wire: the delay of this will be different since the transistion is not that bad comapred. Eventhough both are same cells, depending upon the input tran, the delay got changed. Same goes with o/p load also.  

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/75274263-46c7-45a7-b494-a95286fc040e">  

</details>

<details>
<summary>
Timing analysis with ideal clocks using openSTA
</summary>

<i> **Setup timing Analysis(Flip flop)** </i> :  
Setup time is the required time duration that the input data MUST be stable before the triggering-edge of the clock. If data is changing within this setup time window, the input data might be lost and not stored in the flip-flop as metastability might occur. What is metastability? When setup and hold time requirements are violated, the flip-flop state becomes unstable, and after an unpredictable duration, the state of the flip-flop can settle either way (1 or 0). This scenario is known as metastability. As shown in the following diagram, output Q1 passes through the slow logic and arrives late at the input D2 of FF2, which leads to setup time violation and the loss of the new data. Thus combinational delay must be less than clock frequency - setup time.  

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/62aa2cbb-93d9-4045-9ca4-12c2ca1675ca">  
  

**Clock Jitter**:  This is a characteristic of the clock source and the clock signal environment. It can be defined as “deviation of a clock edge from its ideal location.” Clock jitter is typically caused by clock generator circuitry, noise, power supply variations, interference from nearby circuitry etc. Jitter is a contributing factor to the design margin specified for timing closure.  
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/5ab2d48b-fb60-4cd4-bd5c-72eaf7402bd6">  

<i> **Post-synthesis timing analysis using Openlane** </i> :

Timing analysis is carried out outside the openLANE flow using OpenSTA tool. For this, a new file pre_sta.conf is created. This file would be reqiured to carry out the STA analysis. Invoke OpenSTA outside the openLANE flow as follows:
```
sta pre_sta.conf
```

Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:
- Change synthesis strategy, synthesis buffering and synthesis sizing values.
- Review maximum fanout of cells and replace cells with high fanout.

</details>

<details>
<summary>
Clock tree synthesis TritonCTS and signal integrity
</summary>

The purpose of building a clock tree is enable the clock input to reach every element and to ensure a zero clock skew. H-tree is a common methodology followed in CTS. Before attempting a CTS run in TritonCTS tool, if the slack was attempted to be reduced in previous run, the netlist may have gotten modified by cell replacement techniques. Therefore, the verilog file needs to be modified using the write_verilog command. In this stage clock is propagated and make sure that clock reaches each and every clock pin from clock source with mininimum skew and insertion delay. Inorder to do this, we implement H-tree using mid point strategy.  

- **Balanced Tree CTS**: In a balanced tree CTS, the clock signal is distributed in a balanced manner, often resembling a binary tree structure. This approach aims to provide roughly equal path lengths to all clock sinks (flip-flops) to minimize clock skew. It's relatively straightforward to implement and analyze but may not be the most power-efficient solution.

- **H-tree CTS**: An H-tree CTS uses a hierarchical tree structure, resembling the letter "H." It is particularly effective for distributing clock signals across large chip areas. The hierarchical structure can help reduce clock skew and optimize power consumption.
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/6269d08b-5895-4f04-931c-be311a162db0">

- **Star CTS**: In a star CTS, the clock signal is distributed from a single central point (like a star) to all the flip-flops. This approach simplifies clock distribution and minimizes clock skew but may require a higher number of buffers near the source.

- **Global-Local CTS**: Global-Local CTS is a hybrid approach that combines elements of both star and tree topologies. The global clock tree distributes the clock signal to major clock domains, while local trees within each domain further distribute the clock. This approach balances between global and local optimization, addressing both chip-wide and domain-specific clocking requirements.

- **Mesh CTS**: In a mesh CTS, clock wires are arranged in a mesh-like grid pattern, and each flip-flop is connected to the nearest available clock wire. It is often used in highly regular and structured designs, such as memory arrays. Mesh CTS can offer a balance between simplicity and skew minimization.

- **Adaptive CTS**: Adaptive CTS techniques adjust the clock tree structure dynamically based on the timing and congestion constraints of the design. This approach allows for greater flexibility and adaptability in meeting design goals but may be more complex to implement.

<i> **CrossTalk** </i> :  Crosstalk is a disturbance caused by the electric or magnetic fields of one telecommunication signal affecting a signal in an adjacent circuit. Essentially, every electrical signal has a varying electromagnetic field. Whenever these fields overlap, unwanted signals -- capacitive, conductive or inductive coupling -- cause electromagnetic interference (EMI) that can create crosstalk. Overlap can occur with structured cabling, integrated circuit design, audio electronics and other connectivity systems. For example, if there are two wires in close proximity that are carrying different signals, their currents will create magnetic fields that induce a weaker signal in the neighboring wire. Impact: Crosstalk is a significant concern in VLSI design due to the high integration density of components on a chip. Uncontrolled crosstalk can lead to data corruption, timing violations, and increased power consumption. Mitigation: VLSI designers employ various techniques to mitigate crosstalk, such as optimizing layout and routing, using appropriate shielding, implementing proper clock distribution strategies, and utilizing clock gating to reduce dynamic power consumption when logic is idle.  

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/79620abf-1e88-4fec-8938-1622c3a4a722">  

<i> **Clock Net Shielding** </i> : Shielding is done so as to prevent gltch. Shields are connected to VDD or GND. The shields do not switch.VLSI designers may use shielding techniques to isolate the clock network from other signals, reducing the risk of interference. This can include dedicated clock routing layers, clock tree synthesis algorithms, and buffer insertion to manage clock distribution more effectively. Clock Domain Isolation: VLSI designs often have multiple clock domains. Shielding and proper clock gating help ensure that clock signals do not propagate between domains, avoiding metastability issues and maintaining synchronization.  

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/1a7157d8-6181-4c9c-85e6-3208090e38f8">  

<i> **LAB** </i> :  
The purpose of building a clock tree is enable the clock input to reach every element and to ensure a zero clock skew. H-tree is a common methodology followed in CTS. Before attempting a CTS run in TritonCTS tool, if the slack was attempted to be reduced in previous run, the netlist may have gotten modified by cell replacement techniques. Therefore, the verilog file needs to be modified using the ```write_verilog``` command. Then, the synthesis, floorplan and placement is run again.   
To run CTS use the below command:
```
run_cts
```

After CTS run, my slack values are:
```
setup = 13.45
Hold = 0.16
```
<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/ea432a79-0497-4ab5-8fd9-90286c5e06bf">  

Here, my both values are not voilating as they are positive.  
Since, clock is propagated, from this stage, we do timing analysis with real clocks. From now post cts analysis is performed by operoad within the openlane flow:
```
openroad
read_lef <path of merge.nom.lef>
read_def <path of def>
write_db pico_cts.db
read_db pico_cts.db
read_verilog /home/parallels/OpenLane/designs/picorv32a/runs/RUN_09-09_11-20/results/synthesis/picorv32a.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
read_sdc /home/parallels/OpenLane/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
<img width="600" alt="268354707-f93e8387-ca88-4a88-b809-c8c52436ad85" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/6d0e2dee-4790-4cab-ae50-94856701e26b">  

<img width="600" alt="268354659-fff6a48d-59f0-446e-b6da-f456dcef0dbe" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/27b6c135-e578-4ee7-9625-3a7f5d8b05a0">  

To check all the clock buffers, use these commands in openlane:
```
echo $::env(CTS_CLK_BUFFER_LIST)
set $::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CTS_CLK_BUFFER_LIST)
```

</details>

### Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA
<details>
<summary>
Routing and design rule check(DRC)
</summary>

<i> **Maze Routing and Lee's algorithm** </i>:  
Routing is the process of creating physical connections based on logical connectivity. Signal pins are connected by routing metal interconnects. Routed metal paths must meet timing, clock skew, max trans/cap requirements and also physical DRC requirements. In grid based routing system each metal layer has its own tracks and preferred routing direction which are defined in a unified cell in the standard cell library.  
There are four steps of routing operations:
1. Global routing
2. Track assignment
3. Detail routing
4. Search and repair

The Maze Routing algorithm, such as the Lee algorithm, is one approach for solving routing problems. In this method, a grid similar to the one created during cell customization is utilized for routing purposes. The Lee algorithm starts with two designated points, the source and target, and leverages the routing grid to identify the shortest or optimal route between them. The algorithm assigns labels to neighboring grid cells around the source, incrementing them from 1 until it reaches the target (for instance, from 1 to 7). Various paths may emerge during this process, including L-shaped and zigzag-shaped routes. The Lee algorithm prioritizes selecting the best path, typically favoring L-shaped routes over zigzags. If no L-shaped paths are available, it may resort to zigzag routes. This approach is particularly valuable for global routing tasks. However, the Lee algorithm has limitations. It essentially constructs a maze and then numbers its cells from the source to the target. While effective for routing between two pins, it can be time-consuming when dealing with millions of pins. There are alternative algorithms that address similar routing challenges.  

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/a6c25672-e5f3-462c-bf28-5ddc93d8b1b3">  

<i> **DRC** </i> :  

Design Rule Checking (DRC) verifies as to whether a specific design meets the constraints imposed by the process technology to be used for its manufacturing. DRC checking is an essential part of the physical design flow and ensures the design meets manufacturing requirements and will not result in a chip failure. The process technology rules are provided by process engineers and/or fabrication facility.Each process technology will have its own set of rules. The number of DRC rules and complexity of rules increases as the manufacturing technology shrinks at advanced nodes DRC verifies whether a design meets the predefined process technology rules given by the foundry for its manufacturing. DRC checking is an essential part of the physical design flow and ensures the design meets manufacturing requirements and will not result in a chip failure. It defines the Quality of chip. They are so many DRCs, let us see few of them Design rules for physical wires Minimum width of the wire Minimum spacing between the wires Minimum pitch of the wire To solve signal short violation, we take the metal layer and put it on to upper metal layer. We check via rules via width via spacing.  

<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/f4e6ba96-3deb-4675-9fe0-ca6daa30e10a">  
<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/47135e42-21e1-4e5f-a7cb-fccdcd90fe96">  
<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/cc9c05ac-5dd5-4e4f-ad3a-d36b8ffa1262">  

<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/75a51f01-c34b-4b17-be9b-b25a10413c85">  
<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/47022cb5-9530-47e5-a3a0-d0236d538609">  
<img width="250" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/b68b0bf8-3006-4fc3-9dea-613ef70bd369">  

<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/9cd493b1-9e9a-4dd2-a081-f892253ebcdf">  

</details>
<details>
<summary>
Power distribution network and routing
</summary>

<i> **LAB** </i> :  
PDN must be generated after CTS and post-CTS STA analyses.
```
gen_pdn
```

We can confirm the success of PDN by checking the current def environment variable: ```echo $::env(CURRENT_DEF)```  

- ```gen_pdn``` - Generates the Power Distribution network.
- The power distribution network has to take the ```design_cts.def``` as the input def file.This will create the grid and the straps for the Vdd and the ground. These are placed around the standard cells.
- The standard cells are designed such that it's height is multiples of the space between the Vdd and the ground rails. Here, the pitch is 2.72. Only if the above conditions are adhered it is possible to power the standard cells.
- The power to the chip, enters through the power pads. There is each for Vdd and Gnd
- From the pads, the power enters the rings, through the via. The straps are connected to the ring. Vdd straps are connected to the Vdd ring and the Gnd Straps are connected to the Gnd ring. There are horizontal and the vertical straps.
- Now the power has to be supplied from the straps to the standard cells. The straps are connected to the rails of the standard cells.
- If macros are present then the straps attach to the rings of the macros via the macro pads and the pdn for the macro is pre-done.
- There are definitions for the straps and the railss. In this design straps are at metal layer 4 and 5 and the standard cell rails are at the metal layer 1. Vias connect accross the layers as required.

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/d621bace-65ac-4995-aaf5-8256c019bd40">  

<i> **Routing** </i> :
OpenLANE uses the TritonRoute tool for routing. There are 2 stages of routing:
1. Global routing: Routing region is divided into rectangle grids which are represented as course 3D routes (Fastroute tool).
2. Detailed routing: Finer grids and routing guides used to implement physical wiring (TritonRoute tool).

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/9ecd3b99-7633-4f06-82c5-dc953564fc62">  


</details>

<details>
<summary>
Triron route features
</summary>

Features of TritonRoute:
- Honouring pre-processed route guides.
- Assumes that each net satisfies inter guide connectivity.
- Uses MILP based panel routing scheme.
- Intra-layer parallel and inter-layer sequential routing framework.

<i> **Pre-processed route guides** </i> :  TritonRoute places significant emphasis on following pre-processed route guides. This involves several actions:
- Initial Route Guide Analysis: TritonRoute analyzes the directions specified in the preferred route guides. If any non-directional routing guides are identified, it breaks them down into unit widths.
- Guide Splitting: In cases where non-directional routing guides are encountered, TritonRoute divides them into unit widths to facilitate routing.
- Guide Merging: TritonRoute merges guides that are orthogonal (touching guides) to the preferred guides, streamlining the routing process.
- Guide Bridging: When it encounters guides that run parallel to the preferred routing guides, TritonRoute employs an additional layer to bridge them, ensuring efficient routing within the preprocessed guides.
- Assumes route guide for each net satisfy inter guide connectivity Same metal layer with touching guides or neighbouring metal layers with nonzero vertically overlapped area( via are placed ).each unconnected termial i.e., pin of a standard cell instance should have its pin shape overlapped by a routing guide( a black dot(pin) with purple box(metal1 layer))

<img width="500" alt="![image](https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/007ef3bb-4477-42ba-b936-1d0b58918c5e)

<i> **Inter guide connectivity and intra-inter layer routing** </i> :
Two guides are connected if They are on the same metal layer with touching edges or they are on neighbouring metal layers with a non zero vertically overlapped area. Each unconnected terminal should have its pin shape overlapped by a route guide.  
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/d3594a3d-6a6b-4505-9a10-ffd2d44a0fb9">  

<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/6ad6605e-0856-417e-a329-d7de50eb5420">  

<i> **Handling connectivity** </i> :  
The inputs to triton detailed route are lef file, def file, preprocessed route guides. THe outputs are detailed routing solutions with optimized wire length and via coun. Constraint files: Route guide honoring, connectivity constraints and design rules.
- Access Point: An on grid metal poiny on the route guide, used to connect to lower layer segments, upperlayer pins or io ports.
- Access Point Cluster: A collection of all access points derived from lower layer segments upper layer guide a pin or an io port.

<img width="600" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/9db457f1-a6cb-496e-bd12-4f4486b9f857">  

<i> **Topology Algorithm** </i> :  
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/d351c6a6-6b30-406e-8e31-6809259683df">  




</details>

### References
1. https://www.vsdiat.com
2. https://github.com/Devipriya1921/Physical_Design_Using_OpenLANE_Sky130
3. https://github.com/Rachana-Kaparthi/ADVANCED-PHYSICAL-DESIGN-USING-OPENLANE-SKY130
4. https://chat.openai.com
5. http://opencircuitdesign.com/magic/
6. https://github.com/nickson-jose/vsdstdcelldesign/
7. https://github.com/The-OpenROAD-Project/OpenLane
8. https://github.com/alwinshaju08/Physicaldesign_openlane

