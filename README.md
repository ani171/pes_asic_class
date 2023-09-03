# VLSI Physical Design for ASIC's
<details>
<summary>Installation of RISC-V Toolchain</summary>

https://github.com/kunalg123/riscv_workshop_collaterals/blob/master/run.sh

- Execute the commands in run.sh
- See that the gcc version on your system is of version 12
If not an error of this kind be found:
![image](https://github.com/ani171/pes_asic_class/assets/97838595/32c1802e-9b05-4248-b3b3-f06a9f6188b7)
```
sudo apt upgrade
sudo apt install build-essential
sudo apt -y install gcc-12 g++-12
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 12
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
gcc --version; g++ --version
```
The above commands will update your gcc to version 12. To check for successful installation run the below command and the output will be shown as depicted below
```
riscv64-unknown-elf-gcc --version
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c7b66c97-cca2-4393-983d-ea63087f87e4)
</details>

## Day 1-Introduction  to RISC-V ISA and GNU Compiler Chain

<details>
<summary>Instruction Set Architecture</summary>
Instruction set architecture or computer architecture is an abstract model of the computer that defines how the CPU is controlled by the software. It acts as an interface between languages like C, C++, Java, and the hardware. The type of instructions depends on the type of hardware.
</details>

<details> 
<summary> From Apps to Hardware </summary>
Application software ---> System software ---> Hardware <br>

- System Software converts application software into binary language
- It has three major parts:
	- Operating system
	- Compiler
	- Assembler
- The operating system acts on small functions present in C, C++, Java, or any other language codes and gives it to the Compiler which in turn generates the .exe file which has all the Instructions. The .exe file is fed into the assembler, which generates the Machine Language code through which hardware can be implemented
	
### Type of Instructions
- Pseudo Instructions
- Base Integer Instructions(RV64I)
- Multiply Extension(RV64M)
- Single and Double precision floating point Extension(RV64F and RV64D)

#### Application Binary Interface
These are the keywords through which programmers can access the registers of RISC-V. They are basically the **System functions** associated with the RISC-V registers
</details>

<details> 
<summary>Labwork for RISC-V Toolchain </summary>
	
Write a program to calculate the sum of numbers from 1 to n
```
#include <stdio.h>
int main(){
  int i,sum=0,n=10;
  for(i=1;i<=n;i++){
    sum=sum+i;
  }
printf("Sum of numbers from 1 to %d is %d",n,sum);
}
```
- To execute the above type in the following commands
```
gcc sum.c
./a.out
```
- To display the code present in the .c file
```
cat sum.c
```
- To compile the C language code using RISC-V Compiler
  1. O1 optimization
```
riscv64-unknown-elf-gcc -O1 -mabi=lb64 -march=rv64i -o sum.o sum.c
```
  2. Ofast optimization
```
riscv64-unknown-elf-gcc -Ofast -mabi=lb64 -march=rv64i -o sum.o sum.c
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1435199c-922a-48e0-9bb8-3e160ff67580)
If there is an error found as above, use the following commands and then re-run the compilation command
```
vim ~/.bashrc
export PATH=~/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH
export PATH=~/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH
```
- To view the assembly-level code for the C program, which is compiled using RISC-V
```
riscv64-unknown-elf-objdump -d sum.o
riscv64-unknown-elf-objdump -d sum.o | less
```
- Output using O1 Optimization
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2af6308f-5950-4241-bb34-4c366e2148b2)
- Output using Ofast optimization
![image](https://github.com/ani171/pes_asic_class/assets/97838595/a3a17aff-e47a-4db7-ad7a-350895097c8f)

#### Spike stimulation and debugging
```
spike pk sum.o
spike -d pk sum.o
```
The above command is used for debugging
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2750abef-9d28-4641-bab4-f8c1c0d43309)
- Click on ENTER to show the first line and ENTER to show successive lines
- Click on q to quit the debug process
</details>

<details>
<summary>Integer Number Representation</summary>
- Unsigned numbers: are just like integers but they don't have a + or - sign associated with them.<br>
  Range: [0, (2^n)-1 ]<br>
- Signed numbers: These are a set of both positive and negative numbers
  Range : [0, 2^(n-1)-1] to [-1 to 2^(n-1)] <br>
  To represent negative numbers in binary 2's complement methodology is used.
