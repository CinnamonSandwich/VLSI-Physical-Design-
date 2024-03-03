# VLSI Physical Design For ASICs

## Introduction
VLSI is short for Very Large Scale Intergration, it is the backbone of electronic chipset and circut design.This involves combining millions and billions of transistors into a single chip.
ASIC stands for Application Specific Integrated Circuit. These chips are designed to perform only one particular task or application and cannot be reprogrammed or modified after its production.

This repository contains assignments, course work and information regarding the VLSI Physical Design for ASICs special topic conducted by [Kunal Ghosh](https://in.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836?challengeId=AQFOE-8XWqvDuQAAAYoTpyzxq1afC_Nxqc_dZ94UOZth6AanND2-gAFCOIj8R3y0DMvnaTTmTFI7aKc-LSG02Hywcef8bxZSpg&submissionId=4563a283-6122-7d17-96c4-cca25ab22f22&challengeSource=AgEFo5oprL5AWwAAAYoTp4ZZHOyILiBZK_2PRvWstoMYK7pvKzJMdcHXAziDDsE&challegeType=AgFPGaInMEcw5wAAAYoTp4Zc1PMIhi6R2bACXVZ77SGGmdi-Xvh6dLM&memberId=AgHb4FI9TN1n0gAAAYoTp4Zf8VGMv0VqEBsKP6Fg99QBV0U&recognizeDevice=AgFeN_2mMINCIAAAAYoTp4Zj_9GbyKDu-JtCCfHIqIeS-azOH4Qt) at PES University Electronic City Campus for the Electronics and Communication Engineering Department.

## Tools and Downloads Required 
This course requires a few tools that are necessary to work on the lab and course contents:
- RISC-V GNU Compiler Toolchain
- Spike RISC-V ISA Simulator
- RISC-V Proxy Kernel and Boot Loader

All of these can be installed by the setup script provided by the instructor:
```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh 
./run.sh
```
## Lab Assignments
### Day-1
#### Assignment 1
Write a program to display the sum of numbers from 1 to n and compile with x86-gcc

The code is as follows, 


![Screenshot from 2023-08-21 16-24-17](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d3f8f752-cc79-42e3-b07b-5357b8256a97)



The output obtained is : 


![Screenshot from 2023-08-21 16-19-17](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/1810a794-fd5f-4c3a-84f2-74b75a565c81)


#### Assignment-2
The program is disassebled to run on riscv processors. This code is compiled using riscv64-gcc.




![Screenshot from 2023-08-21 17-19-53](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/77328cbf-1773-4a4d-8c95-f1f62e06f5a9)



The output is obtained using the command :
```
riscv64-unknown-elf-objdump -d 1ton.o | less
```
And then the output is searched for the main function by ```/main```
The output of the main function obtained is as follows:










![Screenshot from 2023-08-21 17-23-49](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/3d11e323-f233-4d4a-beef-9bbdbb6429d5)





Around 15 instructuons are returned.

Now instead of ```-O1``` , ```-Ofast``` is substitued,
we get,




![Screenshot from 2023-08-21 17-37-08](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/347e056c-6bc9-4721-b6d1-de2d5ad54316)


and the output is as follows.




![Screenshot from 2023-08-21 17-40-26](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/90c8fd9c-0cbf-4e8a-ad91-2ea943cd0276)


It is seen that 12 instructions are returned this time.


#### Assignment-3

Here we shall execute and debug ```1ton.c``` file and use the riscv64-gcc compiler:

Run the command ,
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o rv.out 1ton.c
```

The following output is obtained,
 
![Screenshot from 2024-01-15 14-14-53](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/24d9e559-ffa2-451b-8d07-39a275bf2947)

To execute the program we now use the Spike RISCV ISA Simulator.

![Screenshot from 2024-01-15 14-16-45](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/2b91bb6e-9af2-4e59-8d25-c5f8d0c8ae22)

Debugging is done by starting Spike in debug mode by using the command,
```
spike -d pk rv.out
```
 The program counter is moved from 0 to the first instruction using ,
 ```
until pc 0 100b0
```
and to find the contents of register a0 before being written we use,
```
reg 0 a0
```

The next instruction is executed by pressing enter,
and we see the register value has changed after execution,

![Screenshot from 2024-01-15 14-22-01](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/cb2752c0-09dd-4e04-a393-ec2976be0bd6)

The stack pointer updated value is seen by,

![Screenshot from 2024-01-15 14-23-45](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/f3aafff7-3ce2-44d1-9053-a32e8e89f3e3)

It is seen that the value has changed by 10.

#### Assignment 4
The objective of this assignment is to test the limits of `long long int` on RV64I, on `unsigned` and `signed` integer.

Write the program `unsignedLimits.c` 
```c
#include <stdio.h>
#include <math.h>

int main(){
	unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
	unsigned long long int min = (unsigned long long int) (pow(2,64) *(-1));
	printf("lowest number represented by unsigned 64-bit integer is %llu\n",min);
	printf("highest number represented by unsigned 64-bit integer is %llu\n",max);
	return 0;
}
```
To compile and run with riscv64-gcc:
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o limits.o unsignedLimits.c
spike pk limits.o
```
The output obtained is ,






















