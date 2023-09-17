# OPENLANE
# Day 1

<details>
  <summary>
    Introduction
  </summary>
**RISC-V Architecture**

RISC-V is an open-source instruction set architecture (ISA) designed for use in computer processors. It's named after the five "RISC" principles: Reduced Instruction Set Computing. Unlike proprietary ISAs like x86 (used by Intel and AMD) and ARM (used by various companies, including Apple and Qualcomm), RISC-V is open and freely available for anyone to use, modify, and implement. So C program will compile into assembly language and then converted into binary format. This binary will execute on the layout itself.

RISC-V is a versatile and open ISA that has the potential to disrupt the semiconductor industry by democratizing processor design and enabling a wide range of applications. Its openness, simplicity, and flexibility make it an attractive choice for both industry professionals and educational institutions. 




**Simplified RTL2GDS flow**


![rtlflow](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/7aee3c43-cf1c-407b-83a0-f60aedbecfa3)

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



![stdcell](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/3108e86e-741e-43c6-8f20-4260bfb168a7)





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

**Sign-off**-Sign-off involves a comprehensive set of checks, analyses, and reviews to ensure that the design meets all specifications, performance targets, and manufacturing requirements. 

**GDSII File  Generation**-Once the layout is verified and passes all checks, the final step is to generate the GDSII file format, which represents the complete physical layout of the chip. The GDSII file contains the geometric information necessary for fabrication, including the shapes, layers, masks, and other relevant details.

</details>



<details>
  <summary>
    SOC design and OpenLANE
  </summary>
  

**OpenLane and Strive chipsets**

OpenLane is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow and toolchain that helps engineers and designers automate the process of designing and manufacturing custom integrated circuits. It is primarily used for creating semiconductor chips for various applications, such as microprocessors, memory chips, and custom ASICs.

![strivefamily](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/1e821aee-b987-4814-ad00-0d77c7689140)


**OpenLane ASIC Flow**

![olasicflow](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/bcc8a481-0fd8-49ff-819b-645db677e6b4)



**OpenLane Installation**

**Installation of dependencies**

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```

**Docker Installation**

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 


# Check for installation
sudo docker run hello-world
```

**Installation of OpenLane**

```
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
cd OpenLane
make
make test
cd /home/arulvignesh/OpenLane/designs/ci
cp -r * ../
```  

**Synthesis in OpenLane**

```
cd ~/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis

```

Synthesis statistics:

![reports](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/5ac46367-4731-4aa2-93cb-49f361b43bd1)



>Flop ratio = Number of D Flip flops/total Number of cells    =  1596/10104   = 0.1579

            
             


             
</details>




# Day 2

<details>
  <summary>
    Chip floor Planning and considerations
  </summary>

**Width and height of core and die**-Core is where the logic blocks are placed and this seats at the center of the die. The width and height depends on dimensions of each standard cells on the netlist. **Utilization factor is (area occupied by netlist)/(total area of the core)**. In practical scenario, utilization factor is 0.5 to 0.6. This is space occupied by netlist only, the remaining space is for routing and more additional cells. **Aspect ratio is (height)/(width)** of core, so only aspect ratio of 1 will produce a square core shape.

**Preplaced Cells**-These are reusable complex logicblocks or modules or IPs or macros that is already implemented (memory, clock-gating cell, mux, comparator...) . The placement on the core is user-defined and must be done before placement and routing (thus preplaced cells). The automated place and route tools will not be able to touch and move these preplaced cells so this must be very well defined

**Decoupling capacitors**-During a logic state change an increased demand on current behavior happens. Resistance in a non-idea circuit means there are multiple voltage drops betwen the supply and logic circuit.

>Noise Margin : voltages should be inside a logic margin value (NM_l or NM_h) do be detected as 0 or 1, respectively. Voltage drops can affect the result for the logic outcome (undefined region). Decoupling capacitors are placed next to the preplaced cells to prevent the voltage drops during transition.


![decap](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/2df18d9c-f43a-49f4-8703-1ca9e025ea62)



