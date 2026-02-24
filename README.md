# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

<img width="1913" height="1078" alt="Screenshot 2026-02-24 102211" src="https://github.com/user-attachments/assets/4bcecd79-6a87-4647-a996-e2789c098614" />




◦ Verilog/VHDL Files (.v or .vhdl or .vhd)
**Design code**
```verilog
`timescale 1ns/1ps
module counter(
	input clk,
	input rst,
	input m,
	output reg [3:0] count);
always@(posedge clk or negedge rst)
begin
	if(!rst)
		count = 0;
	else if(m)
		count = count + 1;
	else
		count = count - 1;
end
endmodule

```
**Testbench**
```verilog
`timescale 1ns/1ps
module counter_tb;
reg clk,rst,m;
wire [3:0] count;
counter uut (clk,rst,m,count);
always #5 clk = ~clk;
initial
begin
	clk = 0;
	rst = 0; #5;
	rst = 1;
	m = 1; #160;
	m = 0;
	#160;
	$finish;
end
endmodule

```

<img width="1915" height="1077" alt="Screenshot 2026-02-24 102933" src="https://github.com/user-attachments/assets/e4c68be4-53cb-4f4f-b06e-961b20289f50" />
◦ SDC (Synopsis Design Constraint) File (.sdc)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

<img width="1919" height="1075" alt="image" src="https://github.com/user-attachments/assets/e41b8d5e-9b32-4263-9187-61911d1c6562" />


•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/ac640aa1-2721-4542-a8f5-7ba28786c52a" />

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/b4b64939-7110-4af2-a141-8fd4c70d593f" />


• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/8b1defd9-a9d9-49ac-8f3f-a0fe69cc683c" />


#### Area report:
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/4d23dbc2-6528-4a38-bba2-e38ffac6121e" />


#### Power Report:

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/0b9c7465-65a5-48e7-b5d6-07290d8f90df" />


#### Timing Report: 
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/a9fc6a7d-1153-458e-b71e-db156966fb1d" />

#### Netlist:

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/f7994699-9e2f-4154-a2cc-3e8f70f38dbc" />


#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





