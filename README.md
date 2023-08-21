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















