# Day 5

#  Power Distribution Network

- General block diagram of PDN:
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/45e9dece-5cc0-4979-99ba-8ac88ba76b08)


- We will use the ``` gen_pdn``` command after the **clock tree synthesis** run to generate the power distribution network.
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/180d98a7-298c-4bd0-bd6a-89ebe763a462)
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/fe10a84a-50db-4a83-a4ce-04d651507806)

- The **current def** command in the above image is used to tell that the most recent step done was generation of PDN.
- The **17-pdn.def** file generated after this step contains the content of __cts.def__ along with the power distribution network.


# Routing
- To know about the different switches available for routing, we can refer the README.md file present in the *configuration* folder of *openlane* directory.
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/dba57401-ed9a-4e19-acd2-2db0ecced451)

- By using the following commands we can know, which type of global and detailed routing is going to be performed. We can change the type by using **set** commands along with the parameter names present in the routing seciton of the **README.md** file. Here, we have just used the default types :
  - ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/26815f32-ebe6-4e1b-85c7-8489a11119ab)
 
- Now, we can proceed for routing by using the command :
- ``` run_routing ```
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/2f76cb50-3e84-4c89-ad2e-68ecb64e5b1f)
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/ef89659e-681a-4d7b-aaca-7e7a41dcf3e6)
- SPEF(Standard Parasitic Exchange Format) Extraction is automatically done at the end of routing process as can be seen in the above image.
- So a **.spef** file is created :
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/9e2220c7-71fe-4008-98b2-d94dc1745ad7)


- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/15875958-051e-4b18-a1e0-3e2f591dcdb7)
- We can observe in the above image that there are no violations our design.

- Magic tool view of layout after routing is done:
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/d29f41fc-022c-4ad8-9c35-aaf3c5513d0d)
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/635e1f42-3e34-48cc-8a77-f5251ed18506)
- ![image](https://github.com/Shashank-raut1999/SoC/assets/165283786/6765277d-eaa0-4f40-89de-94d3232c15fa)




## Post-routing STA analysis :
- For STA analysis,
  - we need to create a new db(database) file because the **def** has changed.
  - we will use the **preroute_netlist**
  - we  need to read the **spef** file which contains all the information related to the parasitic capacitances.
 

 -  

  



   
     



 




