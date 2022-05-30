# VSDFPGA
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

![image](https://user-images.githubusercontent.com/92938137/170981570-1879eb7b-43f8-4d36-9d8d-781b77b735f6.png)
![image](https://user-images.githubusercontent.com/92938137/170981634-54318d8b-0084-4c7a-9dca-743cfeb98290.png)
![image](https://user-images.githubusercontent.com/92938137/170981652-bdd2bfef-583d-4941-9bbb-3083ad4c26aa.png)
- To convert a digital waveform to analog: data format-> analog -> step

![image](https://user-images.githubusercontent.com/92938137/170981667-ed3fcdf1-8e8f-4560-9641-8e54259e1579.png)

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

![image](https://user-images.githubusercontent.com/92938137/170984715-2e07f334-e880-422a-abff-80e5d5538eb7.png)

- STEP 3:
Generate PLL and ILAs IP from the Xilinx IP catalog

![image](https://user-images.githubusercontent.com/92938137/170984916-bad0fccf-fb13-4144-87fe-df38d646ae61.png)
![image](https://user-images.githubusercontent.com/92938137/170984934-62de14d4-14eb-4b32-b9f8-3409aadbe3f2.png)
![image](https://user-images.githubusercontent.com/92938137/170984944-866ccdc2-1045-4fc7-84c2-9a5fdbc103c5.png)
![image](https://user-images.githubusercontent.com/92938137/170984960-22c5b8c4-c668-4498-9f02-72e475185d77.png)
![image](https://user-images.githubusercontent.com/92938137/170984971-a85e24af-369d-43dd-9e5a-a9ce93c8eb8d.png)

- STEP 4:
Add the testbench top_Soc_tb.v

![image](https://user-images.githubusercontent.com/92938137/170985056-92bcb039-c66f-4485-a5bc-98616c34a95b.png)

- STEP 5:
Run the simulation

![image](https://user-images.githubusercontent.com/92938137/170985184-7bbb22b5-11b0-438e-a899-cd1d9bb81f5d.png)
![image](https://user-images.githubusercontent.com/92938137/170985199-2bd61d22-361c-4e9b-9615-141a70ccd535.png)
Main_clock period: 30ns and Core_clk period: 10ns
  - Analog waveform:
  
![image](https://user-images.githubusercontent.com/92938137/170985217-69875058-2d6e-4fa2-9c37-702f5d1b4787.png)
![image](https://user-images.githubusercontent.com/92938137/170985232-c0fd5657-3c38-4fdc-9622-398f90cd225c.png)
 
- STEP 6:
Add the constraints file 

![image](https://user-images.githubusercontent.com/92938137/170985402-ff9ab3e0-8865-4af4-99dc-2648ce0acfc1.png)

- STEP 7:
Run the RTL Synthesis

![image](https://user-images.githubusercontent.com/92938137/170985484-f036f3af-3ff8-4342-811c-b2cff8f7ba86.png)

- STEP 8:
Run Impelementation and check for any setup and hold time violations

![image](https://user-images.githubusercontent.com/92938137/170985999-8a377ee2-d96f-4b02-9f16-e8a507e95857.png)
 - If we observe any hold time violation it is only because of false paths.
 ![image](https://user-images.githubusercontent.com/92938137/170987567-8ce4d89d-65c7-4dfb-bcaa-694c1b70a527.png)
 - Therefore set up false time constraints in the Constraints file.

` set_false_path -hold -from  [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to [get_pins uut3/inst/ila_core_inst/*/D] `

` set_false_path -hold -from [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to[get_pins uut3/inst/ila_core_inst/u_trig/U_TM/N_DDR_MODE.G_NMU[2].U_M/allx_typeA_match_detection.ltlib_v1_0_0_allx_typeA_inst/*/D] `
![image](https://user-images.githubusercontent.com/92938137/170985868-62aede05-7464-45a9-96c7-8e3a30d45ac7.png)

- STEP 9:
Generate bit stream and program the FPGA Board

![image](https://user-images.githubusercontent.com/92938137/170986557-0ff863b5-c476-4c35-a5a6-42e3a11d5177.png)
![image](https://user-images.githubusercontent.com/92938137/170986645-f66a5480-1110-480a-88ba-d17e92685284.png)
  - If the reset pin is high:
  
  ![image](https://user-images.githubusercontent.com/92938137/170987150-657d2c4a-181e-4ea8-bb86-e8436e7e371f.png)
  - If the reset pin is low:
  
  ![image](https://user-images.githubusercontent.com/92938137/170987222-e8373991-b053-4ed5-abb2-cf2049078482.png)
  - Analog form
  
  ![image](https://user-images.githubusercontent.com/92938137/170987350-e4c9a87c-1f4b-4e87-a048-0338107ccc3d.png)


# ACKNOWLEDGEMENT
- [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
- [Steve Hoover](https://github.com/stevehoover), Founder, Redwood EDA
- [Shivani Shah](https://github.com/shivanishah269),TA
