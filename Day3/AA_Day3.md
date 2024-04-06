# Day 3

# Design library cell using Magic Layout and ngspice characterization
---

## Table Of Content:
 * [Changing parameters of floorplan](#Can-we-change-the-parameters-of-floorplan)
 * [SPICE Deck](#SPICE-Deck)
 * [Cloning Git Repo for Standard cell of Inverter](#Cloning-Git-Repo-for-Standard-cell-of-Inverter)
 * [16-Mask CMOS Fabrication Process](#16-Mask-CMOS-Fabrication-Process)
 * [SPICE Extraction](#SPICE-Extraction)
 * [Rise Time](#Rise-Time)
 * [Fall transition time](#Fall-transition-time)
 * [Rise Cell Delay](#Rise-Cell-Delay)
 * [Fall Cell Delay](#Fall-Cell-Delay)
 * [Finding DRC Errors by Magic tool](#Finding-Errors-in-DRC-by-Magic-tool)
     + [Finding error in poly.mag file](#Finding-error-in-polymag-file)


## Can we change the parameters of floorplan?
==> YES.
- Let us try to change the distance between pins in the floorplan process. Since, changing the values is allowed in OPENLANE process flow.
- We need to copy the parameter name from the config.tcl file in the floorplan folder inside results whose value is to be changed.
- We then paste this command prefixed with __set__ word in the terminal window where we had run the run_floorplan command.


   -   ![alt text](image.png)
 - In this command we changed the distance between the input pins and output pins on the floor which were placed equidistant earlier. 

- Then execute the __run_floorplan__ command again to proceed with the changed parameter.
-  Then again we need to run the invoke the magic tool as done earlier.
   - ![alt text](image-1.png)
   - we observe that the pins are now not equidistant.
 
     

# SPICE Deck
     

 - ![alt text](image-2.png)

 - We can write the SPICE Deck as follows :

    - ![alt text](image-4.png)
    - The lines with stars are the comments.
    - The connections related to the PMOS and NMOS are give in a specific order as :
      - __mosname__ - __drain__ - __gate__ - __substrate__ - __source__ - __W__ - __L__.

    - The connections for other elements are given in the order :
        - __elementname__ - __uppernode__ - __lowernode__ - __value__

    - Some examples of spice simulation commands are shown in the above image.

# Cloning Git Repo for Standard cell of Inverter :


   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b7e12c2a-01b6-46ab-846c-9137bec58b61)
   - We cloned the inverter in __openlane__ directory by using the command :

     
     -   ```  git clone https://github.com/nickson-jose/vsdstdcelldesign.git  ```
     
   -  We copied the __sky130A.tech__ file from */home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech*  into the cloned *vsdstdcell* directory.

   - We then execute the following command to see the inverter in *magic* . 
       - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f3b98ffe-9ae2-4a81-b93a-e93833d8241e)
       -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f6203232-97e1-4026-9eaa-d7b44291015a)
                   
# 16-Mask CMOS Fabrication Process:

**1. Selection of substrate :**
   
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/42116f08-9f7b-44bf-bf33-9ce78ff8d8fc)
  


**2. Active Region Formation:**

   
   - This step is done to ensure isolation between NMOS and PMOS when both the NMOS and PMOS will be made in the same substrate.
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/eb7f612e-1017-4bd7-bde2-6c4ec53621d3)

**3. N-Well and P-Well Formation :**

   - In this step, the NMOS is formed by masking the PMOS region and then ion implanting the donor atoms(Phosphorous) of required dose in the NMOS region.
   - Similarly, the PMOS region is formed by masking the NMOS region and then ion implanting the acceptor atoms(Boron) of required dose in PMOS region.
   - Then the substrate is placed in a high temperature furnace so that the doped atoms can diffuse into the silicon.
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/4a3b9aaf-94fc-4ef1-bf9b-3eb14d9c1835)

**4. Formation of Gate:**

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/a18af614-0d83-463f-9799-6e427ff2da10)

**5. LDD Formation(Lightly Doped Drain) :**

   - This step is done to address the problem of *Hot electron* and *Short Channel Effects*.
   - Hot electron problem occurs when a large electric field due to large supply voltage is induced across a small channel MOSFET.
   - Short Channel effects occur when in MOSFETs having channel length lower than 1um. Due to which the channel length is comparable to the depletion regions present on drain-substrate interface. So, the drain voltage tries to influence the current in channel region.
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/df94f36c-9c70-4506-bca5-835ae0708833)

**6. Source and Drain Formation:**

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/8ee11e00-50b2-4ed4-a32e-04d051a2e842)

 **7. Formation of Contacts and Local Interconnects:**

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/34e2df4f-b920-4e67-ba40-5c22f4e4878c)

 **8. Higher Layer Metal Formation:** 

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9090e1fe-eec5-438c-b28a-a893bd9a975c)