</details>
<details>
	
<summary>Lab</summary>

- Write a C program that shows the maximum and minimum values of "n" bit unsigned numbers
  Considering n=64 here
```
#include <stdio.h>
#include <math.h>
int main(){
  int n=64;
	unsigned long long int max = (unsigned long long int) (pow(2,n) -1);
	unsigned long long int min = (unsigned long long int) (pow(2,n) *(-1));
	printf("Minimum value is %llu\n",min);
	printf("Maximum value is %llu\n",max);
	return 0;
}
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c627c451-4da5-4039-aec1-d9cfb5ac89d3)

- Write a C program that shows the maximum and minimum values of "n" bit signed numbers
```
#include <stdio.h>
#include <math.h>

int main(){
	int n=64;
	long long int max = (long long int) (pow(2,n-1) -1);
	long long int min = (long long int) (pow(2,n-1) *(-1));
	printf("Minimum value is %lld\n",min);
	printf("Max value is %lld\n",max);
	return 0;
}
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/7fad8130-2427-4101-93ed-4d95eba6a14b)
</details>

## Day 2 - Introduction to ABI and Basic Verification Flow

<details>
<summary>Application Binary Interface</summary>
- An Application Binary Interface is the interface between two binary program module programs allowing them to work together. It defines the interface between two software components or systems that are written in different programming languages, compiled by different compilers, or running on different hardware architectures.<br>
- ABI defines how your code is stored inside the library file so that any program using your library can locate the desired function and execute it.
</details>
<details>

<summary>Double Words- Memory allocation</summary>
Architecture can also be divided into two types based on the process of loading memory. Memory can be loaded in two ways <br>
1. Little Endian: Here, the least significant byte is at the lowest memory address, and the most significant byte is at the highest memory address.<br>
2. Big Endian: Here, the most significant byte is at the lowest memory address, and the least significant byte is at the highest memory address.

![image](https://github.com/ani171/pes_asic_class/assets/97838595/2745c597-e8d1-4ff9-8a52-a7001e178130)
</details>
<details>
<summary>Load, Add, and Store Instructions</summary>

- Load Instruction
Considering the instruction ```ld x8,16(x23)```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/775631b4-8e05-4194-ad01-27c17ee5ccda)

  - Here ld represents the loading of double-word
  - x8 is the destination register
  - x23 is the source register which has the base address
  - 16 is the offset which is added to the base address
  - The base address and the offset are added to generate the Physical Address
  - The content of the physical address is accessed and now loaded to the destination register i.e. x8 in here

- Add Instruction
![image](https://github.com/ani171/pes_asic_class/assets/97838595/afc0bd1f-49a2-4d6b-8b4a-77f1770cf20a)
Instruction: ``` add x8,x24,x8```
  - Here add represents a normal adding arithmetic operation
  - x8 is the destination register
  - x24 is the source register 1
  - x8 is the source register 2
</details>
<details>
	
<summary>32 Registers and their general ABI Names</summary>

Through the ABI names, we reserve some of these registers for certain purposes
![image](https://github.com/ani171/pes_asic_class/assets/97838595/3c47998d-33da-4a49-badd-6f3d99dafb29)
</details>
<details>
<summary>Labwork</summary>

Using ABI Function calls (re-writing C program using ASM language)
C program- .c file
```
#include <stdio.h>

extern int load(int x, int y);

int main()
{
  int result = 0;
  int count = 9;
  result = load(0x0, count+1);
  printf("Sum of numbers from 1 to 9 is %d\n", result);
}
```
Assembly file - .s file
```
.section .text
.global load
.type load, @function

load:

add a4, a0, zero
add a2, a0, a1
add a3, a0, zero

loop:

add a4, a3, a4
addi a3, a3, 1
blt a3, a2, loop
add a0, a4, zero
ret
```
Compile the above using
```
riscv64-unknown-elf-gcc -Ofat -mabi=lp64 -march=rv64i -o custom1to9.o custom1to9.c load.S
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f2edf713-44dd-49fc-92c8-868112046471)
To get the assembly-level code

```
riscv64-unknown-elf-objdump -d custom1to9.o |less
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/a3b0636e-e4be-47e1-862b-ee46dffbd770)
</details>
<details>
<summary>RISC-V CPU (PICORV-32)</summary>
	
PicoRV-32 is a size-optimized RISC-V CPU Core that implements the RISC-V RV32IMC Instruction Set.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/d06a8c6d-7546-4cae-b99b-0e137a259294)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/3f26df7d-57bd-4d96-ae8d-b56f851bce32)
</details>

