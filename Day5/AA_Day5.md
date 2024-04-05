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

 




