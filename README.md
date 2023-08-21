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
The above commands will update your gcc to version 12
To check for successful installation run the below command and the output will be shown as depicted below
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

### LAB-1
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


  