## SPICE Extraction 

 - To know the logical functions of the inverter, we will first extract the spice and then we will do the simulations in ngspice.
 - **Extracting SPICE**:
     - we will execute the following command in the *tkcon* window of magic tool :
           - ``` extract all ```
     - <img src="https://github.com/Shashank-raut1999/SoC/assets/165283786/a4164186-d7d1-46ef-a9fd-9fd3d5953075" width="70%">
     - This will create an **sky130_inv.ext** file in the *vsdstdcelldesign* directory.
     - Then we will use this **sky130_inv.ext** file to create a SPICE file to be used in *ngspice* tool by executing the following command in the *tkcon* window of magic tool:
     -   ``` ext2spice cthresh 0 rthresh 0 ```                       <br>
     -   ```  ext2spice ```
                                             
     - *ctresh 0 rthresh 0* in above command will extract all the parasitic capacitaces also.
        - <img src="https://github.com/Shashank-raut1999/SoC/assets/165283786/80aa7926-2b4a-45b7-8e5c-7033dbace60c" width="70%">
        - we are now able to see these files in the *vsdstdcelldesign* directory.
        - <img src="https://github.com/Shashank-raut1999/SoC/assets/165283786/ab58d687-2325-4143-b751-c31b62acb97f" width="70%">

     - We can use **vim** editor in linux to view the content of spice file by running the following command in the terminal:
         -  ``` vim sky130_inv.spice ```
         - <img src="https://github.com/Shashank-raut1999/SoC/assets/165283786/7aff29ef-547f-4e83-b95c-34a81fcf4948" width="70%" height="70%">
         - The __.option scale__ specifies the dimensions of the grid box in layout.
         - We will edit this file to  make the required changes and set values of voltage sources and include the *transient* analysis commands.
         - To edit this file in vim editor :
              - Open this file in vim editor in terminal.
              - Press "I" to invoke the "Insert" mode for editing.
              - make the required changes.
              - Press __Esc__ and then type  ``` :wq ``` to save and exit.
              - <img src="https://github.com/Shashank-raut1999/SoC/assets/165283786/ffd6600b-fe15-4523-be58-d767e87b5fa7">



         - Then we will run the command
                    ```  ngspice sky130_inv.spice ```
           to invoke the ngspice tool.
           - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/bc422fe1-fae0-4bea-92df-9da87ea1decb)
           - We will get the following view:
             - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/71abf27f-527c-4571-8a24-fe807126d3c8)

           - We will then execute the command
                   ```     plot y vs time a  ```

           -  The input and output transient waveforms will be plotted as follows:
            - <img src="https://github.com/Shashank-raut1999/SoC/assets/165283786/8a150de3-0ed6-4be2-a249-be8d7b84329f" width="70%">
            - When we click on any point in that  graph, the corresponding x and y values are immediately displayed in the terminal. We can use this feature to calculate risetime, falltime, etc.
               
          - We can  execute the ```   exit  ``` command to exit ngspice.
                   

 
## Rise Time
   -  Rise Time = Time taken by output to rise to 80% of its final value - Time taken by output to rise to 20% of its final value
   -  output rises to 20% of its final value at 2.1825 ns.
   -  output rises to 80% of its final value at 2.2464 ns
   -  **RISE TIME = 2.2464  - 2.1825 = 0.0639 ns = 63.9ps**
   -  Screenshot at 20% (i.e 0.66V) :
   -   ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/039283cd-9fad-4c76-a8de-403bbd173ced)
   -   Screenshot at 80% (i.e. 2.64V) :
   -   ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/dd765fb3-8888-4ead-91bf-fef37f933cc8)