**Power Planning**-Power Planning Decoupling capactor for sourcing logic blocks with enough current is not feasible to be applied all over the chip but only on the critical elements (preplaced complex logicblocks). Large number of elements switching to logic 0 might cause ground bounce due to large amount of current that needs to be sink at the same time, and switcing to logic 1 might cause voltage droop due to not enough current from the powersource to source needed current of all elements. Ground bounce and voltage droop might cause the voltage to not be within the noise margin range. The solution is to have multiple powersource taps (power mesh) where elements can source current from the nearest VDD and sink current to the nearest VSS tap. This is the reason why most chips have multiple powersource pins.



**Pin placement**-Taking into account the inputs, outputs and preplaced cells, the netlist is defined (via VHDL/Verilog).
Normally input and output pins are placed at opposite sides of the core.
Pin placement also depends on where the logic blocks are placed - this requires a full understanding of the design.
The communication ("handshake") between frontend team (that defined the network connectivity) and backend team (that defines the pin placement) is also critical.
Clock ports are bigger in size, as the clock drives the flip flops and require more current/less resistance.



Floorplan envrionment variables or switches:

    FP_CORE_UTIL - floorplan core utilisation
    FP_ASPECT_RATIO - floorplan aspect ratio
    FP_CORE_MARGIN - Core to die margin area
    FP_IO_MODE - defines pin configurations (1 = equidistant/0 = not equidistant)
    FP_CORE_VMETAL - vertical metal layer
    FP_CORE_HMETAL - horizontal metal layer


![floorplan](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/fbd35697-9616-4dfe-b014-db930b7e1138)



**Lab**

**Run floorplan on OpenLane:** `% run_floorplan`


Floorplan envrionment variables or switches:

    FP_CORE_UTIL - floorplan core utilisation
    FP_ASPECT_RATIO - floorplan aspect ratio
    FP_CORE_MARGIN - Core to die margin area
    FP_IO_MODE - defines pin configurations (1 = equidistant/0 = not equidistant)
    FP_CORE_VMETAL - vertical metal layer
    FP_CORE_HMETAL - horizontal metal layer

 
