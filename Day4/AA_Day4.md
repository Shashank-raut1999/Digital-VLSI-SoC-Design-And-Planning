# Day 4 


# Pre-layout timing analysis and importance of good clock tree:
---

## Table of Content

* [Guidelines For Standard Cell](#Guidelines-to-be-followed-while-making-a-standard-cell)
* [vsdstdcelldesign layout](#Verifying-Layout)
* [lef File Extraction](#lef-file-extraction)
* [Running OPENLANE](#OPENLANE)
* [Modifying parameters of cell](#Modifying-parameters-of-cell)
* [Timing Analysis with OpenSTA](#Timing-Analysis-with-OpenSTA-tool-after-synthesis)
* [Reducing slack by Fixes](#Reducing-slack-by-Fixes)
* [Process-flow on Newly Generated Netlist](#Process-flow-on-Newly-Generated-Netlist)
* [OPENROAD](#OPENROAD)
* [Assigned Task](#Assigned-Task)




#### Our task is to find the errors in the layout of *vsdstdcelldesign* and then fix them so that this layout can be plugged in the *picorv32a* process flow :
- While doing placement, we need the layout inforamtion which is present in **lef** file. So we need to get this lef file first.
## Guidelines to be followed while making a standard cell 
1. The input and the output ports must lie on the intersection of the vertical and the horizontal tracks.
2. The width of the standard cell must be in the odd multiple of the track pitch and height must be in the even multiples of the vertical pitch.


#### What is a track file?
- It is a file which contanins the info about the tracks and metal layers which will be used during the routing.
- To view the **track file** :
       - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/18557fb2-80e6-4c3c-8e92-116193540dfb)
       - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/0c03bf25-c51e-4629-8a2f-64f501bd1d6c)


- In above image : For example, an **li1** layer the horizontal track(X) is at an offset of 0.23 and has a pitch of 0.46.
- Similarly, for **li1** vertical track(Y) is at an offset of  0.17 and has a pitch of 0.34.



 ## Verifying Layout
  - Now, to open the vsdstdcelldesign layout :
  - Execute :
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c2a54ba5-2f09-42d9-bc41-915c19bad687)
  - We will see the following window:
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/fedc862c-9f70-483c-8f5c-2ce499a3749b)
  - We need to make the *Grid* visible to verify the layers pitches in the layout:
    - so execute the command ``` help grid ``` shown in below image in the **tkcon** window to know the syntax.
    - then we execute the command ``` grid 0.46um 0.34um 0.23um 0.17um ``` which are the horizontal and vertical pitches of the **li1** layer. The grid which we see in below image can be used to verify the **li1** layer.
   
      
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c644f411-ee16-4a1a-b111-bf5d88e537a8)
      

   - Having the ports at the intersection of the horizontal and vertical tracks ensure that  route can reach there from Y as well as X direction.
  
 - We can verify the __Guideline 2__ as :
    - The width of the standard cell must be in the odd multiple of the track pitch and height must be in the even multiples of the vertical pitch.
      
    -  Width of std cell = 3 * 0.46um = 1.38um

    - Height of std cell = 8 * 0.34um = 2.72um
  
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/0774d433-a8a7-4dc6-bf15-28e4b1d958c3)
  
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/908f24cf-50c3-4dd8-9496-abbf46ac3306)

           
 ## Info related to the ports is given in the website given below :
-   ``` https://github.com/nickson-jose/vsdstdcelldesign  ```
 




 ## lef file extraction 
  - The __lef__ is now ready to be extracted so we will execute the following instructions to give a name of our choice to this layout.

    
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b47f5876-8c37-430f-9275-5543be4e65d8)

  - The above shown instruction saves our layout by the name **sky130_vsdinv.mag** in the **vsdstdcelldesign** directory.
  - Now open this newly created  **sky130_vsdinv.mag** in *magic* and execute the ``` lef write ``` command in the *tkcon* window to make a lef file in the **vsdstdcelldesign**  directory .

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/7936ee23-daa1-4a8f-afc1-9a4fe5f87541)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2b8adb7a-70dd-4ebc-956b-a8f9e6714083)

  - Content of **lef file** :
      - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/90ce2e51-997a-4cd8-b50d-c95511369ecd)

  - Now this lef file is ready to be plugged in the **picorv32a** design.
  - Before that, we will copy this file and the library files to a **src** folder of __picorv32a__ directory so that all the required files are there in same location.
    
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d0e4631d-c301-4ac9-992d-0706654a92b2)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/5e79029b-16db-45af-9fbb-164faf0e8871)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2c362711-0cb8-4b9f-941a-837c6864e666)


