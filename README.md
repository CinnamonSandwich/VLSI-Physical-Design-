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

![Screenshot from 2024-03-03 13-28-55](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d86e2189-6d5e-4d1f-ad96-50ce406def89)

To test if it is the limit, we modify as follows:
```c
unsigned long long int max = (unsigned long long int) (pow(2,127) -1);
unsigned long long int min = (unsigned long long int) (pow(2,127) *(-1));
```
The output obtained is the same.

![Screenshot from 2024-03-03 13-28-55](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d86e2189-6d5e-4d1f-ad96-50ce406def89)

 To check whether the values do get changed, we modify the code to:
```c
unsigned long long int max = (unsigned long long int) (pow(2,5) -1);
unsigned long long int min = (unsigned long long int) (pow(2,5) *(-1));
```
The output is ,

![Screenshot from 2024-03-03 13-33-12](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/441dc841-8cc8-4b2d-b416-70014fb44533)

Now we test it with another program,
We test it with the program  `signedLimits.c` 
```c
#include <stdio.h>
#include <math.h>

int main(){
	long long int max = (long long int) (pow(2,5) -1);
	long long int min = (long long int) (pow(2,5) *(-1));
	printf("lowest number represented by 64-bit integer is %lld\n",min);
	printf("highest number represented by 64-bit integer is %lld\n",max);
	return 0;
}
```

To compile and run with riscv64-gcc:
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o slimits.o signedLimits.c
spike pk slimits.o
```
The output obtained is,

![Screenshot from 2024-03-03 13-36-11](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/f26fa817-94e1-4c43-b575-87eaaa571397)

To test if it is the limit, we modify as follows:
```c
long long int max = (long long int) (pow(2,127) -1);
long long int min = (long long int) (pow(2,127) *(-1));
```
The output is the same:

![Screenshot from 2024-03-03 13-36-11](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/f26fa817-94e1-4c43-b575-87eaaa571397)

To check whether the values do get changed, we modify the code to:
```c
long long int max = (long long int) (pow(2,5) -1);
long long int min = (long long int) (pow(2,5) *(-1));
```

The output now is ,

![Screenshot from 2024-03-03 13-38-08](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/e6058770-4485-4520-94d5-24500686494c)

This concludes the assignment.


## Day 2 Assignments

### Assignment 1

The objective of this assignment is to write `load.s` and `1to9_custom.c` as descibed in the algorithm
- `load.s`
```asm
.section .text
.global load
.type load, @function

load:
	add	a4, a0, zero
	add	a2, a0, a1
	add	a3, a0, zero
loop:	add	a4, a3, a4
	addi	a3, a3, 1
	blt	a3, a2, loop
	add	a0, a4, zero
	ret
```
- `1to9_custom.c`
```c
cat 1to9_custom.c 
#include <stdio.h>

extern int load(int x, int y);

int main()
{
	int result = 0;
	int count = 9;
	result = load(0x0, count + 1);
	printf("Sum of nos. from 1 to %d is %d\n", count, result);
}
```
This concludes the assignment 1

### Assignment 2
The objective of this assignment is to compile the code with the files and options with the following command:
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.s
```
![Screenshot from 2024-03-03 13-42-15](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c7f6f89c-cd19-4d8e-9fcf-d5a20c3af1b4)

To run, use the following command:
```bash
spike pk 1to9_custom.o
```

The output generated is,

![image](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/3a267bc7-64c7-4184-a658-ecef2d3da6e3)

Upon inspecting the object dump we find,

![image](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/03642401-68f0-4a44-ad8a-198c3f7ce8e9)

This concludes the assignment 2

## Assignment 3
The objective of this assignment is to run C program on PicoRV32 

Make the set-up script executable, and execute
```bash
chmod +x rv32im.sh
./rv32im.sh
```
![image](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/6f490a96-0a31-4670-8e35-f762ebe7e603)


![image](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/2649f799-7e3d-43c7-9afa-906e169ad346)


![image](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c20e9cc6-74db-41f1-8642-c85cebb13866)

This concludes the assignment 3

## Day 1 Assignments - RTL

### Lab 1
The objective of the assignment is to simulate `good_mux.v` and seeing outputs in **gtkwave**