## Fall transition time
 - Fall transition time = Time taken by output to fall to 80% of its final value - Time taken by output to fall to 20% of its final value
 - output rises to 20%(i.e. 2.64V) of its final value at 4.0527 ns
 - output rises to 80%(i.e. 660mV) of its final value at 4.0951 ns
 - **FALL TIME =4.0951 - 4.0527 = 0.0424ns = 42.4ps**
 - Screenshot at 20% :
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/22733522-8410-4466-a426-039d78efc356)
 - Screenshot at 80% :
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/1e508261-d0c5-4b3c-8699-c89e946d9dfb)

## Rise Cell Delay
  - Rise Cell Delay = Time taken by output to rise to 50% of its final value - Time taken by input to fall to 50% of its final value
  - 50% of 3.3V = 1.65V
  - Time taken by output to rise to 50% of its final value is 2.2121ns
  - Time taken by input to fall to 50% of its final value is 2.1516ns
  - Rise Cell Delay = 2.2121ns - 2.1516ns = 0.0605ns = **60.5ps**
  - Screenshot at 50% values of input and output
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/38a1476b-9225-4f18-a770-0bff4448b3d0)

## Fall Cell Delay
  - Fall Cell Delay = Time taken by output to fall to 50% of its final value - Time taken by input to rise to 50% of its final value  
  - 50% of 3.3V = 1.65V
  - Time taken by output to fall to 50% of its final value is 4.077ns
  - Time taken by input to rise to 50% of its final value is 4.050ns
  - Fall Cell Delay =  4.077 - 4.050  = ns = **27ps**
  - Screenshot at 50% values of input and output :
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/1ab851ec-5e59-4a58-a08e-7e2df1864edd)


### We have successfully characterized our inverter.
  - Our next objective is to create a __lef file__ using the layout and we will plugin this lef file in the __picorv32a__ core.



# Finding Errors in DRC by Magic tool 
   - For help related to Magic DRC we can go to the website:
           -  http://opencircuitdesign.com/magic/Technologyfiles/TheMagicTechnologyFileManual/DrcSection
   - Link to Google_Skywaters Design Rules:
           - https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html
   - For reference , we can use the github repo of Google-Skywater:
           -  https://github.com/google/skywater-pdk


  #### Follow the steps:
   - First go to the home directory.
   - To download the lab files for performing DRC corrections:
   - ``` wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz ```
 
   - To extract the lab files from the downloaded file:
   - ``` tar xfz drc_tests.tgz ```
 
   - Then go inside the lab folder **drc_tests**.
   - Then we can list all the directories by using the ``` ls -al ``` command.
   - Then we will view the .magicrc file by using the ``` gvim .magicrc ``` command.This file is the startup script for magic. It tells magic where to find the technology file. The technology file is already made available locally in the same directory because we may need to make changes to this file.
   - Then we will open the magic tool by using ``` magic -d XR & ``` command to start the magic in better graphics.

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/fbef7633-258b-46f1-9a33-ac460639299c)
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2bcccf49-5880-4a16-a29c-f6084a95e5f3)

   - Content of .magicrc file:
        - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d879bc80-88d5-4e20-a072-4c67af922585)


   - Just for example we open the **Met3.mag** file in the magic tool.
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/3e7a9dec-51be-49fa-aa45-9d8f71effaad)
     - In the above layout we have a couple of independent layouts, most of them with a DRC error flagged in white hash colour.
     - Each layout is named with a name and that name calls out the rule number in the Google-Skywater documentation at the following link.
     - ``` https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html ```
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9a67ecb6-c026-460b-a58e-04a514a214cc)
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b466352b-9e31-4c59-a39e-6662cd0f50f8)
     - We can make a rectangle by mouse clicks and then hovering the mouse pointer over the *metal3 contact* icon and pressing the ```p``` button.
     - Then we can execute the command ``` cif see VIA2 ``` in the *tkcon* terminal.
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/1fc28ab8-4c47-424f-94ec-87b6dd7c9a05)
     - The squares in the above layout don't actually exist in the layout but they actually represent is a mask layer for **via2** when we will generate GDS from layout.
     - we can select the layout and press ``` d ``` to delete that layout.
     - We can press __Shift+Z__ to zoom out and __Ctrl+Z__ to zoom in.
       
 