**Check the results.** The output of this stage is `runs/[date]/results/floorplan/picorv32.def` which is a [design exchange format](https://teamvlsi.com/2020/08/def-file-in-vlsi-design-exchange.html), containing the die area and positions. 
```
...........
DESIGN picorv32a ;
UNITS DISTANCE MICRONS 1000 ;
DIEAREA ( 0 0 ) ( 660685 671405 ) ;
............
```
The die area here is in database units and 1 micron is equivalent to 1000 database units. **Thus area of the die is (660685/1000)microns\*(671405/1000)microns = 443587 microns squared.** 

**View the layout on magic**. Open def file using `magic`:  

```
magic -T /home/arulvignesh/OpenLane/vsdstdcelldesign/libs/sky130A.tech lef read tmp/merged.nom.lef def read results/floorplan/picorv32a.def &

```
![floorplan1](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/601facbc-1d47-4f34-9b93-2c7344326bf4)
![floorplan2](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/d079cbf6-7966-4366-8002-e2f99b30c7ad)
![floorplan](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/9a94d940-f61a-461c-abe2-b2a73fe8b450)

To center the view, press "s" to select whole die then press "v" to center the view. Point the cursor to a cell then press "s" to select it, zoom into it by pressing 'z". Type "what" in `tkcon` to display information of selected object. These objects might be IO pin, decap cell, or well taps as shown below.  


![floorplan3](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/818c9f05-993e-4a6c-aafa-9ba96a1a5899)

</details>
<details>
  <summary>
    Library binding and placement
  </summary>


**Placement**

First we need to bind the netlist with physical cells. We have shapes for OR, AND and every cell for pratice purpose. But in reality we dont have such shapes, we have give an physical dimensions like rectangles or squares weight and width. This information is given in libs and lefs. Now we place these cells in our design by initilaising it.

![placement1](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/0cb284b4-ecec-45f2-8d07-dae5ab220739)
![placement2](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/d2126daf-d80d-4583-9c8a-d0dfd382dc14)



**Optimization**

The next step is placement. Once we initial the design, the logic cells in netlist in its physical dimisoins is placed on the floorplan. Placement is perfomed in 2 stages:

Global Placement: Cells will be placed randomly in optimal positions which may not be legal and cells may overlap. Optimization is done through reduction of half parameter wire length. Detailed Placement: It alters the position of cells post global placement so as to legalise them. Legalisation of cells is important from timing point of view.

Optimization is stage where we estimate the lenght and capictance, based on that we add buffers. Ideally, Optimization is done for better timing.

![placement3](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/d3f85659-9c29-4df4-bf4d-89016cc08b80)


**Run placement:** `% run_placement`. This commmand is a wrapper which does global placement (performed by RePlace tool), Optimization (by Resier tool), and detailed placement (by OpenDP tool). It displays hundreds of iterations displaying HPWL and OVFL. The algorithm is said to be converging if the overflow is decreasing. It also checks the legality. 

**View the output of this stage**. The output of this stage is `runs/[date]/results/placement/picorv32a.placement.def.` To see actual layout after placement, open def file using `magic`:  

```
magic -T /home/arulvignesh/Openlane/vsdstdcelldesign/libs/sky130A.tech lef read tmp/merged.nom.lef def read results/placement/picorv32.def &
```
![placementOL](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/86cd041a-7df4-4730-829e-4bb8d4646fbe)

![placementOL1](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/a3289db5-185c-43ba-a36d-0e63682e6363)
![placementOL2](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/c9c5e61e-5675-4984-9b0f-ab283bc5e71e)


</details>

<details>
  <summary>
    Cell design and characterization flows
  </summary>

**Library Characterization:**
Of all RTL-to-GDSII stages, one common thing that the EDA tool always need is data from the library of gates which keeps all standards cells (and, or, buffer gates,...), macros, IPs, decaps, etc. Same cells might have different flavors inside the library (different sizes, delays, threshold voltage). Bigger cell sizes means bigger drive strength to drive longer and thicker wires. Bigger threshold voltage (due to bigger size) will take more time to switch(slower clock) than those with smaller threshold voltage.  

A single cell needs to go through the cell design flow. The inputs to make a single cell comes from the foundry Process Design Kits:
 - DRC & LVS Rules = tech files and poly subtrate paramters (CUSTOME LAYOUT COURSE)
 - SPICE Models  = Threshold, linear regions, saturation region equations with added foundry parameters. Including NMOS and PMOS parameteres (Ciruit Deisgn and Spice simulation Course)
 - User defined Spec = Cell height (separation between power and ground rail), Cell width (depends on drive strength), supply voltage, metal layer requirement (which metal layer the cell needs to work)

The library cell developer must adhere to the rules given on the inputs so that when the cell is used on a real design, there will be no errors. Next is design the library cell:
1. Design the circuit function (Output: circuit design language (CDL))
2. Model the pmos and nmos that meets input library requirement
3. Layout the design using Euler's path and sticky diagram to produce best area. This can be done on `magic` layout tool.The outputs are:
   - GDSII (layout file)
   - LEF (defines the width and height of cell)
   - extract spice netlist .cir (parasitics of each element of cell: resistance, capacitance)
 Afte design is characterization using GUNA software, where the outputs are timing, noise, and power characterization.

</details>
<details>
  <summary>
    Timing charaterization
  </summary>


  ### Timing characterisation

In standard cell characterisation, One of the classification of libs is timing characterisation.

Timing defintion | Value
------------ | -------------
slew_low_rise_thr  | 20% value
slew_high_rise_thr |  80% value
slew_low_fall_thr | 20% value
slew_high_fall_thr | 80% value
in_rise_thr | 50% value
in_fall_thr | 50% value
out_rise_thr | 50% value
out_fall_thr | 50% value

### Propagation Delay and Transition Time

#### Propagation Delay:
The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. Poor choice of threshold values lead to negative delay values. Even thought you have taken good threshold values, sometimes depending upon how good or bad the slew, the dealy might be still +ve or -ve.

```
Propagation delay = time(out_thr) - time(in_thr)
```
#### Transition Time:

The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```


</details>

## Day 3
<details>
  <summary>
    Cmos inverter using NGSPICE
  </summary>
</details>


<details>
  <summary>
Inception of Layout and CMOS Fabrication Process  </summary>
</details>


<details>
  <summary>
Lab on SKY130 Tech File
</details>



<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
* [Kunal Ghosh - Co-founder (VSD Corp. Pvt. Ltd)](https://github.com/kunalg123)
* [Nickson Jose - VSD VLSI Engineer](https://github.com/nickson-jose)