![image](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/831aa5b0-0dca-47fd-afe2-98c7d98f00cb)


This concludes lab 1

### Lab 2
The objective of this lab is to smth `good_mux.v` in YoSys 

First the liberty file is read, 

<img width="405" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c711f68f-747f-4441-bf2b-8322c99ce9fa">

Then the verilog file is read,

<img width="400" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/94f1dc2c-d2de-46ca-b0bc-768b71dd7bf6">

Then the top-level design is synthesized,
```
yosys> synth -top good_mux 

3. Executing SYNTH pass.

3.1. Executing HIERARCHY pass (managing design hierarchy).

3.1.1. Analyzing design hierarchy..
Top module:  \good_mux

3.1.2. Analyzing design hierarchy..
Top module:  \good_mux
Removed 0 unused modules.

3.2. Executing PROC pass (convert processes to netlists).

<removed extended output>

3.25. Printing statistics.

=== good_mux ===

   Number of wires:                  4
   Number of wire bits:              4
   Number of public wires:           4
   Number of public wire bits:       4
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_MUX_                          1

3.26. Executing CHECK pass (checking for obvious problems).
Checking module good_mux...
Found and reported 0 problems.
```
Then the gate level file is generated,
```
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

4. Executing ABC pass (technology mapping using ABC).

4.1. Extracting gate netlist of module `\good_mux' to `<abc-temp-dir>/input.blif'..
Extracted 1 gates and 4 wires to a netlist network with 3 inputs and 1 outputs.

4.1.1. Executing ABC.
Running ABC command: "<yosys-exe-dir>/yosys-abc" -s -f <abc-temp-dir>/abc.script 2>&1
ABC: ABC command line: "source <abc-temp-dir>/abc.script".
ABC: 
ABC: + read_blif <abc-temp-dir>/input.blif 
ABC: + read_lib -w /home/alphadelta1803/Desktop/folder/sky130RTLDesignAndSynthesisWorkshop/verilog_files/../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
ABC: Parsing finished successfully.  Parsing time =     0.11 sec
ABC: Scl_LibertyReadGenlib() skipped cell "sky130_fd_sc_hd__decap_12" without logic function.
<removed output>
ABC: + write_blif <abc-temp-dir>/output.blif 

4.1.2. Re-integrating ABC results.
ABC RESULTS:   sky130_fd_sc_hd__mux2_1 cells:        1
ABC RESULTS:        internal signals:        0
ABC RESULTS:           input signals:        3
ABC RESULTS:          output signals:        1
Removing temp directory.
```

The part in which we are interested is ,
```
ABC RESULTS:   sky130_fd_sc_hd__mux2_1 cells:        1
ABC RESULTS:        internal signals:        0
ABC RESULTS:           input signals:        3
ABC RESULTS:          output signals:        1
```

By using the `show` command we can view the logic diagram,

<img width="595" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/a7511660-8973-4ccf-8935-c29e288e14cf">

This concludes the Lab 2 as well as the Day 1 assignments for RTL.

## Day 2 Assignments - RTL

### Lab 1

We have a verilog file `multiple_modules.v`,

```verilog
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule

```
Upon Synthesizing the top module,

```
3.25. Printing statistics.

=== multiple_modules ===

   Number of wires:                  5
   Number of wire bits:              5
   Number of public wires:           5
   Number of public wire bits:       5
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     sub_module1                     1
     sub_module2                     1

=== sub_module1 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_AND_                          1

=== sub_module2 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_OR_                           1

=== design hierarchy ===

   multiple_modules                  1
     sub_module1                     1
     sub_module2                     1

   Number of wires:                 11
   Number of wire bits:             11
   Number of public wires:          11
   Number of public wire bits:      11
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     $_AND_                          1
     $_OR_                           1
```
- Upon running `show` command:
![show command](https://github.com/alfadelta10010/pes-asic-class/blob/main/day2_rtl/assets/show_mulmodule.png)
- This is the hierarchical design
- Writing the verilog file gives us the following output:
```verilog
/* Generated by Yosys 0.32+66 (git sha1 b168ff99d, gcc 11.4.0-1ubuntu1~22.04 -fPIC -Os) */

