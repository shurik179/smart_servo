# smart_servo
Reverse-engineering Feetech/goBilda/REV dual mode servos

## Introduction
This repository documents my attempts to reverse-engineer protocol used by dual-mode servos 
made by Feetech, such as https://www.feetechrc.com/products?keyword=FT51

These are **not** serial servos - they use usual PWM control signal; however, they can be 
re-programmed using a dedicated programmer: https://www.feetechrc.com/FE-FRS1-C001.html
This programmer allows configuring them as continuous rotation servos and also setting (software) 
angle limits  (much less useful, in my opinion).

The same servos, with custom made geartrain and case, are also sold by REV robotics 
(https://www.revrobotics.com/rev-41-1097/) and goBilda (https://www.gobilda.com/standard-size-servos). 
Both of them are very popular in Firts Tech Challenge (FTC) robotics competition. 
Rev and goBilda also sell the programmers for them; [goBilda one](https://www.gobilda.com/servo-programmer-for-2000-series-dual-mode-servo/) looks to be just a rebranded version of Feetech 
programmer. Rev robotics [programmer](https://www.revrobotics.com/rev-31-1108/) seems to be custom-built. 

Same servo and programmer are also rebranded and sold by other vendors, e.g. https://www.studica.com/studica-robotics-brand/smart-robot-servo-programmer-2.

However, the protocol used for reprogramming the servos is not published (to the best of my knowledge). 
I wanted to experiment with them, so I tried to reverse engineer that protocol. 

Some of this is used on earlier work of @JJTech0130, see https://github.com/JJTech0130/smart-servo

## Methods 
Communication between the servo and REV programmer was captured and analyzed using scope.


## Findings 
The protocol seems to be based on Dynamixel's protocol version 1: https://emanual.robotis.com/docs/en/dxl/protocol1/. 

Namely, it uses half-duplex UART (8N1), and the general packet format is the same as Dynamixel's: 
2 header bytes, ID, length, instruction, parameters, checksum. The instruction codes are also the same as Dynamixel's 
(see https://emanual.robotis.com/docs/en/dxl/protocol1/#instruction) - at least, ping, read, and write. As for Dynamixel, each servo also has individual ID; by default it is set to 1, but can be reset if needed. 

However, it also differs from usual Dynamixel as follows:

- Baud rate is 78 000, which is rather non-standard

- The registers are very different. We can only guess hte register table.


Here is the google doc showing captured communication and suggested decoding:
https://docs.google.com/spreadsheets/d/1iJSD4zjrjVcDQU_t4Cwhj8StkT7XHumxqmx2ObuaDb4/edit?usp=sharing