## We need to modify the **config.tcl** file of **picorv32a** directory :

 - So open the **config.tcl** file of **picorv32a** directory in any editor and add the commands shown in below image :
 -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/231d7b3c-31eb-4d54-8bcb-35fdb5b9d9c6)

 - save and exit.

   
## OPENLANE 

- Go to the open lane directory and execute the ``` docker ``` command.
- Execute the following commands :

-    ```
         ./flow.tcl -interactive
         package require openlane 0.9
         prep -design picorv32a
         set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
         add_lefs -src $lefs
         run_synthesis
     ```

- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/0f483256-b624-49c6-9de8-840efa4e945c)

- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/670e12a3-9ffd-4d64-a244-65d5c91e624c)

- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d131974c-a3d9-4b7d-91ef-5b91a87d810e)


## Modifying parameters of cell

 - We will try to modify the parameters of our cell by referring the *README.md* file in the *configuration* folder in *openlane* directory :

 - The README.md file contains information about the parameters of the cell. We will refer the variable names and the parameter values from **synthesis** section of this file to modify the parameters of the cell :

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2c9bf0e5-79f8-4bca-9e61-334ad23c4743)

 - We will give the following commmands in the terminal where openlane is open
 -   ```
       prep -design picorv32a -tag 01-04_12-54 -overwrite
       set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
       add_lefs -src $lefs

       echo $::env(SYNTH_STRATEGY)
       set ::env(SYNTH_STRATEGY) "DELAY 3"
       echo $::env(SYNTH_BUFFERING)
       echo $::env(SYNTH_SIZING)
       set ::env(SYNTH_SIZING) 1
       echo $::env(SYNTH_DRIVING_CELL)
       run_synthesis
     ```

 - ```
   prep -design picorv32a -tag 01-04_12-54 -overwrite
   ```
 -  is used to overwrite the existing files with previous values of simulations.
 - The commands starting with the word **echo** are used to view the current values of the parameter whereas the commands starting with __set__ are used to set the parameter values.

   
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d3cbf2dc-6c80-4113-b3a1-695711ca964b)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/4b7478cf-e4fa-432d-a7e6-5a92cc2f708b)

 - In below image, we can observe that the **chip area** has increased and the value of **slack** has reduced : 

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/61cff799-5a01-4191-8db0-044c88e7d709)



#### Since synthesis of the picorv32a using our custom designed cell is successful, so we can run the floorplan :
  - ``` run_floorplan```
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/27a52c4e-31b0-411d-9e51-9655ebbb38ae)

  - Since, we are getting the error so first again do the synthesis using above commands and then we will use following commands to do the floorplan:
  -  ```
       init_floorplan
       place_io
       tap_decap_or
     ```

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/ce36d9b5-9e8c-4609-b57c-5950008a2d75)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/4c9feb30-50fc-4720-9ada-cf0cde610d99)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/aab50eaf-de2c-473e-9342-431e23af7d92)


#### Since floorplan of the picorv32a using our custom designed cell is successful, so we can run the placement :

  - ``` run_placement```
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/fe8f79bb-0959-4949-b0dc-a06ffeaf5560)
  - placement is successful.

#### Then execute the following command to open the magic :

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/298d4eb9-e4a3-4753-9af0-445fea68eec3)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b7e9cd69-5785-4f69-aaad-8c9da633da36)

  - We are able to observe our custom cell on the layout as follows :

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/a807be16-43f3-4fa3-9b25-24e2461d4404)

  - We can run the ```expand``` command in the *tkcon* window :

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/7270a6d6-2e4f-4f17-88d6-436b1b8ef05d)

  - The overlapping of our cell with other cells show that there is power rail sharing between them.

  - **Our custom cell was successfully integrated into the picorv32a design.** 


