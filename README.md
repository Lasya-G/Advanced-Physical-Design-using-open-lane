# Advanced-Physical-Design-using-open-lane

### Table of Contents
- [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda---openlane-and-sky130-pdk)
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
</details>

<details>
<summary>
Get familiar to open-source EDA tools
</summary>
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
