# Day 3


## **Can we change the parameters of floorplan?**
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

# SPICE Deck:
     

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
          -     ```  git clone https://github.com/nickson-jose/vsdstdcelldesign.git  ```
   -  We copied the __sky130A.tech__ file from */home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech*  into the cloned *vsdstdcell* directory.

   - We then execute the following command to see the inverter in *magic* . 
       - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f3b98ffe-9ae2-4a81-b93a-e93833d8241e)
       -  ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/f6203232-97e1-4026-9eaa-d7b44291015a)
         



    