# Timing Analysis with OpenSTA tool after synthesis 

 - We will do STA on the initial picorv32a design which had timing violations.

 - First we need to run the synthesis using the following commands in openlane directory:

 - ```
       docker
       ./flow.tcl -interactive
       package require openlane 0.9
       prep -design picorv32a
       set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
       add_lefs -src $lefs
       set ::env(SYNTH_SIZING) 1
       run_synthesis
   ```

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/4fb311a5-be7c-4ecd-856e-638fbc92cf75)
  
   - Now we need to make a new ``` pre_sta.conf  ``` file containing the commands shown in below image in **openlane** directory for doing STA. We can do this by vim editor.
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/6aa2116b-f378-4e76-ad33-f84e741179d7)

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/deadb2ff-5c7f-461c-b603-0e30240d4e9f)
  



   - We need to remember that when we are outside __openlane__ , we cannot use the definitions of *environmental* variables which were used during synthesis. So, we will create a **my_base.sdc** file which will contain the definitions of environment variables.
  
   - Now, we also need to create ``` my_base.sdc ``` file containing the content shown in below image in ``` openlane/designs/picorv32a/src ``` directory :
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/30918e24-b86a-42eb-bad1-189ab4cd3d20)
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/1cfec651-3c72-42b8-9213-76ea7b434d84)
  
   
   #### To invoke the OpenSTA tool :

     - Go to the openlane directory in a new terminal and execute the ``` sta pre_sta.conf ``` command.
  
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c6371f98-0cd3-444f-ac6c-67cf755cb065)
  
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/69c0b627-d9ea-48cf-a52c-3928c7a74aeb)
  
     - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/cd8dde00-7cbc-4287-b6e0-4dd57ac93bdb)
  
     - In above image, we can see that the delay is more due to more Fanout.
     - We can check by executing the command ``` report_net -connections _pin no_ ```.
  
     - So, now we will try to change the FANOUT parameter and again do the synthesis by executing the following commands:
  
     - ```
       prep -design picorv32a -tag 02-04_05-27 -overwrite
       set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
       add_lefs -src $lefs
       set ::env(SYNTH_SIZING) 1
       set ::env(SYNTH_MAX_FANOUT) 4
       echo $::env(SYNTH_DRIVING_CELL)
       run_synthesis
       
       ```

     -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b8478589-8ad1-4289-a391-98a163ee175c)
  
     -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f7b1e1ae-eaaa-4bc9-a287-af0ec39fe6e2)

 

  
  
      - Now, run the ``` sta pre_sta.conf ``` command in a new terminal in openlane directory :

      - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f1a416ce-0451-4397-85c4-1a496a678770)
  
      - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f8b079d4-c7c2-4335-b042-61b8b5fb305c)
  
      - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2df5633a-e176-4bc9-bbaf-de29cf7127d1)
  

