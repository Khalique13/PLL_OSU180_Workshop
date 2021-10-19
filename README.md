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

![blockdiagram](https://user-images.githubusercontent.com/80625515/137904145-fcaccd2a-aef9-4177-a52c-9dea5fee8af9.jpg)


<picture-block-diagram>

## Tools Used

- [Ngspice](http://ngspice.sourceforge.net/download.html)
- [eSim EDA tool](https://github.com/FOSSEE/eSim.git)
- [Magic](http://opencircuitdesign.com/magic/)

## SPICE Netlist

Inverter 



  ![inv-schematic](https://user-images.githubusercontent.com/80625515/137901210-03f1a56c-6673-41e7-8e31-224aa3378ecb.png)
  
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
  
 ![nand-schematic](https://user-images.githubusercontent.com/80625515/137901358-b05e4d9a-b78e-483d-b1a9-bcc804084aa1.png)
 

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

  
4-input NAND Gate
  
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

### Inverter Simulation Snap
  
 ![inv term](https://user-images.githubusercontent.com/80625515/137901545-cbca9b36-7a73-410a-884f-2830c7f443e9.png)

### Inverter Waveform Snap
  
  ![inv-wave](https://user-images.githubusercontent.com/80625515/137901695-07899fb0-24fb-4250-a8c6-8b9827291a50.png)

### NAND Simulation Snap
  
  ![nand-simulation](https://user-images.githubusercontent.com/80625515/137902275-223f81b5-08ad-453f-bf98-32f191f57f5b.png)

### NAND Waveform Snap
  
  ![nand-wave](https://user-images.githubusercontent.com/80625515/137902390-d8c48e12-ba58-4f1f-a68e-29015bb1ecbb.png)

### 3-input NAND Simulation Snap
  
  ![nand3-sim](https://user-images.githubusercontent.com/80625515/137902961-97bd7d56-5fd0-4115-9902-a62baa495b2d.png)

### 3-input NAND Waveform Snap
  
  ![nand3-wave](https://user-images.githubusercontent.com/80625515/137903046-a9706ad1-aaf1-4532-a815-ba0b2d31fbe5.png)

### 4-input NAND Simulation Snap
  
  ![nand4-sim](https://user-images.githubusercontent.com/80625515/137903691-2f12e153-fa7c-4540-ad92-597e37bf821e.png)

### 4-input NAND Waveform Snap
  
  ![nand4-wave](https://user-images.githubusercontent.com/80625515/137903717-afbe908c-b933-478b-989e-7f9c19a51de9.png)

### Phase Frequency Detector simulation Snap
  
  ![pfd-sim](https://user-images.githubusercontent.com/80625515/137904692-8a8cdbf1-8425-40d7-9a13-5185c8e5da77.png)

### Phase Frequency Detector Waveform Snap
  
  ![pfd-wave](https://user-images.githubusercontent.com/80625515/137904930-f66de482-099c-459e-a19e-b06e93f94a6a.png)

  
### Charge Pump with Phase Detector Waveform Snap  
  
  ![cf-wave](https://user-images.githubusercontent.com/80625515/137905602-90326688-0378-45da-af1b-5373efd5ebe6.png)

### Voltage Controlled Oscillator Simulation Snap
  
  ![vco-wave](https://user-images.githubusercontent.com/80625515/137906098-84e42774-461f-474f-b7e8-4145e516677b.png)

### Frequency-Divider Simulation Waveform Snap
  
  ![freq-div-wave](https://user-images.githubusercontent.com/80625515/137906528-60dbc888-4471-49eb-99c3-dc23b7377a8d.png)

### Final PLL Waveform Snap
  
  ![pll-wave](https://user-images.githubusercontent.com/80625515/137906946-32a9f03a-ff6c-4998-8401-620fe318e0fe.png)



  
## Layout

```
# Change direcory to access Layout files
cd PLL_OSU180_Workshop/Layout/`
```
  
### Standard cell Layout
  
  ```
  cd std_cells/
  magic -T ../SCN6M_SUBM.10.tech std_cells.mag
  ```
  
  ![std-cell-mag](https://user-images.githubusercontent.com/80625515/137917812-22e2e3e3-5782-4239-9b55-051d3824a517.png)

### TKCON terminal Snap
  
  ```
  extract all
  ext2spice chthresh 0
  ext2spice
  ```
  
  ![pfd-tkcon](https://user-images.githubusercontent.com/80625515/137917695-95ee56d7-a27a-4ba7-a7a8-a4f26a3a8a65.png)
  
### Phase Frequency Detector Layout
  
  ```
  cd pfd/
  magic -T ../SCN6M_SUBM.10.tech pfd.mag
  ```
  
  ![pfd-mag](https://user-images.githubusercontent.com/80625515/137917627-799b44f3-7f31-46fb-af34-42035c513df7.png)

 ### Voltage-Controlled Oscillator Layout 
   
  ```
  cd pfd/
  magic -T ../SCN6M_SUBM.10.tech vco101.mag
  ```
  
  ![vco-mag](https://user-images.githubusercontent.com/80625515/137917889-2fe4fa18-ff02-403a-bb62-cbb3d0e535f5.png)

 ### MUX Layout 
  
  ```
  cd mux21/
  magic -T ../SCN6M_SUBM.10.tech mux21.mag
  ```
  
  ![mux-mag](https://user-images.githubusercontent.com/80625515/137917430-483e3737-c145-4696-b835-feeddceb7fc8.png)
  
### Frequency Divider (/2) Layout
  
  ```
  cd freqdiv2/
  magic -T ../SCN6M_SUBM.10.tech freq_divider2.mag
  ```
  
  ![freq2-mag](https://user-images.githubusercontent.com/80625515/137917357-3d654c84-9caa-435b-b6f3-3651ba01382e.png)

### Frequency Divider (/8) Layout
  
  ```
  cd freqdiv8/
  magic -T ../SCN6M_SUBM.10.tech freq_divider8.mag
  ```
  
  ![freq8-mag](https://user-images.githubusercontent.com/80625515/137917403-f851cacc-1289-494a-89b8-bea4f9573670.png)
  
### Final PLL Layout
  
  
  ```
  cd PLL/ 
  magic -T ../SCN6M_SUBM.10.tech pll.mag
  ```
  #### Terminal Snap
  
  ![layout-term](https://user-images.githubusercontent.com/80625515/137942867-c3618b89-8af3-46e9-b63f-e1581f1c2150.png)

  
  ![pll-mag](https://user-images.githubusercontent.com/80625515/137917759-50bd236e-6cea-4d0b-9aa2-265e97cded4c.png)


## Post-Layout Simulation
  
  1. Phase Frequency Detector Waveform Snap
  
  ![pfd-post-sim](https://user-images.githubusercontent.com/80625515/137934471-e42e38d9-842f-4f78-ba8b-a7c8eee02135.png)

  2. Multiplexer waveform Snap
  
  ![mux-post-sim](https://user-images.githubusercontent.com/80625515/137934546-9994597b-cf52-41d9-a031-0c463c93dc05.png)

  3. Frequency Divider Simulation Snap (by 2)
  
  ![freqdiv-post-sim](https://user-images.githubusercontent.com/80625515/137934571-b4fde167-7ea9-4815-aaa5-7a3b0ad9e5cb.png)

  4. Frequency Divider Simulation Snap (by 8)

  ![freqdiv8-post-sim](https://user-images.githubusercontent.com/80625515/137935295-8d2500f3-b760-4024-b9b1-8813bb3e9e53.png)

  5. Voltage Controlled Oscillator Simulation Snap
  
 ![vco-sim-post](https://user-images.githubusercontent.com/80625515/137947386-4c38c1e9-5661-42f3-ae1b-6053f4eb142e.png)


  6. PLL Final Simulation Snap
  
  ![pllv2-vcp-post](https://user-images.githubusercontent.com/80625515/137935995-4275daaf-4c76-46a8-8682-f4f7da901179.png)

  ![pllv2-post](https://user-images.githubusercontent.com/80625515/137936016-5a620799-cc41-4106-b305-d59efd6709d5.png) 
  
  ![pllv2-2post](https://user-images.githubusercontent.com/80625515/137936071-67e25448-5ad3-44ad-a3a9-e122e7ac926e.png)
  
  ![pllv2-3-post](https://user-images.githubusercontent.com/80625515/137936101-2ab247e7-b794-4370-889f-7979d1247736.png)

  
## References

- https://github.com/parasgidd/avsdpll_3v3.git

## Author

- Mohammad Khalique khan, Bachelor of Technology (ECE), Aliah University, gjkhalique@gmail.com

## Acknowledgement

- Paras Gidd, M.Tech.( Microelectronics ), Manipal Institute of Technology,(MAHE), parasgidd@gmail.com
- Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd. - kunalpghosh@gmail.com