## RTL Design using SKY130 Technology

## Day 1 - Iverilog Design and Testbench
<details>
<summary>Introduction</summary>
- RTL Design is checked for adherence to the spec by simulating the design
- Simulator (Iverlog in here) is a tool used for checking the design ( set of Verilog codes in here)
- Working of Simulator: The Simulator looks for changes in the input signal and evaluates the output. If the input values are changed, only then they are reflected in the changes in output values
</details>
<details>
<summary>Testbench</summary> 
	
- Testbench is an environment used to verify the correctness or soundness of a design or model.
- TestBench does not have any primary inputs or outputs
![image](https://github.com/ani171/pes_asic_class/assets/97838595/726e3f71-c496-464b-9919-841548fe23de)
</details>
<details>
<summary>Iverilog Based Simulation flow</summary>
	
- vcd file: A Value Change Dump file stores all the information about value changes in the simulator
- GTKwave: It is a software, used as a simulation tool to verify the Verilog design code through a testbench.
 	```
	sudo apt install gtkwave
  	```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c95028f6-66a5-49a6-a165-3a7bdd3310ba)
</details>
<details>
<summary>Labs using Iverilog and GTKwave</summary>
	
```
mkdir VLSI
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/ade39e4f-d2ca-447f-9121-0a3d6d4f4dd1)
- The library files are stored in my_lib
- All the Verilog models of the standard cells are present in verilog_model
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c18d9a86-b938-4f9c-a201-9b7c05d570f4)
- verilog_files has all the source files and testbench files of the required standard cells ( has the design files)
- for every file for example good_mux.v file there is a **tb_**good_mux.v file. We can see a one-to-one mapping between the Verilog Design file and it's testbench file
- Load both the design source file and testbench file into the verilog simulator (iverilog in here) `iverilog good_mux.v tb_good_mux.v`.
- An `a.out` file is created.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/5e2251e6-bb07-4790-aa2c-28ff9905ba71)
- On executing this file `./a.out` an VCD file is dumped out of the simulator
- Loading the file into GTKwave using the command `gtkwave tb_good_mux.vcd`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/227e9d60-e736-40b9-ba23-7ca7f51c0845)<br>
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f77735ab-5559-47e7-b96f-d399c9a861e5)
<br>
- For looking into the file structure `gvim tb_good_mux.v -o good_mux.v`

![image](https://github.com/ani171/pes_asic_class/assets/97838595/b7f938c7-7768-43fb-9aee-45a600758255)

</details>
<details>
<summary>Logic Synthesis</summary>
	
- RTL Design: Behavioural representation of the required specification
-  .lib: Collection of logical modules
-  Synthesizer: Tool used for converting RTL to netlist
-  Netlist: Representation of design in the form of standard cells present in .lib
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2150b1cf-e724-42a7-ac34-4879fcc3c298)
<br>
**Need of different flavors of gates**
- Combinational logic (Propagation Delay) determines the maximum speed of operation of the digital logic circuit
- T_clock > T_pd + T_cq + T_setup
- To achieve maximum clock frequency, for better performance the delays should be as minimum as possible. This would mean that only faster cells are sufficient
- But to ensure that there are no hold delay issues, gates are required to work slowly, creating a contractionary requirement
- Therefore, for better performance fast cells are used  while to avoid hold-time delays slow cells are used.
<br>
**Fast Cells v/s Slow Cells**
- Fast Cells
	- Fast cells use wider transistors to enable higher current carrying capacity. This allows for quicker charging and discharging of capacitive loads, resulting in faster signal transitions.
 	- Wider transistors generally consume more power compared to narrower ones due to the increased current flow and larger gate capacitance.
	- While faster cells offer improved performance, they might have larger silicon area requirements due to the increased number of transistors. Additionally, they might be more susceptible to issues like noise and power consumption.

- Slow Cells
	- Slow cells use narrower transistors to reduce power consumption and minimize power dissipation.
	- Narrower transistors consume less power due to their lower current carrying capacity and reduced gate capacitance.
	- While slower cells consume less power, they might operate at lower clock frequencies and have longer signal propagation delays. This can impact their ability to process data quickly.

- The choice between faster and slower cells depends on the specific requirements of the digital logic circuit's application. Designers often need to strike a balance between performance, power consumption, and area constraints.
</details>
<details>
<summary>Yosys</summary>
	
- Yosys is a framework for Verilog RTL Synthesis.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/09ce7b51-d138-40e2-9716-923632c49efc)
<br>

**Installation of Yosys**
```
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make
sudo apt-get update
sudo apt-get install build-essential clang bison flex  libreadline-dev gawk tcl-dev libffi-dev git  graphviz xdot pkg-config python3 libboost-system-dev libboost-python-dev libboost-filesystem-dev zlib1g-dev
make config-gcc
make
sudo make install
```
- To invoke Yosys
```
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/0ae756d0-5cd1-46b6-8ce6-ee3ee438f4bb)