# Reducing slack by Fixes 

  - Since OR gate which has a drive strength of 2 is driving 4 fanout.

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/98acba7c-893b-4444-bb75-61708c980e12)

  - So we will replace this **OR Gate** with another **OR Gate** having *Drive strength* of 4 by executing the following commands.

  - ```
      # Reports all the connections to a net
       report_net -connections _11672_

       # Checking command syntax
       help replace_cell

       # Replacing cell
       replace_cell _14510_ sky130_fd_sc_hd__or3_4

       # Generating custom timing report
       report_checks -fields {net cap slew input_pins} -digits 4

     ```

    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f739e988-0755-4181-b768-b493c0778e9a)
   
    - slack gets reduced compared to earlier slack:
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/e515dba8-79e2-46e3-808f-e68e6e24ead9)
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/bcfdf132-a109-4999-8a8e-5393889ee8d8)
   
    - In above case also OR gate which has a drive strength of 2 is driving 4 fanout.
    - So we will replace this **OR Gate** with another **OR Gate** having *Drive strength* of 4 by executing the following commands.
   
    - ```
      # Reports all the connections to a net
       report_net -connections _11675_

       # Replacing cell
       replace_cell _14514_ sky130_fd_sc_hd__or3_4

       # Generating custom timing report
       report_checks -fields {net cap slew input_pins} -digits 4
      ```

    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/456bf4bc-c167-48ea-9fd1-0a47fa48bb44)
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/adf05464-32cc-4dab-a0c0-8a20ed78284b)
   
    - The slack got reduced to 23.1405 ns.
   

    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d6f65073-4a37-401d-ab5c-d2472f779d6c)
   
    - In above case , an OR gate with a drive strength of 2 driving an Open Drain (OA) has more delay. So we will replace this OR gate with another OR gate having a drive strength of 4 by executing following commands:
   
    - ```
       # For reporting all the connections to a net
       report_net -connections _11643_

       # For replacing cell
       replace_cell _14481_ sky130_fd_sc_hd__or4_4

       # Generating custom timing report
       report_checks -fields {net cap slew input_pins} -digits 4
      ```

    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/337dd4d5-b2b2-438d-b04b-81c2e988527b)
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9b538eb3-8b7d-4068-9024-12c5123e4f18)
   
    - The slack is reduced to 23.1362 ns.
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/28a8d1ae-2163-4acf-b3fa-8f1ae4b7ecb3)
   
    - In above case also, an OR gate with a drive strength of 2 driving an Open Drain (OA) has more delay. So we will replace this OR gate with another OR gate having a drive strength of 4 by executing following commands:
   
    - ```
       # For reporting all the connections to a net
       report_net -connections _11668_

       # For replacing cell
       replace_cell _14506_ sky130_fd_sc_hd__or4_4

       # Generating custom timing report
       report_checks -fields {net cap slew input_pins} -digits 4
      ```

    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/bcccb2eb-3f1e-4460-9a9f-b45644f3490e)
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/e4434a4d-9782-4c4d-8837-f0dd6fdefedb)
   
    - The slack is reduced to 22.6173 ns.
   
    - Now, we need to verify whether the net **_14506_** is replaced with **sky130_fd_sc_hd__or4_4** or not.
    - So we will execute the following command:
    -   ``` report_checks -from _29043_ -to _30440_ -through _14506_ ```
   
    - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/1fe6a98c-14d6-4e7f-bd4c-a2ab43ca9d3d)
   


   
## Process-flow on Newly Generated Netlist 


 - Now we will replace the old netlist with this newly generated netlist which was generated after reducing slack. And then we will run *floorplan* , *placement* and *CTS* .

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9b89f8f6-294f-4697-aaf4-ef41b1172a27)

 - The image shown above contains the __netlist__ before making the modifications.
 - So, we will make a copy of this old netlist and then we will add the newly generated netlist to be used in our openlane flow.
 - So we can make the copy by writing the following code.
 - ```
       # First go to the following location
       cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/02-04_05-27/results/synthesis/

       # Listing contents of the directory
        ls -ltr

       # For copying the netlist with particular name 
        cp picorv32a.synthesis.v picorv32a.synthesis_old.v

       # Listing contents of the directory
         ls -ltr
    ```

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/ec2212d7-498d-4e02-89a5-145fc0cd9a51)
  
   - Now we can proceed to put the newly generated netlist.
   - So for that, we need to run the ``` write_verilog ``` command in the terminal where we did STA.
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c6d00f88-a5cc-4af2-95fa-dbe4dbed04aa)
  
   - Now, to verify whether the newly generated netlist is correct or not, we will check whether the changes we made earlier(replacing OR gates etc) are reflected in this new file or not.
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/6592cbd2-5cb7-47fe-8179-7c9a21adea09)
   - Hence, verified.
  


   ### Do synthesis , floorplan , placement and cts :

     - We will proceed with the earlier strategy which had no violations.
     - Go to the **openlane** directoer and execute the following commands:
     - ```
              prep -design picorv32a -tag 02-04_05-27 -overwrite
              set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
              add_lefs -src $lefs
              set ::env(SYNTH_STRATEGY) "DELAY 3"
              set ::env(SYNTH_SIZING) 1
              run_synthesis
              init_floorplan
              place_io
              tap_decap_or
              run_placement

              # Incase getting error
              unset ::env(LIB_CTS)

              run_cts
       ```

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/e7063531-eb7d-4169-8d76-a88bf0e59176)
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/02e6d1f0-3651-4768-89df-c57a7c2ef211)

   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/364c522c-ff49-40a8-ae43-a3a49ff06ad5)
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/a8cdfab7-1677-43fa-b150-54a2d0348c6a)
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2bc8779f-2090-4546-bf1b-b605ee86695f)
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/3733a480-715b-4962-bc18-bb816cb79605)
  
   - Now a **.cts** file is created in the location shown below:
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2929d85d-28c6-4317-b59f-6c4d9e6e1cd9)
  
   - This **cts** file consists of the content of the previous **picorv32a.synthesis.v** with newly added buffers.


