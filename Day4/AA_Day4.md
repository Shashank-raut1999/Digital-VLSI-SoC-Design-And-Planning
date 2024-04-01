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

 - So open the **config.tcl** file of **picorv32a** directory in **vim** editor and add the following commands :
 -   ```          set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"  
                     set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
                     set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
                     set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
        
                     set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
      ```
 - 
   







 
