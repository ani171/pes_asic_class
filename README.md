# pes_asic_class
# VLSI Physical Design for ASIC's
## Installation of RISC-V Toolchain

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

## Day 1
## Introduction  to RISC-V ISA and GNU Compiler Chain
#### Instruction Set Architecture
Instruction set architecture or computer architecture is an abstract model of the computer that defines how the CPU is controlled by the software. It acts as an interface between languages like C, C++, Java, and the hardware. The type of instructions depends on the type of hardware.

### From Apps to Hardware
Application software ---> System software ---> Hardware
- System Software converts application software into binary language
- It has three major parts:
  1. Operating system
  2. Compiler
  3. Assembler
-The operating system acts on small functions present in C, C++, Java, or any other language codes and gives it to the Compiler which in turn generates the .exe file which has all the Instructions. The .exe file is fed into the assembler, which generated the Machine Language code through which hardware can be implemented
### Type of Instructions
- Pseudo Instructions
- Base Integer Instructions(RV64I)
- Multiply Extension(RV64M)
- Single and Double precision floating point Extension(RV64F and RV64D)
#### Application Binary Interface
These are the keywords through which programmers can access the registers of RISC-V. They are basically the **System functions** associated with the RISC-V registers

### LAB
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
- click on ENTER to show the first line and successive ENTER to show successive lines
- click on q to quit the debug process

### Integer Number Representation
- Unsigned numbers: are just like integers but they don't have a + or - sign associated with them.
  Range: [0, (2^n)-1 ]
- Signed numbers: these are a set of both positive and negative numbers
  Range : [0, 2^(n-1)-1] to [-1 to 2^(n-1)]
  To represent negative numbers in binary 2's complement methodology is used.
### LAB
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

## Day 2
## Introduction to ABI and Basic Verification flow

### Application Binary Interface
- An Application Binary Interface is the interface between two binary program module programs allowing them to work together. It defines the interface between two software components or systems that are written in different programming languages, compiled by different compilers, or running on different hardware architectures.
- ABI defines how your code is stored inside the library file, so that any program using your library can locate the desired function and execute it.

### Double Words- Memory allocation
Architecture can also be divided into two types based on the process of loading memory. Memory can be loaded in two ways
1. Little Endian: In here, the least significant byte is at the lowest memory address, and the most significant byte is at the highest memory address.
2. Big Endian: Here, the most significant byte is at the lowest memory address, and the least significant byte is at the highest memory address.

![image](https://github.com/ani171/pes_asic_class/assets/97838595/2745c597-e8d1-4ff9-8a52-a7001e178130)

### Load, Add, and Store Instructions

- Load Instruction
Considering the instruction ```ld x8,16(x23)```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/775631b4-8e05-4194-ad01-27c17ee5ccda)

  - Here ld represents loading of double word
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

### 32 Registers and their general ABI Names
Through the ABI names, we reserve some of these registers for certain purposes
![image](https://github.com/ani171/pes_asic_class/assets/97838595/3c47998d-33da-4a49-badd-6f3d99dafb29)

### LAB
- Using ABI Function calls (re-writing C program using ASM language)

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

### RISC-V CPU
### PICORV-32
PicoRV-32 is a size-optimized RISC-V CPU Core that implements the RISC-V RV32IMC Instruction Set.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/d06a8c6d-7546-4cae-b99b-0e137a259294)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/3f26df7d-57bd-4d96-ae8d-b56f851bce32)

## RTL Design using SKY130 Technology

## Iverilog Design and testbench

- RTL Design is checked for adherence to the spec by simulating the design
- Simulator (Iverlog in here) is a tool used for checking the design ( set of Verilog codes in here)
- Working of Simulator: The Simulator looks for changes in the input signal and evaluates the output. If the input values are changed, only then they are reflected in the changes in output values

### Testbench 
- Testbench is an environment used to verify the correctness or soundness of a design or model.
- TestBench does not have any primary inputs or outputs
![image](https://github.com/ani171/pes_asic_class/assets/97838595/726e3f71-c496-464b-9919-841548fe23de)


### Iverilog Based Simulation flow
- vcd file: A Value Change Dump file stores all the information about value changes in the simulator
- GTKwave: It is a software, used as a simulation tool to verify the Verilog design code through a testbench.
 	```
	sudo apt install gtkwave
  	```
![image](https://github.com/ani171/pes_asic_class/assets/97838595/c95028f6-66a5-49a6-a165-3a7bdd3310ba)

### Labs using Iverilog and GTKwave
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
![image](https://github.com/ani171/pes_asic_class/assets/97838595/227e9d60-e736-40b9-ba23-7ca7f51c0845)
![image](https://github.com/ani171/pes_asic_class/assets/97838595/00745329-b2f4-4299-930f-2a5a5bcbc07d)

- For looking into the file structure `gvim tb_good_mux.v -o good_mux.v`
![image](https://github.com/ani171/pes_asic_class/assets/97838595/b7f938c7-7768-43fb-9aee-45a600758255)

## Logic Synthesis

- RTL Design: Behavioural representation of the required specification
-  .lib: Collection of logical modules
-  Synthesizer: Tool used for converting RTL to netlist
-  Netlist: Representation of design in the form of standard cells present in .lib
![image](https://github.com/ani171/pes_asic_class/assets/97838595/2150b1cf-e724-42a7-ac34-4879fcc3c298)


**Need of different flavors of gates**
- Combinational logic (Propagation Delay) determines the maximum speed of operation of the digital logic circuit
- T_clock > T_pd + T_cq + T_setup
- To achieve maximum clock frequency, for better performance the delays should be as minimum as possible. This would mean that only faster cells are sufficient
- But to ensure that there are no hold delay issues, gates are required to work slowly, creating a contractionary requirement
- Therefore, for better performance fast cells are used  while to avoid hold-time delays slow cells are used.

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

## Yosys
- Yosys is a framework for Verilog RTL Synthesis.
![image](https://github.com/ani171/pes_asic_class/assets/97838595/09ce7b51-d138-40e2-9716-923632c49efc)

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
![image](https://github.com/ani171/pes_asic_class/assets/97838595/78a6bc41-60ee-4548-9766-e3c1d219371e)