# OPENROAD :
 - OPENROAD is readily integrated in the OPENLANE so there is no need of defining the environmental variables explicitly.
 - ```
   # Command to run OpenROAD tool
   openroad
   ```
 - In OPENROAD, timing analysis requires the creation og **db**(database) using __lef__ and __tmp__ file.
 - __db__ creation is a one time process. Once we have created it, we can use it anytime when we want to do analysis.
 - __db__ creation in commands :
 - ```
   # Reading lef file
   read_lef /openLANE_flow/designs/picorv32a/runs/02-04_05-27/tmp/merged.lef

   # Reading def file
   read_def /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/cts/picorv32a.cts.def

   # Creating an OpenROAD database file named pico_cts.db
   write_db pico_cts.db

   ```
 - This database file is present in **openlane** directory.

 - Then we can execute the following commands:
 - ```
   # For loading the created db file in Openroad
   read_db pico_cts.db

   # For reading netlist post CTS
   read_verilog /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/synthesis/picorv32a.synthesis_cts.v

   # For reading the library for design
   read_liberty $::env(LIB_SYNTH_COMPLETE)

   # For linking design and library
   link_design picorv32a

   # For reading the custom sdc we created
   read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

   # For setting all clocks as propagated clocks
   set_propagated_clock [all_clocks]

   # Generating custom timing report
   report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

   # To exit to Openlane flow
   exit

   ```

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2067321d-dbc1-4d5a-b373-ada143fc8427)


 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9f5fbd6d-754e-4c5c-9c49-2a16ab5b7422)
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/341b1eb0-d811-4149-829d-dfa86dd7fdd3)
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/e6f36ddc-598b-4a31-9882-38ed0c3636ea)



# Assigned Task 


- To remove sky130_fd_sc_hd__clkbuf_1 from clock buffer list CTS_CLK_BUFFER_LIST :

- For checking the current value of buffer list use the command:
- ``` echo $::env(CTS_CLK_BUFFER_LIST) ```
- For removing **sky130_fd_sc_hd__clkbuf_1** from the list use the command:
- ``` set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0] ```
- For checking current value of **CTS_CLK_BUFFER_LIST** use the command:
- ``` echo $::env(CTS_CLK_BUFFER_LIST) ```
- For checking current value of **CURRENT_DEF** use the command:
- ``` echo $::env(CURRENT_DEF) ```
- For setting def as placement def use the command:
- ``` set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/placement/picorv32a.placement.def ```

- Then we will run cts using:
- ``` run_cts ```
- For checking current value of **CTS_CLK_BUFFER_LIST** use the command:
- ``` echo $::env(CTS_CLK_BUFFER_LIST) ```

- Now we will follow the similar commands we used earlier to run **OPENROAD** :
- ```
  openroad
  read_lef /openLANE_flow/designs/picorv32a/runs/02-04_05-27/tmp/merged.lef
  read_def /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/cts/picorv32a.cts.def
  write_db pico_cts1.db
  read_db pico_cts.db
  read_verilog /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/synthesis/picorv32a.synthesis_cts.v
  read_liberty $::env(LIB_SYNTH_COMPLETE)
  link_design picorv32a
  read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
  set_propagated_clock [all_clocks]
  report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
  report_clock_skew -hold
  report_clock_skew -setup
  exit
  ```

 - Then again for Checking current value of **CTS_CLK_BUFFER_LIST** use the command:
 - ``` echo $::env(CTS_CLK_BUFFER_LIST) ```
 - Then for inserting **sky130_fd_sc_hd__clkbuf_1** to the first index of list use the command:
 - ``` set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1] ```
 - Then for verifying whether the **sky130_fd_sc_hd__clkbuf_1** was inserted to the first index of list or not, we will use:
 - ```echo $::env(CTS_CLK_BUFFER_LIST)```

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/c4a81eb5-25d9-46bf-bfb9-0b4e95cdbcf3)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/4a0c5871-7d26-4d23-80c0-8517f7f9fd51)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/535705a3-b9cb-48ef-9553-0b5ade1987f2)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/cd4d3d0e-6859-480b-aff0-2189ad54cf0b)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/46e6b7b7-b145-4eb6-af58-8fc98f2834f6)

