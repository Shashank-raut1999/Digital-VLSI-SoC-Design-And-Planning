# Day 2

# Floorplanning And Introduction to library cells 
---

## Table of Content :
* [Defining the width and height of CORE and DIE](#1-Defining-the-width-and-height-of-CORE-and-DIE)
* [Concept of preplaced cells](#2-Concept-of-preplaced-cells)
* [Need for Decoupling Capacitors](#3-Need-for-Decoupling-Capacitors)
* [Need for Power Planning](#4-Need-for-Power-Planning)
* [Pin placement and logical cell placement blockage](#5-Pin-placement-and-logical-cell-placement-blockage)
* [LAB Work](#6-LAB)
* [Netlist Binding and Physical Cells](#Netlist-Binding-and-Physical-Cells)
* [Cell Design Flow](#Cell-Design-Flow)
    +[Inputs](#Inputs)
    +[](#)
    +[](#)


## 1. Defining the width and height of CORE and DIE :
  - The width and height help us to analyse the terms like utilization factor and aspect ratio.
  - **Utilization Factor** = *Area Occupied by the Netlist* / *Total Area of the Core*
  - **Aspect Ratio** = *Height* / *Width*
  -  Ex:   Netlist occupies an area of 4 sq units  

       - ![Netlist area](image.png) 
      
      - Core has a total area of 8 sq units

        - ![Total Core area](image-1.png)

      - Utilization factor = 4/8 = 0.5
      - Aspect Ratio of core = 2/4 = 0.5    

## 2. Concept of preplaced cells:
  - If some portion of a netlist is repeating many times in the circuit then we can create separate block for this netlist and place it on floor before placement and routing process. These are called as pre-placed cells. We can not modify the positions of these blocks.
  - Examples of pre-placed cells are : MUX, Memory, Clock-gating cell,etc.
    - Ex: ![Pre-placed cells](image-2.png)


## 3. Need for Decoupling Capacitors:
   - ![Decoupling cap](image-3.png)

   - Due to the voltage drop occuring across the wire, the voltage across the circuit may not be sufficient to provide peak current required during switching. 
   -This may cause the output voltage of the circuit to be in an undefined state(voltage may lie in the undefined region of voltage-transfer-characteristics plot).
   - So, we can use a __Decoupling Capacitor__ put across the circuit so that this capacitor can be charged upto the voltage supply potential.
   - This helps the circuit to get the peak current while switching with proper output voltages to be recognized as logic 1 or logic 0.
    - ![Decoupling Capacitor](image-4.png)  


   - These decoupling capacitors are used to surround the pre-placed cells to avoid any problems.


       - ![pre placed cells](image-5.png)  

## 4. Need for Power Planning:
  
  - Ex : A 16-bit Bus is connected to an inverter
    - ![alt text](image-6.png) 
    - **Ground Bounce:**
      - ![alt text](image-7.png)
    - **Voltage Droop**
      - ![alt text](image-8.png)
        
   - The voltage Droop and Ground Bounce problems will occur when we are using a single voltage source for more number of cells due to which this single supply source is not able to supply enough charge.

   **We can solve this by using multiple power supply lines (known as MESH) as shown in figure below:**  
      - ![alt text](image-9.png)

   **Mesh**   :   ![alt text](image-10.png)

## 5. Pin placement and logical cell placement blockage

   - Consider an example of the following circuit:
      - ![alt text](image-11.png)

      - The input, output and clock pins are to be created on the chip to be able to be interfaced with external packages.

   - We place various pins on the  empty area near the edges of the die. 
   - The position of pins should facilitate the routing process.
   - The clock pins are wider than the i/o pins to provide minimum resistance path so that this clock signal can be transferred as fast as possible because these clock pins drive all the blocks on core continuously.
   - ![alt text](image-12.png) 

   - **Logical Cell Placement Blockage** :
         We need to do this so that the automated placement and routing tools does not place cells in the area of pins. 

        - ![alt text](image-13.png)  

# 6. LAB: 
   - ![alt text](image-14.png) 
   - The README.md file in the above shown location contains all the information related to the processes flow parameters.
     - ![alt text](<Reademe for configurartion folder.png>)
     - Each parameter can be termed as a switch whose value can be modified during the process flow but not in this file.

     

  - The __floorplan.tcl__ file in the __configuration__ folder contains the system default values for the parameters. This file has the lowest priority.
       - ![alt text](image-15.png) 

    -  
       After that the priority is for __config.tcl__ file in the following location:

         ![alt text](image-16.png)
    
    - And then the highest priority is for __sky130A_sky130_fd_hd_sc_config.tcl__ file in the above shown location.

    - we then run the __run_floorplan__ command exactly where we were left with the __synthesis_succesful__ message.
    -  ![Floorplan succesful](image-17.png)

    - To view the results of the completed floorplan we go to the following location
       -  ![alt text](image-24.png) 

     - The __def__(design exchange format) file contains all the floorplan info after doing the floorplan process.
        - ![alt text](image-19.png)

     - In this  def file we can observe :
      ![alt text](image-20.png)


      - UNITS DISTANCE MICRONS 1000 : Database units per micron
      - DIE AREA (X0,Y0) (Xf,Yf) : shows the dimensions.
      
      - so we need to divide these dimensions by 1000 to get the size in microns  (i.e.micrometers)
      -  Xf = 660685/1000 = 660.685 um
      -  Yf = 671405/1000 = 671.405 um
      - Die Area = Xf * Yf = 443587.212 (um)^2

  - Since we can not read everything from a def file, we will use __magic__ tool to visually look at the floorplan. Execute the following command to run magic:
    - ![alt text](image-32.png)
   - 

   - ![magic](image-22.png)
  - This is the view of floorplan in the magic tool.
  - We can make the following observations in this tool :
      - As seen in the config.tcl file, horizontal pins are on __metal layer3__ :
      - ![alt text](image-23.png)
      - Vertical pins are on __metal layer2__ :
      - ![alt text](image-25.png)
     

    - We can see the tapcells present on the floor which avoid the latchup occuring in the CMOS by connecting N-well to Vdd and substrate to the ground. : ![alt text](image-26.png) 

    - There are standard cells present on the floor:
      - ![alt text](image-27.png)


  ## Netlist Binding and Physical Cells:

   - **Library** : It consists info about various types of cells, their shapes, sizes, threshold voltages and delays. We can have same logic function being performed by cells of different shapes and delays .
   - Ex : ![alt text](image-28.png)
     
   - **Placement** : By considering the physical shapes of cells from library, we need to place the cells on the floor efficiently.
     - ![alt text](image-29.png)

     - We need to optimize the placement and also use __repeaters__(buffers)so that the cells which are placed far from the pin ports can get correct logic signal which can get deteriorated due to longer wire losses.
      - Ex: ![alt text](image-31.png)
    
   - The wires of different blocks getting crossed will be routed through different layers. 
  
  - **Placement** : 
    - Placement  occurs in two stages : **Global** and **Detailed** and there are different tools to perform these.
    - __Global Placement__ :
       - Focuses on coarse placement and there are no legalizations happening here which means that there may be some overlaps of cell areas happening here.
       - Main aim is to reduce wire length (known as Half Parameter Wire Length i.e. HPWL in OPENLANE). 
       
    - __Detailed Placement__ :
      - This is done to solve the legalization problem after global placement.    

    -  we use the ``` run_placement ``` command to run.

     - ![alt text](image-33.png)
    

    - Then we will run the following command to view the placement in magic tool :
      - ![alt text](image-34.png)

      - we can observe the placement of standard cells as follows:
       - ![alt text](image-35.png)

  ## Cell Design Flow:
   - ![alt text](image-36.png)

     1. __Inputs__ :
      The inputs to cell design flow are provided from the foundry in form of PDKs. Some of the PDKs are as follows :
        - **DRC and LVS** files : These contain the info related to the allowable physical dimensions of devices.
        - **SPICE Models** files : These contain all the info related to the MOSFET devices like threshold voltage, oxide capacitances, oxide thickness , etc.
        - **User defined specifications** files : 
           - Cell width : cells with higher drive strength have higher width.
           - Cell height : It is decided by the separation between power and ground rails.
           - Supply voltage : It may be required to build cells which operate on particular voltages.
           - Metal layers : It may be required to build cells which are present on particular metal layers.
           - Pin locations and Drawn-gate length are some of the other user defined specs.

      2. __Design Steps__ :
         - __Circuit Design__ : This is done to model the PMOS and NMOS in order to meet the library requirements. The file generated by this step is called __CDL__(Circuit Description Language) file which will be given to layout.  
         - __Layout Design__ : In this step, we make the *Euler's Path* and *Stick Diagram* for a particular design.
         - We need to convert the stick diagram into a layout adhering to the rules defined in DRC files.
         - This step produces GDSll output files, LEF file(containing the height and width of MOS) output file and extracted spice netlist  (.cir file)output file (containing parasitics of each and every element in the circuit) .
      
      3. __Characterization__ :
            - This helps us to get the *timing* , *power* and *noise* information.
            - This is done by **GUNA** software.
            - The output files produced by this step are :
              - Timing, Power and Noise libs and functions.


        ### Timing Characterization:

        - Based on the definitions of parameters present in the *Timing Characterization* step, we calculate upcoming definitions.
          - ![alt text](image-37.png)

        -  If the treshold points are not chosen correctly we can get a problem of *Negative time delay* .
        - ![alt text](image-38.png)

        - We can also get negative time delay if the input slew is very high which occurs if the length of wire for input is large.
       - ![alt text](image-39.png)


- End
