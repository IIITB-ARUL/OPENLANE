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

**GDII File  Generation**-Once the layout is verified and passes all checks, the final step is to generate the GDSII file format, which represents the complete physical layout of the chip. The GDSII file contains the geometric information necessary for fabrication, including the shapes, layers, masks, and other relevant details.


**OpenLane**

OpenLane is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow and toolchain that helps engineers and designers automate the process of designing and manufacturing custom integrated circuits. It is primarily used for creating semiconductor chips for various applications, such as microprocessors, memory chips, and custom ASICs.

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



![strivefamily](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/1e821aee-b987-4814-ad00-0d77c7689140)
![reports](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/5ac46367-4731-4aa2-93cb-49f361b43bd1)
![snynthesis](https://github.com/IIITB-ARUL/OPENLANE/assets/140998631/c25add3b-f24d-4252-9dd6-70c732637228)


</details>
