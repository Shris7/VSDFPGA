# VSDFPGA - Implementation of Mixed Signal SoC (RISC-V based Core + PLL) on FPGA
This repository explains the implementation of Mixed Signal SoC (RISC-V based Core + PLL) on FPGA.

# TABLE OF CONTENT
- INTRODUCTION
- TOOLS USED
- STEPS TO CONVERT TL-VERILOG TO SYSTEM VERILOG OR VERILOG   
- STEPS FOR RTL SIMULATION OF RVMYTH+PLL USING IVERILOG
  - OUTPUT
- SIMULATION,SYNTEHSIS AND IMPLEMENTATION USING VIVADO TOOL
- ACKNOWLEDGEMENT

# INTRODUCTION
__MYTH Core:__ https://github.com/shivanishah269/risc-v-core

__PLL:__ https://github.com/vsdip/rvmyth_avsdpll_interface

# Tools used:

__Makerchip:__  

[Makerchip](https://www.makerchip.com/) is a free web-based IDE as well as available as [makerchip-app](https://gitlab.com/rweda/makerchip-app), a virtual desktop application for developing high-quality integrated circuits. You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, and waveforms are tightly integrated.

__Icarus Verilog:__

[_Icarus Verilog_](http://iverilog.icarus.com/) is a Verilog simulation and synthesis tool.

__GTKWave:__

[GTKWave](http://gtkwave.sourceforge.net/) is a waveform viewer.

__Xilinx Vivado:__

[Xilixn Vivado](https://www.xilinx.com/support/university/vivado.html) provides complete SoC-strength, IP-centric and system-centric, next generation development environment. Currently, this project is done using Vivado HL Design Edition 2019.1.

# FPGA board used:
Zedboard Zynq-7000 ARM/FPGA SoC Development Board ([Product Link](https://www.avnet.com/wps/portal/us/products/avnet-boards/avnet-board-families/zedboard/))

![image](https://user-images.githubusercontent.com/15063738/125192735-f9c65b80-e266-11eb-9130-799199fa208a.png)


# Installtion and Overview of Sandpiper
* [SandPiper](https://www.redwoodeda.com/products) is a code generator that generates readable, well-structured, Verilog or SystemVerilog code from the given TL-Verilog code.
* [SandPiper SaaS Edition](https://pypi.org/project/sandpiper-saas/) runs as a microservice in the cloud to support easy open-source development. Install Sanpiper SaaS Edition for this project. 
* To run locally, SandPiper Education Edition can be requested from [RedwoodEDA](https://www.redwoodeda.com/products)

# STEPS TO CONVERT TL-VERILOG TO SYSTEM VERILOG OR VERILOG

1. Install Sandpiper SaaS (https://pypi.org/project/sandpiper-saas/)
2. `git clone https://github.com/shivanishah269/vsdfpga.git`
3. `cd vsdfpga/verilog`
4. ` sandpiper-saas -i rvmyth.tlv -o rvmyth.v --iArgs` -For TL-verilog to verilog
5. ` sandpiper-saas -i rvmyth.tlv -o rvmyth.sv --iArgs` -For TL-Verilog to system verilog
![image](https://user-images.githubusercontent.com/92938137/170981469-c5c55207-df85-4f36-aaca-16b40b425784.png)
![image](https://user-images.githubusercontent.com/92938137/170981490-44182e85-7109-4f55-8d83-45f887e1b343.png)
![image](https://user-images.githubusercontent.com/92938137/170981519-94f1926d-2ae5-45ec-a513-1ad8d8c8f1b5.png)


# STEPS FOR RTL SIMULATION OF RVMYTH+PLL USING IVERILOG

1. `iverilog rvmyth_pll_tb.v rvmyth_pll.v clk_gate.v`
2. `./a.out`
3. `gtkwave rvmyth_pll.vcd`
- Clk is the output from avsd_pll given as the input to rvmyth core whose output out[7:0] must run from 0 to 255 n 255 to 0

![image](https://user-images.githubusercontent.com/92938137/171102193-563ee609-eb4e-4778-9996-0d40f05c98ca.png)
![image](https://user-images.githubusercontent.com/92938137/171103308-589ca31e-748c-431b-9a49-f42af7683544.png)
![image](https://user-images.githubusercontent.com/92938137/171103284-84af17f7-ba15-4ef6-8c48-8b477f5159ee.png)
![image](https://user-images.githubusercontent.com/92938137/171103484-e9318592-1be4-4df1-92e2-0d3a49cface7.png)
- To convert a digital waveform to analog: data format-> analog -> step

![image](https://user-images.githubusercontent.com/92938137/171103577-d0f1cd02-d99e-444b-9b96-43b32b1697c2.png)

# SIMULATION,SYNTHESIS AND IMPLEMENTATION USING VIVADO TOOL(FPGA FLOW)
- FPGA FLOW

![image](https://user-images.githubusercontent.com/92938137/170983942-208374ee-95de-4c6b-b4a1-0501a1dd0dfd.png)
  - Input: reset and clk
  - Output: output clk (expected freq)
  - PLLE2 behaves as a multiplier
  - Output clk to the  evmyth core as input and also reset is given to is(core_reset)to  give us the output: dac_out
  - ILA(inter logic analyser) is waveform viewer.
 - STEP 1:
Launch Vivado and create a new project by selecting the required board

![image](https://user-images.githubusercontent.com/92938137/170984537-04165564-40fd-4d4a-a736-7961176048a8.png)
![image](https://user-images.githubusercontent.com/92938137/170984555-328533b3-d2dd-43cb-b8a1-8264bb97ef69.png)

- STEP 2:
Add rvmyth.v,clk_gate.v and top_Soc.v as design sources

![image](https://user-images.githubusercontent.com/92938137/171099736-383497d8-dd65-451d-b430-02a6fd3fcdb7.png)

- STEP 3:
Generate PLL and ILAs IP from the Xilinx IP catalog

![image](https://user-images.githubusercontent.com/92938137/171101342-22fc297f-f7b0-432d-adfe-17db25b266af.png)
![image](https://user-images.githubusercontent.com/92938137/171101562-2ab32b4a-6986-42b5-b322-2f568f4adb87.png)
![image](https://user-images.githubusercontent.com/92938137/171101704-805924e0-958e-46e9-a6d9-6cab1546d50f.png)
![image](https://user-images.githubusercontent.com/92938137/171101744-0a3f6fb7-d5dc-44af-b393-270302ef30b4.png)

- STEP 4:
Add the testbench top_Soc_tb.v

![image](https://user-images.githubusercontent.com/92938137/170985056-92bcb039-c66f-4485-a5bc-98616c34a95b.png)

- STEP 5:
Run the simulation

![image](https://user-images.githubusercontent.com/92938137/171100636-f334df49-66be-4237-80c5-2b8f5ad80ee4.png)
![image](https://user-images.githubusercontent.com/92938137/171100660-fd8d56d3-62c6-4da2-ac2e-8a7fbe649662.png)
Main_clock period: 30ns and Core_clk period: 10ns
  - Analog waveform:
  
![image](https://user-images.githubusercontent.com/92938137/171101102-ee1acc11-000a-497f-b6c2-c76d3c6e5c66.png)
- STEP 6:
Add the constraints file 

![image](https://user-images.githubusercontent.com/92938137/171098905-c790ae74-e216-4693-9d2a-892a11e9029a.png)
- STEP 7:
Run the RTL Synthesis
![image](https://user-images.githubusercontent.com/92938137/171099385-1fd607a8-70a4-4b9e-9da2-cb8b47876718.png)
![image](https://user-images.githubusercontent.com/92938137/171099488-d962aefb-496d-4845-8bba-780668e2aafc.png)

- STEP 8:
Run Impelementation and check for any setup and hold time violations
![image](https://user-images.githubusercontent.com/92938137/171099785-ef3cc4c1-b918-4983-8bf3-d447020e99b2.png)
![image](https://user-images.githubusercontent.com/92938137/171099847-d18aa59a-1a5f-45a5-aabc-3f4e03c8b2cb.png)
 - If we observe any hold time violation it is only because of false paths.
 ![image](https://user-images.githubusercontent.com/92938137/170987567-8ce4d89d-65c7-4dfb-bcaa-694c1b70a527.png)
 - Therefore set up false time constraints in the Constraints file.

` set_false_path -hold -from  [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to [get_pins uut3/inst/ila_core_inst/*/D] `

` set_false_path -hold -from [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to[get_pins uut3/inst/ila_core_inst/u_trig/U_TM/N_DDR_MODE.G_NMU[2].U_M/allx_typeA_match_detection.ltlib_v1_0_0_allx_typeA_inst/*/D] `
![image](https://user-images.githubusercontent.com/92938137/171098762-50520707-29d8-40cb-bf23-a6bfbb9f04fd.png)

- STEP 9:
Generate bit stream and program the FPGA Board

![image](https://user-images.githubusercontent.com/92938137/171098658-f304fe99-7340-4792-99fe-6c1eb09c1608.png)
This is the output you get after which u connect your borad and check for the waveform simulation(Which i do not have currently)
![image](https://user-images.githubusercontent.com/92938137/171098412-c398afc4-a120-4ae8-8b08-ea7450835f16.png)

# ACKNOWLEDGEMENT
- [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
- [Steve Hoover](https://github.com/stevehoover), Founder, Redwood EDA
- [Shivani Shah](https://github.com/shivanishah269),TA
