# <u> Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK </u>

## 1. Arduino Board
       
 ![alt text](Arduino-2.png)

  This is an arduino microcontroller board. The encircled area shows the chip(microprocessor) which is interfaced with other components of the board. The designing of this chip from abstract level all the way down to the fabrication is done by RTL to GDSll flow. 

## 2. Block diagram of a SoC(System on Chip)

 ![alt text](<Block diagram of processor.png>)    
     
This is the basic block diagram of the processor or SoC. It gives us an abstract idea of the different components present in a chip.
It consists of the main processor , SDRAM chip(memory) , ADC (analog to digital converters) , GPIOs(general purpose Input Output registers), communicatioin protocols(like I2C,UART ) etc.

###  Chip is connected to the package with the help of bond wires:
![alt text](<Chip & Package connection.png>)

## 3. How does a chip look like?
 ![alt text](<Macros and Foundry IP's.png>)
 ![alt text](<How chip looks like.png>)

 - Parts of a chip:
     - **PADS** : The blue squares around the edge are called PADS. These docks have connection points that allow the chip to send and receive messages with the outside world. These pads are connected to the pins of package  chip via bond wires.

     - **CORE** :The center black area is the brain of the chip, known as the CORE. The various components in this core work together to perform all the tasks the chip is designed for. 

     - **IP** : Each IP(Intellectual Property) block has a specific function. Chip designers can take these pre-made blocks and snap them together to build the exact functionality they need for their chip. ex: ADC, DAC, PLL, etc.

     - **Macro** : These blocks are blocks which are pure digital logic.



























# Basic Linux Commands
    - cd <name_of_folder>     : opens the particular folder
    - ls                      : lists the content of the folder
    - pwd                     : shows the present working directory
    - mkdir                   : to make a new directory






   















