# Advanced-Physical-Design-using-open-lane

### Table of Contents
- [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
- [Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
- [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
- [Day 4 - Pre-layout timing analysis and importance of good clock tree](day-4---pre-layout-timing-analysis-and-importance-of-good-clock-tree)
- [Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA](#day-5---final-steps-for-rtl2gds-using-tritonroute-and-opensta)

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

<img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/7b760c4f-2dd0-40c1-9533-dbb1b13737f8">  

**OpenLane**

- It started as an Open-source flow for a true Open source tape-out experiment.
- Strive is a family of open everything SoCs. <img width="400" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/6bbdf602-9033-4594-ab31-5eafb0b70018">
- The main goal of OpenLane is to produce a clean GDSII with no human intervention.
- It is tuned for Skywater 130nm Open PDK, also supports XFAB180 and GF130G.
- It has 2 modes of operation: Autonomous and Interactive.

**OpenLane ASIC Flow**:  
<img width="500" alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/f02bc1db-eee4-4c52-aa64-98c0a7577b01">   

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
cd ~/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```

<img alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/bbe46115-0395-47a2-abce-93d9a9c80714">  

To view the netlist, use the following commands:  
```
cd designs/picorv32a/runs/RUN_2023.09.12_06.50.19//results/synthesis/
gedit picorv32.v
```

<img alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/51d96007-594e-4b3f-9bca-d07bc582eb30">  

Synthesis report can be seen by using these:
  ```
cd designs/picorv32a/runs/RUN_2023.09.12_06.50.19//results/synthesis/
gedit 1-synthesis.AREA_0.stat.rpt 
```

<img alt="image" src="https://github.com/Lasya-G/Advanced-Physical-Design-using-open-lane/assets/140998582/a40f673b-6dfd-4ba6-9e69-c1814e013057">  

Flop ratio = Number of flops/Total number of cells = 1596/10104 = 0.1579 

</details>  

### Day 2 - Good floorplan vs bad floorplan and introduction to library cells
<details>
<summary>
Chip Floor Planning considerations
</summary>
</details>
<details>
<summary>
Library binding and placement
</summary>
</details>
<details>
<summary>
Cell design and characterization flow
</summary>
</details>
<details>
<summary>
General timing characterization parameters
</details>

### Day 3 - Design library cell using Magic Layout and ngspice characterization
<details>
<summary>
Labs for CMOS inverter ngspice simulation
</summary>
</details>
<details>
<summary>
Inception of Layout A CMOS fabrication process
</summary>
</details>
<details>
<summary>
Sky130 Tech files Lab
</summary>
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