- To read the library
`read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/8e4c9558-68da-469c-a4cf-d3d8d83ea1b5)

- To read the design file
`read_verilog good_mux.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/4def5432-1f87-4411-9955-6b88da86c445)

- For synthesizing the module
`synth -top good_mux`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/253347a1-65f9-4889-8e2d-e5ab7c8aa950)

- For realizing the logic in the verilog file
`abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/3d8047da-02ef-4e02-a837-a7e79a434090)
	- Number of input signals, output signals and internal signals can be known through above

- To get the graphical version of the realized logic `show`
	-The mux is completely realised in the form of sky130 library cells.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/32f85e16-8411-46c0-a3ab-d64b2658931a)

- To write netlist
```
write_verilog good_mux_netlist.v
!gvim good_mux_netlist.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/98495445-304b-4f7f-9d30-598f7f1fa380)
<br>
![image](https://github.com/ani171/pes_asic_class/assets/97838595/87cf80a8-b7e4-4fd7-8e50-16237068ebec)

- To get a simplified version
```
write_verilog -noattr good_mux_netlist.v
!gvim good_mux_netlist.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/8a32b3ef-aa6f-4e2e-a29f-2f3658b909f8)
<br>
![image](https://github.com/ani171/pes_asic_class/assets/97838595/4387c026-5906-4dee-b7bd-51c4bf0c3f44)
</details>

## Day 2 - Timing libs, Hierarchial and flat synthesis, and efficient flop coding styles

<details>
<summary>Timing Dot libs</summary>

- .lib files
	- To view the contents of .lib file
 	`gvim ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c1f769f2-57e7-45ca-9863-6f028774fe6c)
	- .lib files are used in digital circuit design to provide detailed information about the timing, power, and other characteristics of standard cells. 
	- In the first line i.e. library("sky130_fd_sc_hd__tt_025C_1v80")
 		- Libraries can be slow, fast, or typical. Here `tt` stands for typical. The term typical (abbreviated as "tt") refers to the standard or average performance characteristics of a component or circuit under normal operating conditions.
		- `025C` refers to the temperature at which the library's characteristics are specified.
		- `1v80` is a representation of the supply voltage in volts. This voltage level serves as a reference point for understanding the circuit's behavior and performance under that specific operating voltage.
		- `sc` represents standard cells signifies that the library contains standard cell information and characteristics for use in circuit design.
</details>

<details>
<summary>Hierarchy v/s Flat Synthesis</summary>
	
For synthesizing the module we used the command, `synth -top good_mux`. Now to know what type of synthesis is taking place `mutiple_modules.v` module is used.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1c920aeb-cb77-415c-ae42-eefa2a3c9b0b)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/39fa3e97-3be6-427d-82e1-7a4da0dee4c9)

- There are two sub-modules
	1. AND Gate
	2. OR Gate
