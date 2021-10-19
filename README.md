# On-Chip Clock Multiplier (PLL) using OSU180 

![vsdopentutorial](https://user-images.githubusercontent.com/80625515/137896513-0cc9dace-0454-43a7-be28-a6ce883cf631.png)


## Table of Contents

- [Introduction](#introduction)
- [Tools Used](#tools-used)
- [Spice Netlist](#spice-netlist)
- [Pre-Layout Simulation](#pre-layout-simulation)
- [Layout](#layout)
- [Post-Layout Simulation](#post-layout-simulation)
- [References](#references)
- [Author](#author)
- [Acknowledgement](#acknowledgement)

## Introduction

This repository focuses on workshop on PLL (Phase Locked Loop) also known as On-Chip Clock Multiplier. In this workshop we are going to cover a brief description on what is PLL and it's each block. We will also see pre-layout and post-layout simulations of each block of a PLL.

The clock generator is one of the most crucial part in synchronous processor & probably most susceptible after power lines which can cause failure of entire circuitry if not designed properly. It is widely used in radio frequency or wireless applications.

In view of its usefulness, the phase locked loop or PLL is found in many wireless, radio, and general electronic items from mobile phones to broadcast radios, televisions to Wi-Fi routers, walkie talkie radios to professional communications systems etc.

Here is a block diagram of PLL.

<picture-block-diagram>

## Tools Used

- [Ngspice](http://ngspice.sourceforge.net/download.html)
- [eSim EDA tool](https://github.com/FOSSEE/eSim.git)
- [Magic](http://opencircuitdesign.com/magic/)

## SPICE Netlist

Inverter 

```
****************************
*Inverter
***************************

.include osu018.lib

M1 out in GND GND nfet l=180n w=180n
M2 VDD in out VDD pfet l=180n w=360n

V1 in 0 PULSE 0 1.8 10p 50p 50p 100n 200n
v2 VDD 0 1.8


.control
tran 0.01ns 400ns
plot v(in)+2 v(out)
.endc

.end

```

2-input NAND Gate

```
****************************
*2 input nand
***************************

.include osu018.lib

M1 out in1 N001 N001 nfet l=180n w=180n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 GND GND nfet l=180n w=360n


V1 in1 0 PULSE 0 1.8 10p 50p 50p 100n 200n
V2 in2 0 PULSE 0 1.8 10p 50p 50p 50n 100n
V3 VDD 0 1.8


.control
tran .1ns 200n
plot V(in1)+4 V(in2)+2 V(out)
.endc


.end

```

3-input NAND Gate

```
****************************
*3 input nand
***************************


.include osu018.lib

M1 out in1 N001 N001 nfet l=180n w=540n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=540n
M5 N002 in3 GND GND nfet l=180n w=540n
M6 VDD in3 out VDD pfet l=180n w=360n



V1 in1 0 PULSE 0 1.8 10p 50p 50p 100n 200n
V2 in2 0 PULSE 0 1.8 10p 50p 50p 50n 100n
V3 in3 0 PULSE 0 1.8 10p 50p 50p 25n 50n
V4 VDD 0 1.8


.control
tran .1ns 200n
plot V(in1)+6 V(in2)+4 V(in3)+2 V(out)
.endc


.end

```

```
****************************
*4 input nand
***************************

.include osu018.lib

M1 out in1 N001 N001 nfet l=180n w=720n
M2 VDD in2 out VDD pfet l=180n w=360n
M3 VDD in1 out VDD pfet l=180n w=360n
M4 N001 in2 N002 N002 nfet l=180n w=720n
M5 N002 in3 N003 N003 nfet l=180n w=720n
M6 VDD in3 out VDD pfet l=180n w=360n
M7 N003 in4 GND GND nfet l=180n w=720n
M8 VDD in4 out VDD pfet l=180n w=360n

V1 in1 0 PULSE 0 1.8 10p 50p 50p 100n 200n
V2 in2 0 PULSE 0 1.8 10p 50p 50p 50n 100n
V3 in3 0 PULSE 0 1.8 10p 50p 50p 25n 50n
V4 in4 0 PULSE 0 1.8 10p 50p 50p 12.5n 25n
V5 VDD 0 1.8


.control
tran .1ns 200n
plot V(in1)+8 V(in2)+6 V(in3)+4 V(in4)+2 V(out)
.endc


.end

```

## Pre-Layout Simulation



## Layout

## Post-Layout Simulation

## References

- https://github.com/parasgidd/avsdpll_3v3.git

## Author

- Mohammad Khalique khan, Bachelor of Technology (ECE), Aliah University, gjkhalique@gmail.com

## Acknowledgement

- Paras Gidd, M.Tech.( Microelectronics ), Manipal Institute of Technology,(MAHE), parasgidd@gmail.com
- Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd. - kunalpghosh@gmail.com
