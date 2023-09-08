# OPENLANE
# Day 1

<details>
  <summary>
    SOC design and OpenLANE
  </summary>

**Introduction to all components of open-source digital asic design**





**Simplified RTL2GDS flow**

**Synthesis**

  Synthesis translates the design RTL into circuits made out of components from standard cell library.Here the high level HDL code is converted into gatelevel netlist.Gatelevel netlist is the functional equivalent of RTL.

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

**Standard Cells**

These are the building blocks of the standard cell library.These are pre-designed, pre-characterized, and pre-verified collections of logic gates and flip-flops that can be used as building blocks for creating digital logic circuits.These are available in different sizes and different flavours  to accommodate different design requirements.

These cells have regulaar layout which has fixed height whereas the width is a discrete variable.Each cell has different models or views which are utilized by  the EDA tools.One of the views is liberty view which consists  of delay model,power model,etc.

**Floor Planning**

The main obejective of floor planning is to plan silicon area and robust power distribution.Floorplanning sets the foundation for subsequent steps in the physical design process, such as placement and routing.
Floorplanning includes the allocation of input and output pads or pins, ensuring that they are accessible for external connections and follow design rules, such as avoiding signal contention.

**Macro Floor Planning**


  
  



</details>
