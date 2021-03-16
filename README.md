# Guidance of CACTI Memory Compiler

## 0. Introduction
The CACTI memory compiler is a first-order analytical modeling to estimate the 
area, energy and timing of SRAMs. It is based on the well-known [CACTI](http://www.hpl.hp.com/research/cacti/) 
modeling tool. 
The toolbox was originally developed by UC Berkeley for the aim of course
project. It simply acts as a python wrapper of CACTI and generates the required
library for ASIC design flow. I.e. Synopsys synthesis library (\*.lib) and 
abstract layout for P&R (\*.lef).

## 1. Installation
The software requires Python 2.7 and GNU Compiler Collection. First, we need 
to compile the provided cacti toolbox (a slightly modified version of original 
CACTI v6.5). The compilation can be done in the following steps:
- Change the current directory to the sub-directory `cacti65`
- Compile the CACTI using the Makefile, i.e.
```bash
make
```

The Cheetah package of Python can be installed using the following command:
```bash
pip install Cheetah
``` 

## 2. Usage
First, we need to prepare the configuration file to specify the memory 
specification, such as the memory width (in bits), depth, operation corner and 
etc. A sampled configuration file `sample.conf` is provided to serve as a 
reference. Then the top Python wrapper `ucbsc` can be called to generate the 
all the library files for ASIC design flow. The configuration filename should
be input as an argument of the `ucbsc`. For example, we can use the following
command to generate the sampled library files:
```bash
python ucbsc sample.conf  # `sample.conf` is the memory configuration file
```

The library files will be dumped to the specified directory (current directory
by default). The generated file includes:
- \*.cfg: Configuration file for CACTI tool inputs.
- out.csv: CSV file for CACTI tool output. It solely acts as a temporary file
 for Python wrapper.
- \*.v: RTL behavior model of SRAM. It only serves for the behavior 
 simulation of design, not for synthesis.
- \*.lib: ASCII-format of Synopsys library.
- \*.db: Binary-format of Synopsys library (for Synthesis). The file is 
 automatically from \*.lib if the library compiler is installed on the system.
- \*.lef: ASCII-format of abstract layout (inputs for Place and Route, such 
 as Encounter).   
 
## 3. ASIC FLow
Now that we have added the desired SRAM configuration, we can use the ASIC tools to generate layout for the SRAM val/rdy wrapper. In this section, we will go through the steps manually, and in the next section we will use the automated ASIC flow.
```
 `default_nettype none
 module SRAM_64x64_1P
 (
   input  wire [   5:0] A1,
   input  wire [   0:0] CE1,
   input  wire [   0:0] CSB1,
   input  wire [  63:0] I1,
   output wire [  63:0] O1,
   input  wire [   0:0] OEB1,
   input  wire [   7:0] WBM1,
   input  wire [   0:0] WEB1
 );

 endmodule // SRAM_64x64_1P
 `default_nettype wire
 ```
Notice that this SRAM module is empty! In other words, in the blackbox Verilog file, all SRAMs are implemented as “blackboxes” with no internal functionality. If we included the behavioral implementation of the SRAM, then Synopsys DC would try to synthesize the SRAM as opposed to using the SRAM macro.

For more info, please refer to [ece5745](https://cornell-ece5745.github.io/ece5745-tut8-sram/).

## History

3/16/2021: HaFred
 - Append the ASIC flow with CACTI

2/7/2018: Jingyang Zhu
  - Add README
  - Dump the generated results to a modular directory

4/12/2013: Yunsup Lee
  - compile cacti65 first
  - cleanup; remove ucbcc

8/18/2010: Yunsup Lee
  - initial commit
  - you need Cheetah a template engine for python to run the code
