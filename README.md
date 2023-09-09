# OPENLANE
# Day 1

<details>
  <summary>
    SOC design and OpenLANE
  </summary>

**Introduction to all components of open-source digital asic design**





**Simplified RTL2GDS flow**

**Synthesis**- Synthesis translates the design RTL into circuits made out of components from standard cell library.Here the high level HDL code is converted into gatelevel netlist.Gatelevel netlist is the functional equivalent of RTL.

Example:

```
always @(posedge clk)
  if(c)
    q<=a;
  else
    q<=b;
```
The synthesization of the above verilog code is

![synthesis](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/7b9bc10f-6d91-4497-b9e7-f0e19733d117)

**Standard Cells**-These are the building blocks of the standard cell library.These are pre-designed, pre-characterized, and pre-verified collections of logic gates and flip-flops that can be used as building blocks for creating digital logic circuits.These are available in different sizes and different flavours  to accommodate different design requirements.

These cells have regulaar layout which has fixed height whereas the width is a discrete variable.Each cell has different models or views which are utilized by  the EDA tools.One of the views is liberty view which consists  of delay model,power model,etc.

**Floor Planning**-The main obejective of floor planning is to plan silicon area and robust power distribution.Floorplanning sets the foundation for subsequent steps in the physical design process, such as placement and routing.
Floorplanning includes the allocation of input and output pads or pins, ensuring that they are accessible for external connections and follow design rules, such as avoiding signal contention.

**Macro Floor Planning**-Macro floorplanning focuses specifically on the placement and arrangement of large functional blocks or macros within the chip. Macros are predefined, often complex modules or IP blocks.

**Chip Floor Planning**-Chip floorplanning involves the placement of all components and functional blocks on the entire semiconductor chip, from the highest hierarchy down to the individual standard cells.

**Placement**-Placement involves determining the specific locations and orientations of all the individual components, such as standard cells, macros, I/O pads, and other functional blocks, within the semiconductor chip's layout.
Components has to be placed close to each other to reduce interconnect delay to enable successful routing.

*Gobal Placement*

Here components are positoned optimally.Cells may overlap and also can go offrows.

*Detailed Placement*

Here the positions are obtained form the positions achieved by the global placement and altered minimally.

**Clock tree synthesis**-Its primary purpose is to create a well-structured and efficient clock distribution network throughout the chip. The clock tree ensures that clock signals reach all the sequential elements (e.g., flip-flops) with minimal skew, jitter, and power consumption.
>clock source - roots,clock elements - leaves.


**Routing**-Routing is a fundamental step in the physical design of integrated circuits (ICs) that follows placement and is responsible for creating the physical connections between various components, such as standard cells, macros, and I/O pads, on the semiconductor chip. Routing aims to establish the pathways for both the signal and power/ground nets, ensuring that signals can flow correctly between different parts of the chip while adhering to design constraints and rules.

**Physical verification**-Physical verification, also known as design rule checking (DRC) and layout versus schematic (LVS) checking, is concerned with the correctness and manufacturability of the physical layout of the IC. It involves ensuring that the layout adheres to the design rules and constraints of the semiconductor fabrication process. 

**Timing verification**-Timing verification is concerned with the performance and functionality of the IC with respect to timing requirements. It ensures that the design meets specified timing constraints and operates correctly under various conditions. 




  
  



</details>
