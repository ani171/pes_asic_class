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
-- Operating system
-- Compiler
-- Assembler