- The module `multiple_modules`, is instantiated sub-module 1 and 2
- As per the module, the gate-level logic would be as below
![image](https://github.com/ani171/pes_asic_class/assets/97838595/55e09864-a78c-40f3-9d73-c0924cfe4901)
- But after synthesis
```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.v
read_verilog mutiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f5c349d1-06ad-4e9f-90c4-ca277e2f13f9)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/8807189d-1848-41be-a47c-76bf29e67f8e)
- This above synthesized is of hierarchical form
- To get the netlist
```
write_verilog -noattr multiple_modules_hier.v
!gvim multiple_modules_hier.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/baa7f6be-4981-41a6-885f-3eb27e441929)
- A NAND Implementation is seen, here.
<br>
Stacked PMOS Circuits<br>
- Stacked PMOS NOR requires multiple transistors to be stacked vertically, which leads to a more complex manufacturing process. This complexity can result in lower yields and higher manufacturing costs.<br>
- The stacked PMOS architecture tends to occupy more space compared to other memory cell configurations. This larger cell size translates to a lower storage density<br>
- Due to its larger cell size, stacked PMOS NOR flash has a lower bit density, meaning you can store fewer bits in the same area compared to other architectures like NAND<br>
-  PMOS transistors are constructed using a p-type semiconductor for the channel region, and their carrier mobility tends to be lower than that of NMOS transistors, which use an n-type semiconductor for the channel. Due to the lower carrier mobility of PMOS transistors compared to NMOS transistors, stacked PMOS NOR flash memory cells might experience slower switching speeds, contributing to slower overall memory performance and longer access times.<br>

```
write_verilog -noattr multiple_modules_flat.v
!gvim multiple_modules_flat.v
```

![image](https://github.com/ani171/pes_asic_class/assets/97838595/3f303820-31c7-484c-9725-2d80f1529828)

- Directly the AND and OR Gate are instantiated.<br>

##### Hierarchial Synthesis
In hierarchical synthesis, the design is organized into a hierarchy of modules, with each module representing a functional block or sub-component. Each module is synthesized independently, and then these synthesized modules are connected together to form the complete design.<br>
- Advantages<br>
	- Encourages modular design, making it easier to manage and maintain complex designs.<br>
	- Supports the reuse of modules, as synthesized blocks can be used in multiple designs.<br>
	- Enables concurrent development and optimization of different modules.<br>
	- Can help manage complexity and reduce the size of intermediate files.<br>
- Disadvantages<br>
	- Introduces the challenge of correctly integrating modules and ensuring proper connectivity.<br>
	- Some high-level optimizations might be more challenging due to module-level synthesis.<br>

##### Flat Synthesis
In flat synthesis, the entire design is treated as a single, monolithic unit. This means that the entire design hierarchy, including all sub-modules, is flattened into a single-level representation. All optimizations, logic synthesis, and technology mapping are performed on this single-level design.<br>
- Advantages<br>
	- Simplifies the synthesis process, as the entire design is treated as a single unit.<br>
	- Can lead to high-level optimizations across the entire design.<br>
- Disadvantages<br>
	- Can result in large intermediate files and complex optimization problems.<br>
	- Limited ability to reuse common logic structures across different parts of the design.<br>
 	- Can lead to inefficient use of resources if the design is very large and complex.<br>
<br>
In practice, a combination of both flat and hierarchical synthesis is often used. Hierarchical synthesis is employed for managing the complexity of large designs, and then certain modules might be synthesized flat to achieve specific optimizations.
</details>

<details>
<summary>Flop Coding Styles</summary>

- A flip-flop is a bistable multivibrator circuit element that can store one bit of data. It has two stable states and can be used to represent binary information.
	


#### Glitches
Glitches are unwanted and unpredictable transitions in digital circuits that can occur due to variations in signal propagation delays.
- Reasons for Glitches
1.  Different gates have different propagation delays, and these delays can lead to temporary imbalances in signal timing. If inputs to different gates change at slightly different times, it can result in momentary glitches in the output.
2. Signals may take different path lengths to reach different gates. Longer paths can introduce larger propagation delays, potentially causing timing mismatches and glitches.
3. Race conditions occur when two or more signals arrive at a gate at nearly the same time, and the output of the gate depends on which signal arrives first. This can lead to unpredictable temporary output values before the circuit settles into a stable state.

#### Requirement of flops
- Flip-flops are used in sequential circuits to store data and create a controlled timing mechanism. They can help eliminate glitches that may occur in combinational circuits

#### Asynchronous Reset D flip-flop
- The asynchronous reset feature allows the user to reset the flip-flop's state to a specific value, irrespective of the clock signal
- When the reset input is not active i.e. 0, the flip-flop operates as a standard D flip-flop, capturing the value at the D input on the rising edge of the clock.
- When the reset input is active i.e. 1, the flip-flop's output is forced to 0 regardless of the clock or D input.

`!gvim dff_asyncres.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/55764322-f506-49c5-a02e-84db09adda94)

Simulation
```
cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/76fd7567-c4a4-4fc1-bc13-e1e98caf3559)

