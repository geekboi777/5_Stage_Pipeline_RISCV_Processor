RISC-V CPU 
==================
This is a 32-bit 5-stage pipelined RISC-V CPU that supports basic instructions and some vector arithmetic.  
The simulation was done by using iverilog and the synthesis of the design carried out using Cadence Design Compiler.
The proposed 5-stage pipeline processor has been tested on March 23', and all functions work correctly.

## Usage
To run this work, you will need a verilog compiler such as iverilog.  
- *iverilog testbench.v CPU.v* to run RTL simulation.
- *./a.out*
- This will generate a output text file and a .vcd file dumped in the testbench.
- The simulation waveform for the ouput can be seen via any waveform viewer (gtkwave / Scansion).

Since we can only read 8-bits at a time our instructions has to be broken down into 8-bits.  
>For example:  
>11111110_00000000_00000000_00000010_00010011 //addi $a0, $r0, 0  
>will be convert into:  
>00000000  
00000000  
00000010  
00010011  
in the instruction file

## Block Diagram

![Block Diagram](https://github.com/geekboi777/5_Stage_Pipeline_RISCV_Processor/blob/main/images/pic/block_diagram.png)

## Supported Instructions

* Basic Instructions
	* and 
	* or
	* add 
	* sub 
	* mul 
	* addi 
	* lw
	* sw
	* beq 
* Vector Arithmetic
	* VSUM (vector sum)
	* VSUB (vecotr subtraction)
	* VSM  (vector scalar multiplication)
	* VDP  (vector dot product)

## Supported Instruction Formats in RV32I ISA
![Instruction Formats in RISCV](https://github.com/geekboi777/5_Stage_Pipeline_RISCV_Processor/blob/main/images/pic/InstructionType.png)

## Hazard Detection 
* The proposed CPU supports Hazard Detection and resolves it using Data Forwarding in case of any data dependencies.

![Block Diagram](https://github.com/geekboi777/5_Stage_Pipeline_RISCV_Processor/blob/main/images/pic/DataPath.png)

- The Hazard Detection Unit and the Data Forwarding Unit generate the requisite Control Signals send to the respective MUX's and translated 
  along in the pipeline registers.


![Block Diagram](https://github.com/geekboi777/5_Stage_Pipeline_RISCV_Processor/blob/main/images/pic/hazard.png)

## Inputs/ Outputs to the CPU

![Block Diagram](https://github.com/geekboi777/5_Stage_Pipeline_RISCV_Processor/blob/main/images/pic/IO.png)

* Input Ports
	* Instr_i : Reads in instructions 8-bit at a time. It will take 4 cycles to read in one instruction.
	* D/R : If = 0, output value saved in DataMemory, else output value saved in register.
 	* addr: Address that you want to access, this input is 5-bit since size of both DataMemory and Register are 32.
 	* value_o_addr : The value saved in the memories are 32-bit, but our CPU can only ouput 8-bit at a time. Thus, we need to decide which 8-bit are to be read. For example, if value_o_addr = 0, the CPU will output the most signficant 8-bit in the address we assigned.
* Output Ports
	* value_o : Output value.
  	* is_positive : This is for handling overflow cases when doing vector sum.  When two 7-bit value are added together, it produces a 8-bit value and the value will be interpret as a negative value. When is_positive = 1, the 8-bit output should be interpret as a 8-bit positive value instead of a negative value.

## Simulation Results 

- The PC counter was set to go until the end of the set of instructions -> HEXA (F8) = DEC (248) indicating all instructions passed.
- The simulation waveform results as well as the *"Simulation Passed"* images are depicted.

<p align = "center">
  

<img width="853" alt="Screenshot 2023-03-05 at 8 18 55 PM" src="https://user-images.githubusercontent.com/85869106/222967713-7d124796-c5d0-455a-8916-4b04ea8b19f6.png">



![Block Diagram](https://github.com/geekboi777/5_Stage_Pipeline_RISCV_Processor/blob/main/images/pic/Simulation_results.png)

</p>

## Reference 

David A. Patterson and John L. Hennessy. 2017. Computer Organization and Design RISC-V Edition: The Hardware Software Interface (1st ed.). Morgan Kaufmann Publishers Inc., San Francisco, CA, USA.

