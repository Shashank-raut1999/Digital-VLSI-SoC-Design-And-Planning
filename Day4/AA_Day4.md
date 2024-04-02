# Day 4 :  Pre-layout timing analysis and importance of good clock tree

## Our task is to find the errors in the layout of *vsdstdcelldesign* and then fix them so that this layout can be plugged in the *picorv32a* process flow :
- While doing placement, we need the layout inforamtion which is present in **lef** file. So we need to get this lef file first.
## Guidelines to be followed while making a standard cell :
1. The input and the output ports must lie on the intersection of the vertical and the horizontal tracks.
2. The width of the standard cell must be in the odd multiple of the track pitch and height must be in the even multiples of the vertical pitch.


### What is a track file?
- It is a file which contanins the info about the tracks and metal layers which will be used during the routing.
- To view the **track file** :
       - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/18557fb2-80e6-4c3c-8e92-116193540dfb)
       - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/0c03bf25-c51e-4629-8a2f-64f501bd1d6c)


- In above image : For example, an **li1** layer the horizontal track(X) is at an offset of 0.23 and has a pitch of 0.46.
- Similarly, for **li1** vertical track(Y) is at an offset of  0.17 and has a pitch of 0.34.



 ## Now, to open the vsdstdcelldesign layout :
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
 




 ## *lef* file extraction :
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

   
## OPENLANE :

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


### We will try to modify the parameters of our cell by referring the *README.md* file in the *configuration* folder in *openlane* directory :

 - The README.md file contains information about the parameters of the cell. We will refer the variable names and the parameter values from **synthesis** section of this file to modify the parameters of the cell :

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2c9bf0e5-79f8-4bca-9e61-334ad23c4743)

 - We will give the following commmands in the terminal where openlane is open
 -   ```
       prep -design picorv32a -tag 01 -overwrite
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

 - ``` prep -design picorv32a -tag 01 -overwrite``` is used to overwrite the existing files with previous values of simulations.
 - The commands starting with the word **echo** are used to view the current values of the parameter whereas the commands starting with __set__ are used to set the parameter values.

   
 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d3cbf2dc-6c80-4113-b3a1-695711ca964b)

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/4b7478cf-e4fa-432d-a7e6-5a92cc2f708b)

 - In below image, we can observe that the **chip area** has increased and the value of **slack has reduced : 

 - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/61cff799-5a01-4191-8db0-044c88e7d709)



### Since synthesis of the picorv32a using our custom designed cell is successful, so we can run the floorplan :
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


### Since floorplan of the picorv32a using our custom designed cell is successful, so we can run the placement :

  - ``` run_placement```
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/fe8f79bb-0959-4949-b0dc-a06ffeaf5560)
  - placement is successful.

### Then execute the following command to open the magic :

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/298d4eb9-e4a3-4753-9af0-445fea68eec3)

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/b7e9cd69-5785-4f69-aaad-8c9da633da36)

  - We are able to observe our custom cell on the layout as follows :

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/a807be16-43f3-4fa3-9b25-24e2461d4404)

  - We can run the ```expand``` command in the *tkcon* window :

  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/7270a6d6-2e4f-4f17-88d6-436b1b8ef05d)

  - The overlapping of our cell with other cells show that there is power rail sharing between them.

  - **Our custom cell was successfully integrated into the picorv32a design.** 


# Now we will do the Timing Analysis with OpenSTA tool after synthesis :

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
  
   - Now we need to make a new ``` pre_sta.conf ``` file containing the commands shown in below image in **openlane** directory for doing STA. We can do this by vim editor.
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/433e9d53-9e4c-40fc-bcb7-bb1ffae2610d)
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/deadb2ff-5c7f-461c-b603-0e30240d4e9f)

  
   - Now, we also need to create ``` my_base.sdc ``` file containing the content shown in below image in ``` openlane/designs/picorv32a/src ``` directory :
  
   - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/30918e24-b86a-42eb-bad1-189ab4cd3d20)















 