Synthesis
```
cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show dff_asyncres
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1a6636d6-39e5-4763-887d-f74b70f36eca)

#### Asynchronous Set D flip-flop
- When the set is high, the output of the flip-flop is forced to 1, irrespective of the clock signal.
- When the set is low, the flip-flop operates as a standard D flip-flop, capturing the value at the D input on the rising edge of the clock
`!gvim dff_async_set.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/7cd21b5b-4d4f-49ff-8c6c-c6019d6da147)

Simulation
```
cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
iverilog dff_asyncres_set.v tb_dff_asyncres_set.v
./a.out
gtkwave tb_dff_asyncres_set.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2e7a8b13-7845-43cb-b406-2f36d0124356)

Synthesis
```
cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres_set.v
synth -top dff_asyncres_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show dff_asyncres_set
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/43612641-12a4-4de3-b146-86a7deae4a03)

#### Synchronous Reset D flip-flop
- A synchronous reset D flip-flop is a type of flip-flop that includes a reset input that is synchronized with the clock signal. This means that the reset input will only take effect on a specific clock edge, typically the rising or falling edge of the clock.
- During normal operation, when the reset input is not asserted, the flip-flop operates like a standard D flip-flop
- When the reset input is asserted (active), the flip-flop's output is forced to 0
`!gvim dff_syncres.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1ea4756b-dcdc-46e4-a334-a319bac87e19)

Simulation
```
cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
iverilog dff_asyncres_set.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/b2f29e47-ea44-4fc4-8dd2-3d19b007d47c)

Synthesis
```
cd vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show dff_syncres
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/a9129e69-7c7c-4377-89fa-ed5e0dbf807d)

</details>

<details>
<summary>Optimizations</summary>
1. 
	
`gvim mult_2.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/997d5429-4c1c-40d4-80c9-89b1a16f737d)

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mult2
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/13e49f3a-741a-49ec-aa0d-f923b703a11b)

```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/0a31bf4c-b20a-42de-9d39-1dcac5e2dcbd)

```
write_verilog -noattr mul2_netlist.v
!gvim mul2_netlist.v
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/003a3205-3f1c-4da2-985f-aa233643f613)

2. 
`gvim mult_8.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c2b05990-dec3-40d4-b9cb-758a4192e052)

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mult8
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c02573ca-b08a-4c45-a217-464a82b893cd)
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show 
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/0f0964a4-4a7a-4657-aefd-0d22c244bcc3)

</details>


## Day 3 - Combinational and Sequential Optimizations

<details>
<summary>Combinational Optimization</summary>

- Combinational optimization deals with finding the best solution from a finite set of possible solutions.
- It focuses on finding the best possible solution from a finite set of options for problems that involve discrete variables and have no inherent notion of time.
- Two methods of computational optimization are
	1.  Constant Propagation is a method of optimization that involves identifying and replacing variables with their constant values if they can be determined at compile-time. This optimization helps reduce the execution time of programs by avoiding redundant computations and simplifying expressions.
	2.  Boolean logic optimization is a process of simplifying and improving logical expressions in Boolean algebra. It aims to simplify Boolean expressions or logic circuits by reducing the number of terms, literals, and gates required to implement a given logical function.

### opt_check

`!gvim opt_check.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f700bab9-eb63-47e5-b4f4-faaa6906346f)
- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![image](https://github.com/ani171/pes_asic_class/assets/97838595/27e22aea-f124-4982-bc60-ef63e0a8f82d)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/d715d44f-b4f3-475b-83ce-21337f9a1397)

### opt_check2

`!gvim opt_check2.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/dc6f4bfa-ae82-4002-97f2-395400232d8e)
- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check2.v
synth -top opt_check2
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/70ff1896-d3dd-492e-bcbf-58b802af1159)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/fbe2296a-4615-4e21-bf8f-937bb86bbc86)

### opt_check3

`!gvim opt_check3.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/0a240573-20a6-4b9b-bf9e-27665feaf4b7)
- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check3.v
synth -top opt_check3
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/612b6960-ef18-489d-97f8-62ed4a6cc6c2)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/d9340801-1e8b-495c-b5eb-4dbb451305db)