module multiple_modules(a, b, c, y);
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  wire net1;
  output y;
  wire y;
  sub_module1 u1 (
    .a(a),
    .b(b),
    .y(net1)
  );
  sub_module2 u2 (
    .a(net1),
    .b(c),
    .y(y)
  );
endmodule

module sub_module1(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule

module sub_module2(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__or2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule
```

We see that the hierarchies are preserved.

This concludes the Lab 1

### Lab 2

The objective of this lab is to write a flat netlist.

Using `flatten` and `write_verilog -noattr multiple_modules_flat.v`, we get the following:

```verilog
/* Generated by Yosys 0.32+66 (git sha1 b168ff99d, gcc 11.4.0-1ubuntu1~22.04 -fPIC -Os) */

module multiple_modules(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  wire net1;
  wire \u1.a ;
  wire \u1.b ;
  wire \u1.y ;
  wire \u2.a ;
  wire \u2.b ;
  wire \u2.y ;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _6_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  sky130_fd_sc_hd__or2_0 _7_ (
    .A(_4_),
    .B(_3_),
    .X(_5_)
  );
  assign _4_ = \u2.b ;
  assign _3_ = \u2.a ;
  assign \u2.y  = _5_;
  assign \u2.a  = net1;
  assign \u2.b  = c;
  assign y = \u2.y ;
  assign _1_ = \u1.b ;
  assign _0_ = \u1.a ;
  assign \u1.y  = _2_;
  assign \u1.a  = a;
  assign \u1.b  = b;
  assign net1 = \u1.y ;
endmodule
```

By running the `show` command we get,

<img width="602" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/76a36e97-4b7a-41a0-99c8-e63b187df4ba">

And by executing the submodule synthesis we get,

<img width="592" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d4321cd2-2fed-4d86-bee5-9b1fefa7892c">

This concludes lab 2.

### Lab 3.1

In this lab we will simulate and synthesize a D-Flip Flop with an asynchronous reset,

Simulation results,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/401e9ca1-ecc1-4287-bdee-502592d398d9">

Synthesis results,

<img width="595" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/e02cf794-a96b-48d9-b769-84c1a89e068b">

This concludes Lab 3.1

### Lab 3.2

In this lab we will simulate and synthesize a D-Flip Flop with an asynchronous set,

Simulation results,

<img width="599" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/1603f80c-85c2-45b7-838c-c63892ff964a">

Synthesis results,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/f4c34f8b-0d93-426a-bb43-d8e19fc971e7">

This concludes Lab 3.2

### Lab 3.3

In this lab we will simulate and synthesize a D-Flip Flop with a synchronous reset,

Simulation results,

<img width="599" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/f530740e-1a1b-4bc5-9b90-e5a03d435ed6">

Synthesis results,

<img width="598" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/baa2e983-97b4-4210-aad6-c1a57b0f6d62">

This concludes Lab 3.3

### Lab 4.1

In this lab we will be running optimizations on `mult_2.v`,

Synthesis report :

<img width="572" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/6a057fff-7d25-4c86-97ac-29f4a10f7e8e">

By using `show` command we get,

<img width="599" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c10cd844-aeda-4d17-973d-7b7b98bdeaba">

The netlist dumped by YoSys is,

```verilog
/* Generated by Yosys 0.32+66 (git sha1 b168ff99d, gcc 11.4.0-1ubuntu1~22.04 -fPIC -Os) */

module mul2(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [3:0] y;
  wire [3:0] y;
  assign y = { a, 1'h0 };
endmodule
```

This concludes Lab 4.1

### Lab 4.2

In this lab we will be running optimizations on `mult_8.v`,

The synthesis report is,

<img width="497" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/3a1be3bc-0c81-4776-80b6-a5c450fe0e96">

The netlist dumped by YoSys is,

```verilog
/* Generated by Yosys 0.32+66 (git sha1 b168ff99d, gcc 11.4.0-1ubuntu1~22.04 -fPIC -Os) */

module mult8(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [5:0] y;
  wire [5:0] y;
  assign y = { a, a };
endmodule
```

This concludes Lab 4.2

## Day 3 Assignments - RTL

### Lab 1
The series of labs will deal with combinational logic operations.

#### Lab 1.1

We will be synthesizing `opt_check.v`,

Code :
```verilog
module opt_check (input a , input b , output y);
        assign y = a?b:0;
endmodule
```

Synthesis report is ,

<img width="565" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/ba938dee-0860-47ae-9c2b-e8ac4e1b3854">

Using `show` command we get,

<img width="595" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/2b0cc4a2-b01e-47da-90c7-eaec71fab790">

This concludes Lab 1.1

#### Lab 1.2 

We will be synthesizing `opt_check2.v`,

Code : 
```verilog
module opt_check2 (input a , input b , output y);
        assign y = a?1:b;
endmodule
```

The synthsis report is ,

<img width="516" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c75c1d46-bd9c-4250-ab25-c847feebcb56">

By using `show` command we get ,

<img width="590" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d59fe52a-88d3-4f0b-85ca-dea82f423a85">

This concludes Lab 1.2

#### Lab 1.3

In this lab we will be synthesizing `opt_check3.v`,

Code:
```verilog
module opt_check3 (input a , input b, input c , output y);
        assign y = a?(c?b:0):0;
endmodule
```

Synthesis report is ,

<img width="574" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/af8caa1b-0b9a-45c3-8448-bafe4abc46fe">

By using `show` command we get,

<img width="591" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c0ba7d3a-a35c-439e-89a5-3c1edf49f892">

This concludes Lab 1.3

#### Lab 1.4
In this lab we will be synthesizng `opt_check4.v`,

Code:
```verilog
module opt_check4 (input a , input b , input c , output y);
	assign y = a?(b?(a & c ):c):(!c);
endmodule
```
Synthesis report is ,

<img width="479" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c4b05599-9485-4047-a970-b55ed9941d5c">

By using `show` command we get,

<img width="595" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d5120f36-042a-47c0-9f96-564edc634c77">

This concludes Lab 1.4

#### Lab 1.5

In this lab we will be synthesizing `multiple_module_opt.v`

Code:
```verilog
module sub_module1(input a , input b , output y);
	assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
	assign y = a^b;
endmodule


module multiple_module_opt(input a , input b , input c , input d , output y);
	wire n1,n2,n3;
	sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
	sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
	sub_module2 U3 (.a(b), .b(d) , .y(n3));
	assign y = c | (b & n1);
endmodule
```

The synthesis report is,

![mult_mod_opt_synth_report](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/6317e7a5-b6f4-479e-bab7-1501d816b55a)

By using `show` command we get,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/e9d00ced-a432-4a44-beb1-916e79b0db9f">

This marks the end of Lab 1

### Lab 2
This series of labs deal with Sequential Logic Optimiztions

#### Lab 2.1
In this lab we will simulate and aynthesize `dff_const1.v`.

Code:
```verilog
module dff_const1(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b0;
			else
                q <= 1'b1;
		end
endmodule
```
Simulation,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/11a1ad72-a1f9-40bd-af38-3c6066268137">

Synthesis report,

<img width="546" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/6747cb29-0e5e-49d5-9e92-392a20dfcf7e">

By using `show` command we get,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/07bf1892-2ab2-4a76-9c02-d3859596588d">

This concludes lab 2.1

#### Lab 2.2

In this lab we will be simulating and synthesizing `dff_const2.v`

Code:
```verilog
module dff_const2(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
		begin
        	if(reset)
                q <= 1'b1;
        	else
                q <= 1'b1;
		end
endmodule
```
Simulation,

<img width="599" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/b996b9de-ceaf-42a5-bd9b-70b6bb68726c">

Synthesis report ,

<img width="572" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/61e60c62-efbf-4701-aae5-e58cbd30106c">

By using `show` command we get,

<img width="591" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/0c3d27b9-a523-4878-8a50-c3b4f1e4db36">

This concludes lab 2.2

#### Lab 2.3

In this lab we will be simulating and synthesizing `dff_const3.v`

Code:
```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
        if(reset)
        begin
                q <= 1'b1;
                q1 <= 1'b0;
        end
        else
        begin
                q1 <= 1'b1;
                q <= q1;
        end
end
endmodule
```
Simulation ,

<img width="602" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/11392af3-984c-4e49-a2c9-f22a4ef66dd1">

Synthesis report,

<img width="514" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/e4bb6e1d-630a-42b5-91d4-ecabbb686766">

By using `show` command we get,

<img width="598" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/fc5a15f5-bf84-403c-a235-b6f600739eb5">

This concludes lab 2.3

#### Lab 2.4
In this lab we will be simulating and synthesizing `dff_const4.v`

Code:
```verilog
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
        if(reset)
        begin
                q <= 1'b1;
                q1 <= 1'b1;
        end
        else
        begin
                q1 <= 1'b1;
                q <= q1;
        end
end

endmodule
```

Simulation,

<img width="602" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/2b118c02-ada2-4678-bb5b-1b4a04166091">

Synthesis report,

<img width="542" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/fa807be5-9985-4405-aab9-99a8b8b9c98b">

By using `show` command we get,

<img width="594" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c4a25a34-baac-4430-a41a-cc904994e0ba">

This concludes lab 2.4

#### Lab 2.5
In this lab we will be simulating and synthesizing `dff_const5.v`

Code:
```verilog
module dff_const5(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
        if(reset)
        begin
                q <= 1'b0;
                q1 <= 1'b0;
        end
        else
        begin
                q1 <= 1'b1;
                q <= q1;
        end
end

endmodule
```

Simulation,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/81489b4b-605b-41a2-95ba-cf39caef4f11">

Synthesis report,

<img width="557" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/a645d22f-380a-40dc-abc7-38b9ff56b196">

By using `show` command we get,

<img width="590" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/4833e644-0856-4a0a-b7e6-15337c168573">

This concludes Lab 2.5 and marks the end of Lab 2

### Lab 3
This series of labs will deal with Sequential Optimisations for Unused Outputs


#### Lab 3.1

In this lab we will be synthesizing `counter_opt.v`

Code:
```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
        if(reset)
                count <= 3'b000;
        else
                count <= count + 1;
end
endmodule
```

Synthesis report:

<img width="504" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/43b24991-5447-4768-a31a-014005ddb7ef">

By using `show` command we get,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/27df4225-26de-4b3e-a9c0-fd33c1471788">

This concludes lab 3.1

#### Lab 3.2
In this lab we will be synthesizing `counter_opt2.v`

Code:
```verilog
module counter_opt2 (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
        if(reset)
                count <= 3'b000;
        else
                count <= count + 1;
end

endmodule
```

Synthesis report:

![counter_opt2_synth_report](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/8342c728-6863-4070-b902-63a308361dd6)


By using `show` command we get,

<img width="593" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/129d7619-92a1-40f2-8fa8-99ceea158b78">

This marks the end of lab 3

## Day 4 Assignments - RTL

### Lab 1 
This series of labs will deal with GLS and Synthesis-Simulation Mismatch.

#### Lab 1.1

In this lab we will be simulating, synthesizing and finding the GLS to Gate Level Simulation of `ternary_operator_mux.v`

Code :

```verilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
endmodule
```
Simulation,

<img width="602" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/75827fca-d6c8-4d4f-bfdc-cfa6d977d5b4">

Synthesis report,

<img width="394" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c177cebd-f4f4-4760-b6a6-036b2a1e7890">

By using the `show` command we get,

<img width="593" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d30d98ff-05a5-40f8-b1cf-33734bd9bba7">


The GLS to Gate-Level Simulation is,

<img width="602" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/f2bd82b3-ea8a-4274-8fec-88e37cef8bec">

This concludes lab 1.1


#### Lab 1.2

In this lab we will be simulating, synthesizing and finding the GLS to Gate Level Simulation of `bad_mux.v`

Code:

```verilog
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

Simulation,

<img width="604" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/e7bf6ab1-a1ac-4711-86ba-e54f82ec505a">

Synthesis report is,

<img width="398" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/c5151c78-d808-4469-bd2e-e05bc5d4d042">

By using the `show` command we get,

<img width="598" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/3f872033-cded-4f4f-965b-6ca3d3c3c2d2">

The GLS to Gate-Level Simulation is,

<img width="602" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d2008cbb-b9b1-4ff2-9cba-d6c690f20aab">

This concludes lab 1.2 and marks the end of lab 1


### Lab 2

This lab deals with Synth-Sim Mismatch for Blocking Statement

We Simulate, synthesize and find the GLS to Gate-Level Simulation of `blocking_caveat.v`

Code:

```verilog
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```
Simulation,

<img width="599" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/941723f1-961d-450b-b05d-d80de7cb9d29">


Synthesis report is,

<img width="395" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/fc699373-77d6-43de-9e54-07d856f9b53c">

By using the `show` command we get,

<img width="596" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/2815b768-b436-4862-b942-9c87427cba09">

The GLS to Gate-Level Simulation is,

<img width="599" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/7846ef71-78e3-4dc2-a78c-c78e5adf1b6e">

This marks the end of lab 2.



# Physical Design 

The following labs will deal with different aspects of physical design with various open source tools.

The labs will now be done on the VM image provided by VSD.

## Day 1

### Topic 1: Design Preparation Step

This lab will deal with the design and prepration step in the process of Physical Design 

We do this by,

Running `flow.tcl` and adding package and design

<img width="623" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/976f673b-04a5-4eb3-8add-eb50852da3b5">


### Topic 2: Review files after design prep and run synthesis

We look for the `tmp` folder inside `runs` folder,

Going into the `merged.lef` file we get,

<img width="623" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/457538a0-c843-42b2-86bb-716c94aefac0">

By performing `run_synthesis` we get,

<img width="622" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/0a7649cd-8490-4711-a09b-a8a99e2c064f">


### Topic 3: Steps to characterize synthesis results

 We start by calculating flop ratio,

 ![run_synthesis_stats](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/1e041131-c750-4f8b-8781-05312ec105fe)

```
dfxtp_4 = 1613,
Number of cells = 18036,
Flop ratio = 8.94%
```

The results in `picorv32a.synthesis.v` show ,

<img width="620" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/181ff941-8426-48e5-b502-4ff82969632c">


This marks the end of day 1.

## Day 2

### Topic 1: Steps to run floorplan using OpenLANE

We generate the floorplan,

<img width="621" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/ef697767-c8c2-405a-bc2e-bd1e40cc4803">


### Topic 2: Review floorplan files and steps to view floorplan

The reviewing is done in magic,

By the following command,
`magic -T /usr/local/tools/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &`

<img width="622" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/8f543cde-1f2c-41d8-83f9-e582d9207285">


### Topic 3: Reviewing floorplan layout in Magic

By zooming in we get,

<img width="620" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/7870c96b-30bd-4d35-8469-e090b125988c">

Further zooming in,

<img width="622" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/d6e4b852-0159-43cf-92c8-8ff3dd878fbb">

The report is,

<img width="276" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/6ea8d255-aa39-4510-8534-1195366677b8">


### Topic 4: Congestion aware placement using RePlAce

 `picorv32a.placement.def.png` helps us to take a look at the placement of cells,

![picorv32a placement def](https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/63690c8d-ce59-49c0-ba8a-184bb6c28f23)


Opening in `magic` we get,

<img width="620" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/07c1066d-5ffb-481f-9549-f010012a0189">

Zooming in we see,

<img width="623" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/65e91020-5127-473c-9020-0e05dd7c7165">


This marks the end of day 2.

## Day 3

### Topic 1: I/O Placer Revision

The original output is as such,

<img width="238" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/2163b302-3aad-465a-8c25-402aeb22ded3">

Looking at the 'floorplan.tcl` file,

<img width="163" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/999820c7-cd8d-406a-96e4-edea7aadfa9b">

the modifications are made as follows,

<img width="194" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/a2452ff1-4c28-43fc-93fb-f91e0e2c1ce8">

The final I/O structure obtained is ,

<img width="620" alt="image" src="https://github.com/CinnamonSandwich/VLSI-Physical-Design-/assets/92498341/783c0b3c-62b3-4092-bb8b-395f52db61d0">


### Topic 2: VSD Std Cell Design













































































