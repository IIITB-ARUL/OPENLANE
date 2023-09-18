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


 Configurations on OpenLANE can be changed on the flight. For example, to change IO_mode to be not equidistant, use `% set ::env(FP_IO_MODE) 2;` on OpenLANE. The IO pins will not be equidistant on mode 2 (default of 1). Run floorplan again via `% run_floorplan` and view the def layout on magic. However, changing the configuration on the fly will not change the `runs/config.tcl`, the configuration will only be available on the current session. To echo current value of variable: `echo $::env(FP_IO_MODE)`


### Designing a Library Cell:
1. SPICE deck = component connectivity (basically a netlist) of the CMOS inverter.
2. SPICE deck values = value for W/L (0.375u/0.25u means width is 375nm and lengthis 250nm). PMOS should be wider in width(2x or 3x) than NMOS. The gate and supply voltages are normally a multiple of length (in the example, gate voltage can be 2.5V)  
3. Add nodes to surround each component and name it. This will be used in SPICE to identify a component.    

**Notes:**
 - Width is the length of source and drain. Length is the distance between source and drain
 - PMOS' hole carrier is slower than NMOS' electron carrier mobility, so to match the rise and fall time PMOS must be thicker (less resistance thus higher mobility) than NMOS  
 - A good refresher on MOSFETS and CMOS [is this video](https://www.youtube.com/watch?v=oSrUsM0hoPs) and [this site.](http://courseware.ee.calpoly.edu/~dbraun/courses/ee307/F02/02_Shelley/Section2_BasilShelley.htm)

### SPICE Deck Netlist Description:  

![cmos](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/f4353e82-90e6-42c7-b778-e234674dd365)

**Notes:**
 - Syntax for the PMOS and NMOS descriptiom:
     - `[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]`
 - All components are described based on nodes and its values
 - `.op` is the start of SPICE simulation operation where Vin will be sweep from 0 to 2.5 with 0.5 steps
 - `tsmc_025um_model.mod` is the model file containing the technological parameters for the 0.25um NMOS and PMOS
The steps to simulate in SPICE:
```
source [filename].cir
run
setplot 
dc1 
plot out vs in 
```  

### SPICE Analysis for Switching Threshold and Propagation Delay:
CMOS robustness depends on:  

1. Switching threshold = Vin is equal to Vout. This the point where both PMOS and NMOS is in saturation or kind of turned on, and leakage current is high. If PMOS is thicker than NMOS, the CMOS will have higher switching threshold (1.2V vs 1V) while threshold will be lower when NMOS becomes thicker.

2. Propagation delay = rise or fall delay

DC transfer analysis is used for finding switching threshold. SPICE DC analysis below uses DC input of 2.5V. Simulation operation is DC sweep from 0V to 2.5V by 0.05V steps:
```
Vin in 0 2.5
*** Simulation Command ***
.op
.dc Vin 0 2.5 0.05
```  
Below is the result of SPICE simulation for DC analysis, the line intersection is the switching threshold:  

![cmos1](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/e1a3308c-d13f-4d81-a143-8aacb7756c0d)




Meanwhile, transient analysis is used for finding propagation delay. SPICE transient analysis uses pulse input: 
1. starts at 0V
2. ends at 2.5V
3. starts at time 0
4. rise time of 10ps
5. fall time of 10ps
6. pulse-width of 1ns
7. period of 2ns  

![cmos2](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/a63347c5-75d4-49e9-af26-db8fa98bce28)

The simulation operation has 10ps step and ends at 4ns:  

```
Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
*** Simulation Command ***
.op
.tran 10p 4n
```  
Below is the result of SPICE simulation for transient analysis:

![cmos3](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/a42e7ef4-f5c2-4718-985f-a2fb4ad1dddf)

























</details>


<details>
  <summary>
Inception of Layout and CMOS Fabrication Process  </summary>














 ### CMOS Fabrication Process (16-Mask CMOS Process):  
 **1. Selecting a substrate** = Layer where the IC is fabricated. Most commonly used is P-type substrate  
 **2. Creating active region for transistor** = Separate the transistor regions using SiO2 as isolation
  - Mask 1 = Covers the photoresist layer that must not be etched away (protects the two transistor active regions)
  - Photoresist layer = Can be etched away via UV light  
  - Si3N4 layer = Protection layer to prevent SiO2 layer to grow during oxidation (oxidation furnace)  
  - SiO2 layer = Grows during oxidation (LOCOS = Local Oxidation of Silicon) and will act as isolation regions between transistors or active regions  
  
![cmos4](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/6d201027-5edb-42fb-b559-a59b9d2d10a4)

 **3. N-Well and P-Well Fabrication** = Fabricate the substrate needed by PMOS (N-Well) and NMOS (P-Well)  
  - Phosporus (5 valence electron) is used to form N-well  
  - Boron (3 valence electron) is used to form P-Well.  
  - Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated then Mask 3 while N-Well (PMOS side) is being fabricated
   
![cmos5](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/3cb7d833-3002-41de-8885-a1b9990a0106)

 **4. Formation of Gate** = Gate fabrication affects threshold voltage. Factors affecting threshold voltage includes:    
 
![cmos6](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/08192c17-df5a-4513-95fe-b42ba36b3622)

Main parameters are:
  - Doping Concentration = Controlled by ion implantation (Mask 4 for Boron implantation in NMOS P-Well and Mask 5 for Arsenic implantation in PMOS N-Well)
  - Oxide capacitance = Controlled by oxide thickness  (SiO2 layer is removed then rebuilt to the desire thickness)  
  
 Mask 6 is for gate formation using polysilicon layer.
 
![cmos7](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/49de0092-8946-4db6-a98a-daeedaa8b8ff)
**5. Lightly Doped Drain formation** = Before forming the source and drain layer, lightly doped impurity is added: 
 - Mask 7 for N- implantation (lightly doped N-type) for NMOS 
 - Mask 8 for P- implantation (lightly doped P-type) for PMOS.  
Heavily doped impurity (N+ for NMOS and P+ for PMOS) is for the actual source and drain but the lightly doped impurity will help maintain spacing between the source and drain and prevent hot electron effect and short channel effect. 

![cmos8](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/8a54d16a-43ba-4262-87a7-3cf575fe35bc)
**6. Source and Drain Formation** = Mask 9 is for N+ implantation and Mask 10 for P+ implantation  
 - Channeling is when implantations dig too deep into substrate so add screen oxide before implantation
 - The side-wall spacers maintains the N-/P- while implanting the N+/P+    
 
![cmos9](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/edf6646a-79d2-4d44-88b6-3af86d69938e)

**7. Form Contacts and Interconnects** =  TiN is for local interconnections and also for bringing contacts to the top. TiS2 is for the contact to the actual Drain-Gate-Source. Mask 11 is for etching off the TiN interconnect for the first layer contact. 

![cmos10](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/7a631789-8398-4328-9224-6f5b8dced37d)

**8. Higher Level Metal Formation** = We need to planarize first the layer via CMP before adding a metal interconnect. Aluminum contact is used to connect the lower contact to higher metal layer. Process is repeated until the contact reached the outermost layer.
 - Mask 12 is for first contact hole
 - Mask 13 is for first Aluminum contact layer
 - Mask 14 is for second contact hole
 - Mask 15 is for second Aluminum contact layer. Mask 16 is for making contact to topmost layer. 
 
![cmos11](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/32e288b1-ecd8-4891-9391-71181b8e07f9)























  
</details>


<details>
  <summary>
Lab on SKY130 Tech File
  </summary>





### Layout and Metal Layers:

When polysilicon crosses N-diffusion/P-diffusion (diffusion is also called implantation), then an NMOS/PMOS is created. [Explained here](https://electronics.stackexchange.com/questions/223973/why-diffusions-in-cmos-cad-tool-magic-is-continuous) is the reason why the diffusion layer of source and drain "seems" to be connected under the polysilicon (diffusion layer for source and drain supposedly be separated).


The first layer is local-interconnect layer or local-i then metal 1 to 5. [Here is the process stack diagram](https://skywater-pdk.readthedocs.io/en/main/rules/assumptions.html) of sky130nm PDK. Metal 1 is for Power and Ground lines. `Nsubstratecontact` connects the N-well to locali. `licon` connects the locali to metal1.Locali is for local connections of cells. 

The layer hierarchy for NMOS is: Psubstrate -> Psubstrate Diffusion (psd) -> Psubstrate Contact (psc) -> Local-interconnect (li) -> Mcon -> Metal1. For poly: Poly -> Polycontact -> Locali. P-substrate diffusion an N-substrate diffusion is also referred to as P-tap and N-tap. 

The output of the layout is the LEF file. [LEF (Library Exchange Format)](https://teamvlsi.com/2020/05/lef-lef-file-in-asic-design.html) is used by the router tool in PnR design to get the location of standard cells pins to route them properly. So it is basically the abstract form of layout of a standard cell. `picorv32a/runs/[DATE]/tmp` contains the merged lef files (cell LEF and tech LEF). Notice how metal layer directon (horizontal or vertical) is alternating. Also, metal layer width and thickness is increasing. 

### Magic Commands:  
- Left click = lower-left corner of box  
- Right click = upper-right corner of box  
- "z" = zoom in, "Z" = zoom out, "ctrl + z" = zoom into the box 
- Middle click on empty area will turn the box into empty (similar to erasing it)
- "s" three times will select all geometries electrically connected to each other  
- `:box` = display parameters of selected box  
- `:grid` 0.5um 0.5um = turn on/off and set grid   
- `:snap user` = snap based on current grid  
- `:help snap` = display help for command  
- `:drc style drc(full)` = use all DRC when doing DRC checking
- `:paint poly` = paint "poly" to current box
- `:drc why` = show drc violation inside selected area (white dots are DRC violations )
- `:erase poly` = delete poly inside the box
- `:select area` = select all geometries inside the box
- `:copy n 30` = copy selected geometries to North by 30 grid steps
- `:move n 1` = move selected geometries to North by 1 step ("." to move more, "u" to undo)  
- `: select cell _08555_` = select a particular cell instance (e.g. cell \_08555_ which can be searched in the DEF file)
- `:cellname allcells` = list all cells in the layout
- `:cellname exists sky130_fd_sc_hd__xor3_4` = check if a cell exists 
- `:drc why` = show DRC violation and also the DRC name which can be referenced from [Sky130 PDK Periphery Rules](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root).



### Lab - Slew Rate and Propagation Delay Characterization:

The task is to characterize a sample inverter cell by its slew rate and propagation delay.  



 View the mag file using magic `magic -T sky130A.tech sky130_inv.mag &`:  
 
 
![magiclayout](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/23f7ab5f-e1f2-4e45-afa3-d593265d2981)


 Make an extract file `.ext` by typing `extract all` in the tkon terminal. 
 Extract the `.spice` file from this ext file by typing `ext2spice cthresh 0 rthresh 0` then `ext2spice` in the tcon terminal.  


We then modify the spice file to be able to plot a transient response:

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.0774f
C1 VPWR Y 0.117f
C2 A Y 0.0754f
C3 Y VGND 2f
C4 A VGND 0.45f
C5 VPWR VGND 0.781f
//.ends

.tran 1n 20n
.control
run
.endc
.end
```  

Open the spice file by typing `ngspice sky130A_inv.spice`. Generate a graph using `plot y vs time a` :  


![ngspice1](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/240baa41-403e-4033-810a-3a51a54c8362)


![ngspice2](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/ca26218f-111a-4678-a71a-880cae1ff4b2)



Using this transient response, we will now characterize the cell's slew rate and propagation delay:  
- Rise Transition [output transition time from 20%(0.66V) to 80%(2.64V)]:
    - **Tr_r = 2.19981ns - 2.15739ns = 0.04242 ns**  


- Fall Transition [output transition time from 80%(2.64V) to 20%(0.66V)]:
   - **Tr_f = 4.0672ns - 4.04007ns = 0.02713ns**   


- Rise Delay [delay between 50%(1.65V) of input to 50%(1.65V) of output]:
   - **D_r = 2.18197ns - 2.15003ns = 0.03194ns**   


- Fall Delay [delay between 50%(1.65V) of input to 50%(1.65V) of output]:
   - **D_f = 4.05364ns - 4.05001ns =0.00363ns**  
  


DRC Challenges
==============

Under this section, we will go over

- In-depth overview of Magic's DRC engine
- Introduction to Google/Skywater DRC rules
- Lab : Warm-up exercise : Fixing a simple rule error
- Lab : Main exercie : Fixing or create a complex error

Introdution to Magic and Skywater PDK
====================================
For running the DRC we need to have an understanding of the technology node we are working on. For this one can refer the following

- Magic --> [link]([https://www.github.com](http://opencircuitdesign.com/magic/))
- Skywater PDK 
- Github Repo for Skywater PDK --> [github](https://github.com/google/skywater-pdk)

Lab Setup
========

- Setup to view the layouts
- For extracting and generating views, Google/skywater repo files were built with Magic
- Technology file dependency is more for any layout. hence, this file is created first.
- Since, Pdk is still under development, there are some unfinished tech files and these are packaged for magic along with lab exercise layout and bunch of stuff into the tar ball
```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
- Once we have downloaded the archive in the home directory, we extract it to get the lab .mag files
- There is a hidden file ``.magicrc`` which directs to the various resources for the lab work ahead.

MAGIC
=====

- Run Magic.For better graphic use, the command belwo is used:
```
magic -d XR
```


- Other way to load it is by defining the name while running magic.
```
magic -d XR <file_name>.mag
```

- We will open up met3.mag
- We see multiple independent example metal layouts with some DRC errors. We can refer these errors in the the Skywater PDK design rules which are flageed in the DRC engine.
- We can make a frame around a metal region and in command window write drc why --> this gives us the DRC violated.
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/64ced32f-ff4b-49a0-87d7-de23971032ec)


- Magic uses a lot of derived layers. To see these layers we can make a large box area and use following commands to see metal cut
```
cif see VIA2
```
LAB
===

**Exercise-1**
- Load the poly.mag
- Check the drc violation for poly.9
- Refer the error using skywater pdk design rules
   - We find that distance between regular polysilicon & poly resistor should be 22um but it is showing 17um and still no errors . We should go to sky130A.tech file and modify as follows to detect this error.
- In line this,
```
*******************************************************
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
*******************************************************
```
- Edit as shown.
```
*******************************************************
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
*******************************************************
```

- Now the second edit. In line this.
```
*******************************************************
spacing xhrpoly,uhrpoly,xpc alldiff 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
*******************************************************
```
- Edit as shown.

```
*******************************************************
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
*******************************************************
```
- After this, we tech load ``sky130.tech`` file and execute ``drc check``

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/baacdb4a-831c-4cc4-aad1-12e46bba55e9)

- We can select poly.9 and ``run drc`` why to check for errors. Now it fine.
![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/f65ef446-ab80-46d2-9c38-32c9f590324c)


  
</details>



## Day 4

<details>
  <summary>
    Timing modelling using delay tables
  </summary>



To run previous flow, add tag to prep design:
```
prep -design picorv32a -tag [date]
```
**Extracting the LEF File:**
PnR tool does not need all informations from the `.mag` file like the logic part but only PnR boundaries, power/ground ports, and input/output ports. This is what a [LEF file](https://teamvlsi.com/2020/05/lef-lef-file-in-asic-design.html) actually contains. So the next step is to extract the LEF file from Magic. But first, we need to follow guidelines of the PnR tool for the standard cells:
 - The input and output ports lies on the intersection of the horizontal and vertical tracks (ensure the routes can reach that ports). 
 - The width of the standard cell must be odd multiple of the tracks horizontal pitch and height must be odd multiples of tracks vertical pitch   
 
 To check these guidelines, we need to change the grid of Magic to match the actual metal tracks. The `cd .volare/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info` contains those metal informations.   


 The file consists of 

```
  li1 X 0.23 0.46  //0.46um is the width  
  li1 Y 0.17 0.34  //0.34um is the height 
  met1 X 0.17 0.34
  met1 Y 0.17 0.34
  met2 X 0.23 0.46
  met2 Y 0.23 0.46
  met3 X 0.34 0.68
  met3 Y 0.34 0.68
  met4 X 0.46 0.92
  met4 Y 0.46 0.92
  met5 X 1.70 3.40
  met5 Y 1.70 3.40
```
1. Use `grid` command inside the tkon terminal to match the tracks informations:
```
 grid 0.46um 0.34um 0.23um 0.17um 
````

![grid1](https://github.com/IIITB-ARUL/Physical_design_using_OPENLANE/assets/140998631/5fd75363-a233-40c0-9bb5-9801768326aa)



### Delay Table:  

In order to avoid large skew between endpoints of a clock tree (signal arrives at different point in time):
 - Buffers on the same level must have same capacitive load to ensure same timing delay or latency on the same level. 
 - Buffers on the same level must also be the same size (different buffer sizes -> different W/L ratio -> different resistance -> different RC constant -> different delay).    
 
 ![image](https://user-images.githubusercontent.com/87559347/188773408-e503023f-0288-4993-a68a-5f20bccb886c.png)


Buffers on different level will have different capacitive load and buffer size but as long as they are the same load and size on the same level, the total delay for each clock tree path will be the same thus skew will remain zero. **This means different levels will have varying input transition and output capacitive load and thus varying delay.** 

Delay tables are used to capture the timing model of each cell and is included inside the liberty file. The main factor in delay is the output slew. The output slew in turn depends on **capacitive load** and **input slew**. The input slew is a function of previous buffer's output cap load and input slew and it also has its own transition delay table.

![image](https://user-images.githubusercontent.com/87559347/188783693-423bd170-dd0b-4f2f-9652-8fae9418df31.png)

Notice how skew is zero since delay for both clock path is x9'+y15.


  
</details>



<details>
  <summary>
    Timing analysis with ideal clocks using OpenSTA
  </summary>



 ### Timing Analysis (Pre-Layout STA using Ideal Clocks):
Pre-layout STA will not yet include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).    
![image](https://user-images.githubusercontent.com/87559347/189510818-050c6b22-a319-4969-a23e-c82c57ebd4ff.png)  

Setup timing analysis equation is:  
```
Θ < T - S - SU
```  

- Θ =  Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop  
- T = Time period, also called the required time
- S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time. 
![image](https://user-images.githubusercontent.com/87559347/189511212-8e1ea86f-b2d6-4a68-9948-7d9999087886.png)
- SU = Setup uncertainty due to jitter which is temporary variation of clock period. This is due to non-idealities of PLL/clock source.

  

Pre-Layout STA with OpenSTA:
STA can either be **single corner** which only uses the `LIB_TYPICAL` library which is the one used in pre-layout(pos-synthesis) STA or **multicorner** which uses `LIB_SLOWEST`(setup analysis, high temp low voltage),`LIB_FASTEST`(hold analysis, low temp high voltage), and `LIB_TYPICAL` libraries. 

1. Run STA engine using OpenROAD (which in turn calls OpenSTA): run OpenROAD first then source `/openlane/scripts/openroad/sta.tcl` which contains the OpenROAD commands for single corner STA. This file also contains the path to the [SDC file](https://teamvlsi.com/2020/05/sdc-synopsys-design-constraint-file-in.html) which specifies the actual timing constraints of the design. 
![image](https://user-images.githubusercontent.com/87559347/189568030-f442a238-21e8-4fc1-b5d0-22de00b11af9.png)
The result of running STA in OpenROAD will be exactly the same as the log result of STA after running `run_synthesis` inside OpenLane. Observe the delay:
![image](https://user-images.githubusercontent.com/87559347/189686801-46a9fb96-9be6-40c7-b62a-da3160489cb0.png)

2. To reduce negative slack, focus on large delays. Notice how net `_02682_` has big fanout of 5. Use `report_net -connections _02682_` to display connections. First thing we can do is to go back to OpenLane and reduce fanouts by `set ::env(SYNTH_MAX_FANOUT) 4` then `run_synthesis` again. As shown below, wns is reduced from -1.35ns to -0.82ns.  
![image](https://user-images.githubusercontent.com/87559347/189788023-9f6d85a9-a769-4b54-b156-2fa7b8980178.png)

3. To further reduce the negative slack, we can also try upsizing the cell with high fanout so bigger driver will be used. High fanout results in high load cap which then results in high delay. But since we cannot change the load cap, we can just change the cell size to better drive that large cap load for less delay. As shown below, cell `_41882_` has a high cap load of 0.04nF and this causes a large delay due to `buf_1` not having enough drive strength to drive that high cap load. We can try upsizing the `buf_1` to `buf_4` (listed on the used liberty files are all cells which you can choose) inside OpenSTA: `replace_cell _41882_ sky130_fd_sc_hd__buf_4` 
![image](https://user-images.githubusercontent.com/87559347/189793281-6acff965-b4d1-48a8-a6c3-17d312f901a2.png)

This can be done iteratively until desired slack is reached, this is called timing ECO (Engineering Change Order). To extract the modified verilog netlist: `write_verilog designs/picorv32a/runs/RUN_2022.09.14_05.18.35/results/synthesis/picorv32.v`. Beware that upsizing the cell will naturally increase core size. 

### Summary of OpenSTA Commands:  
```
report_net -connections _02682_
replace_cell _41882_ sky130_fd_sc_hd__buf_4`
report_checks -fields {cap slew nets} -digits 4
report_checks -from _18671_ -to _18739_ -fields {cap slew nets} -digits 4
report_wns
report_tns
report_worst_slack -max
write_verilog designs/picorv32a/runs/RUN_2022.09.14_05.18.35/results/synthesis/picorv32.v
```
</details>


<details>
      <summary> Clock tree synthesis TritonCTS and signal integrity </summary>

---

Clock Tree Synthesis (CTS) plays a vital role in the creation of integrated circuits (ICs), particularly in the realm of digital electronics, where precise timing is of utmost importance. CTS involves the establishment of an organized network or structure of pathways for distributing the clock signal within the IC. This meticulous process guarantees that the clock signal effectively reaches all the sequential components, such as flip-flops and registers, in a synchronized and punctual fashion.

It can be implemeted in various ways and the choice of the specific technique depends on the design requirements, constraints, and goals.
Some of the different types of approches to clock tree synthesis are:

- Balanced Tree CTS: The clock signal is spread out evenly, like branches of a tree. This helps ensure that all parts of the chip get the clock at about the same time, reducing timing problems. It's a straightforward method, but it might not save as much power as other methods.
- H-tree CTS: It is like a tree shape with the letter "H." It's great for spreading out clock signals across big chips. This tree structure helps make sure the timing is good and saves power, especially in large areas of the chip.
- Star CTS: In a star CTS, the clock signal is distributed from a single central point (like a star) to all the flip-flops. This approach simplifies clock distribution and minimizes clock skew but may require a higher number of buffers near the source.
- Mesh CTS: In a mesh CTS, clock wires are arranged in a mesh-like grid pattern, and each flip-flop is connected to the nearest available clock wire. It is often used in highly regular and structured designs, such as memory arrays. Mesh CTS can offer a balance between simplicity and skew minimization.
- Adaptive CTS: Adaptive CTS techniques adjust the clock tree structure dynamically based on the timing and congestion constraints of the design. This approach allows for greater flexibility and adaptability in meeting design goals but may be more complex to implement.

Crosstalk in VLSI
=================

Crosstalk in VLSI refers to unwanted interference or coupling between adjacent conductive traces or wires on an integrated circuit (IC) or chip. It occurs when the electrical signals on one wire influence or disrupt the signals on neighboring wires.Uncontrolled crosstalk can lead to data corruption, timing violations, and increased power consumption. Mitigation: VLSI designers employ various techniques to mitigate crosstalk, such as optimizing layout and routing, using appropriate shielding, implementing proper clock distribution strategies, and utilizing clock gating to reduce dynamic power consumption when logic is idle

Clock net sheilding in VLSI
===========================

Clock net shielding in VLSI refers to a technique used to protect the clock signal from interference or crosstalk. The clock signal is critical for synchronizing the operations of various components on a chip, and any interference can lead to timing issues and performance problems.
VLSI designers may use shielding techniques to isolate the clock network from other signals, reducing the risk of interference. This can include dedicated clock routing layers, clock tree synthesis algorithms, and buffer insertion to manage clock distribution more effectively.
VLSI designs often have multiple clock domains. Shielding and proper clock gating help ensure that clock signals do not propagate between domains, avoiding metastability issues and maintaining synchronization.

CTS LAB
=======
The below command is used to run CTS in OpenLANE.
```
run_cts
```
![run_cts](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/dbdd3e21-b2c4-4994-8196-4eb1a6b15eb0)


![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/427a0679-0ee3-4f14-adca-175ef6719174)

After CTS run, my slack values are ``setup:12.36, Hold:0.38``
Here also both values are not violating.


</details>

## DAY5 
<details>
      <summary> Maze routing and Lee's Algorithm </summary>

---
Routing is the process of establishing a physical connection between two pins. Algorithms designed for routing take source and target pins and aim to find the most efficient path between them, ensuring a valid connection exists.

The Maze Routing algorithm, such as the Lee algorithm, is one approach for solving routing problems.Here a grid similar to the one created during cell customization is utilized for routing purposes.
The Lee algorithm starts with two designated points, the source and target, and leverages the routing grid to identify the shortest or optimal route between them.

Lee's Algorithm has its limitations. It can be time consuming when dealing with millions of pins.It essentially constructs a maze and then numbers its cells from the source to the target. here are alternative algorithms that address similar routing challenges.

Here in this case he shortest path is one that follows a steady increment of one.There might be multiple paths, but the best path that the tool will choose is one with less bends.The route should not be diagonal and must not overlap an obstruction such as macros. The Lee algorithm prioritizes selecting the best path, typically favoring L-shaped routes over zigzags. If no L-shaped paths are available, it may resort to zigzag routes. This approach is particularly valuable for global routing tasks.

This algorithm however has high run time and consume a lot of memory thus more optimized routing algorithm is preferred .

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/4ab58f1b-3999-42ff-b722-f30ac2bcda45)

Design Rule Check
==================

Design rule checks are physical checks of metal width, pitch and spacing requirement for the different layers which depend on different technology nodes.It verifies whether a design meets the predefined process technology rules given by the foundry for its manufacturing.

The layout of a design must be in accordance with a set of predefined technology rules given by the foundry for manufacturability. After completion of the layout and its physical connection, an automatic program will check each and every polygon in the design against these design rules and report any violations.

![image](https://github.com/akul-star/Advanced-Physical-Design/assets/75561390/90753419-6485-48ab-9da4-84cfa30318f3)


</details>



<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
* Kunal Ghosh - Co-founder (VSD Corp. Pvt. Ltd)
* Nickson Jose - VSD VLSI Engineer
* Chatgpt
* N Sai Sampath,Colleague,IIIT-B


## Reference

- https://www.vsdiat.com
- https://github.com/Devipriya1921/Physical_Design_Using_OpenLANE_Sky130
- https://github.com/nickson-jose/vsdstdcelldesign
- https://github.com/AngeloJacobo/OpenLANE-Sky130-Physical-Design-Workshop