### opt_check4

`!gvim opt_check4.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/799e8aab-a202-49b4-884a-840a00914f0b)
- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog opt_check4.v
synth -top opt_check4
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/b422fd93-93b2-40d1-a4bd-11c55ca67e90)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/5ee0a6a1-0e72-407f-b44d-1707c617edbe)

### multiple_module_opt

`!gvim multiple_module_opt.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f41e17c1-bd50-44ee-88f7-1139d51cd7ae)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
flatten
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/150f827a-cdf3-4d8a-afe0-2c2ce308d2bb)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c627ce9f-b543-4fa3-9042-d140d08c7555)

</details>

<details>
<summary>Sequential Logic Optimization</summary>

- Sequential logic optimization is the process of enhancing digital circuits that incorporate memory elements and time-dependent behavior, with the aim of improving performance, efficiency, and other key characteristics
-  Sequential logic optimization directly impacts the performance and reliability of digital circuits and systems.
-  Methods of computational optimization are
	1. Sequential constant propagation is a process used in computer programming and software optimization to identify and replace variables with their constant values in a sequential or step-by-step manner. This technique aims to replace variable values with their known constant values at various stages of the logic circuit, optimizing the design for better performance and resource utilization.
	2. State optimization is an optimization technique used in digital design to reduce the number of states in finite state machines (FSMs) while preserving the original functionality.
 	3. Sequential Logic Cloning replicates portions of sequential logic to alleviate bottlenecks and improve circuit throughput.
  	4. Retiming, Adjusts the placement of flip-flops within a circuit to optimize timing, balance critical paths, and enhance overall performance

### dff_const1

`!gvim dff_const1.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/901c3efa-77ea-48a4-add0-92e20d955df8)

- Simulation
```
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave tb_dff_const1.vcd
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/4eb14917-08b9-41f9-b331-5c85d31b0dfb)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/9cec763b-d531-44ef-9c0e-545512272a29)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1094a869-2537-48f1-af69-8151279f39b8)

### dff_const2

`!gvim dff_const2.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1602d765-5a15-4aeb-97c5-7a8e9ca842b0)

- Simulation
```
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave tb_dff_const2.vcd
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/4a0f4d5a-a06b-4879-8c21-ee245b2d5fdb)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/182f42e7-e1b1-4072-893e-7d28c4e41355)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/b888c1b4-2c9f-48af-8c97-b969597dd14f)

### dff_const3

`!gvim dff_const3.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/4d2f34cc-706b-42ac-b1d3-ae7490bfb641)

- Simulation
```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/dd0dbe97-7288-4b67-963e-337a55b9041a)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2b46c07f-3319-49fd-8c99-67a98162abba)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/47aa4f1e-72bc-46c5-913d-53597af3175b)

### dff_const4

`!gvim dff_const4.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/735fb6ba-8637-4fed-a7ad-c69cd6a76482)
- Simulation
```
iverilog dff_const4.v tb_dff_const4.v
./a.out
gtkwave tb_dff_const4.vcd
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/8b1b9931-2a9b-4327-bfc1-bf53023b7ec6)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f20c77a1-4f3e-4669-896b-37ef73e8dcd6)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c58babb7-0724-454f-8d9c-32c7089256d8)

### dff_const5

`!gvim dff_const5.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/55f69d3c-63a3-4db3-af56-74cb642470a5)

- Simulation
```
iverilog dff_const4.v tb_dff_const4.v
./a.out
gtkwave tb_dff_const4.vcd
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/1a86a42e-c651-4088-8f60-6801e6a5a1c4)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog dff_const5.v
synth -top dff_const5
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/0ced56bd-43fb-4f79-b6d5-f6d5c637a0f9)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2b57be12-b491-40de-9cd4-f6116e57d10c)

</details>

<details>
<summary>Sequential optimisations for unused outputs </summary>

### counter_opt

`!gvim counter_opt.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/53ac3d58-98c7-4d3a-bfeb-b5e3822f6cdf)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/9381d58b-9b76-407f-9f6d-0b3d0381403c)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/d936ced3-7163-4208-91ce-9d1561c32da3)

### counter_opt2

`!gvim counter_opt2.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/ca199c11-3f5f-48c6-9eb8-7d481926c84a)

- Synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog counter_opt2.v
synth -top counter_opt2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/f34bf9e5-3f04-472d-b85f-1fdce95657bc)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/e6c0f5fd-f137-4b14-9991-3ae1e62512ce)


</details>

## Day 4 - GLS, blocking vs non-blocking, and Simulation-Synthesis mismatch

<details>
<summary>GLS Concepts and Flow using Iverilog </summary>

### Gate-level Simulation
- Gate-level simulation is a method used in electronics design to test and verify digital circuits at the level of individual logic gates and flip-flops.
- It involves simulating the circuit using the actual logic gates and flip-flops that make up the design, as opposed to higher-level abstractions like RTL (Register Transfer Level) descriptions.
- Gate-level simulation is commonly used in designs where precise timing and functionality are critical.
- It operates at a lower abstraction level than higher-level simulations and is essential for debugging and ensuring circuit correctness.

### GLS using iVerilog

 ![image](https://github.com/ani171/pes_asic_class/assets/97838595/2250328e-ae37-45fb-b66c-01cf19a0d1f5)
1. Write RTL code.
2. Synthesize to generate gate-level netlist.
3. Create a testbench in Verilog.
4. Compile both netlist and testbench.
5. Run the simulation with compiled files. Debug and iterate as needed.
6. Perform timing analysis if necessary.
7. Generate test vectors for manufacturing tests.

### Synthesis-Simulation Mismatch

- Synthesis-simulation mismatch is when there are differences between how a digital circuit behaves in simulation at the RTL level and how it behaves after gate-level synthesis.
- This discrepancy can occur due to various reasons, such as timing issues, optimization conflicts, and differences in modeling between the simulation and synthesis tools.
- To address it, ensure consistent tool versions, check synthesis settings, debug with simulation tools, and follow best practices in RTL coding and design.
- Resolving these mismatches is crucial for reliable hardware implementation.

### Blocking and Non-blocking statements

- Blocking Statements
	- Blocking statements are executed sequentially in the order they appear in the code and have an immediate effect on signal assignments.
	- They are called "blocking" because they block the execution of subsequent statements until they are completed. Blocking statements are typically used within procedural blocks, such as always or initial blocks, to describe sequential behavior.
	- Blocking assignments are typically used to describe combinational logic, where the order of execution doesn't matter, and each assignment depends on the previous one.
```
always @(posedge clk) begin
    // Blocking assignments
    a = b; 
    c = a + 1; 
end
```

- Non-blocking statements
	- Non-blocking statements allow concurrent execution within a procedural block or always block, making them suitable for describing synchronous digital circuits.
	- Non-blocking assignments are typically used to model sequential logic, like flip-flops and registers, where parallel execution is required.
```
reg [2:0] state, next_state;
always @(posedge clk or posedge reset) begin
    if (reset) begin
        state <= 3'b000;
    end else begin
        // Non-blocking assignment to update the next state
        next_state <= state + 1;
    end
end
```

### Caveats With Blocking Statements
- Blocking statements in Verilog are essential for modeling sequential logic and specifying the order of operations within procedural blocks.
	- Race Conditions: When multiple blocking assignments are used within the same always block or initial block, there is a potential for race conditions. Race conditions occur when the final value of a signal depends on the execution order of assignments. To avoid race conditions, use non-blocking assignments or ensure that assignments do not depend on the order of execution.
	- Combinational Loops: Using blocking assignments to create combinational feedback loops (combinational loops with no flip-flops) can lead to undefined behavior and simulation issues.
 	- Debugging Challenges: Debugging code with many blocking assignments can be challenging, especially when trying to track down timing-related issues.
	- Limited for Testbenches: In testbench code, excessive use of blocking statements can lead to simulation race conditions that don't reflect real-world hardware behavior.
	- Order Dependency: The order of blocking statements can affect simulation results, leading to race conditions or unintended behavior.
	- Lack of Parallelism: Blocking statements do not accurately represent the parallel nature of hardware. In hardware, multiple signals can update concurrently, but blocking statements model sequential behavior. As a result, using blocking statements for modeling complex concurrent logic can lead to incorrect simulations.

</details>