## Finding error in poly.mag file
 - How to find an error in the poly.mag file in the drc_tests directory?
   - First we will open the **poly.mag** file in the magic tool either by File/open option in the magic window itself or by typing the command ``` load poly.mag``` in the *tkcon* terminal.
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/063affd5-493f-4251-b009-9c31c061635c)

   -  Let us consider the rule **poly.9**.
   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c9c2eddd-776e-423a-b2e1-ac64da63ed18)

   -  Then we will check the website for that rule i.e. __poly.9__:
   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/8554e9ea-fdc6-4745-b742-7674d8e47c0d)
   -  In the layout in the poly.9 file, the spacing between the **npolyres** and **poly** is only 0.21 microns but it should have been 0.48 microns according to the website. But still there is no error shown in the magic tool.

   -  So now, we need to update the sky130A.tech to show the DRC error in the tool.
   -  So we will open the file **sky130A.tech** file which is present in the **drc_tests** directory with the text editor of linux itself.
   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2d8672fe-479d-4a74-be73-449372c9b0e4)
   -  We will search for **poly.9** in this file by using the __Find__ option.
   -  We find it in 2 places . First one in the **POLY** section and the second one in the __uhrpoly__(__polyresistor__) section. The rules are not set properly in both these places.
   -  So we will add a change in both of these sections.
   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/cf482f7d-65b4-4d81-a035-0899da6d33b9)
   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/5d1aea80-b68d-4989-b811-d6037f517f62)
   - Click on *Save* and close the editor file.
   -  Then we will execute the commands shown in the *tkcon* terminal in below image.
   -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/700f06dd-2d72-4dbc-86bd-500cd345bccf)
   -   We are now able to see the drc warnings(white regions) in the misplaced polyresistance regions.

## How to correctly implement poly resistor spacing to diff and tap?

 - To do this, we need to again make the changes in the **sky130A.tech** file using similar steps we did earlier.
 - Following changes are to be made:
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/13a20057-a46b-4115-9baa-139047b74185)
 - Click on *Save* and close the editor file.
 - Then execute the following commands in the *tkcon* terminal:
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/06ab815b-292e-490f-886c-8ef9cbba9523)

 - We can make the copy of the poly.9 model of poly.mag file in the magic window itself and check for errors:
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b7a0b161-b4e2-4e16-9819-b385c0192aef)
 - We can select the area having drc error and then run the ``` drc why ``` command in the *tkcon* terminal to find the description of that error.


## How to find an error in the *nwell.mag* file in the *drc_tests* directory?
 - Let us open the **nwell.mag** file in the magic tool and look for the **nwell.6** model error.
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c121f441-e31e-4420-ac5c-6d77f949dc14)
 - In above figure , the deep nwell is shown in the yellow stripes and the nwell is shown in dotted green pattern.
 - We can check this error on the site as:
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9a42a604-2ae9-4a5a-8c3b-8fdda801562a)

 - The rule says that the edge of the deep nwell needs to be covered with the overlap all the way around by a ring of regular nwell.

 - Follow the similar steps as done earlier and open the __sky130A.tech__ file in editor as done earlier.
 - The changes we need to make in __sky130A.tech__ file are as follows:
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/022df9f7-0351-44b6-8bb4-0647ec4b90b0)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/074af68f-e990-49db-ba6e-3898851fb4e5)

 - Click on *Save* and exit.
 - Then again open the magic tool and open the **nwell.mag** file in it.
 - Then execute the commands shown in **tkcon** window in a sequence as shown in the following image:
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/0f5b1830-4a1c-49cc-84f5-9e193dd04002)
 - We can observe that now the magic is able to recognize the error and display it.
     
