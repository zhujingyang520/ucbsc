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


## History

2/7/2018: Jingyang Zhu
  - Add README
  - Dump the generated results to a modular directory

4/12/2013: Yunsup Lee
  - compile cacti65 first
  - cleanup; remove ucbcc

8/18/2010: Yunsup Lee
  - initial commit
  - you need Cheetah a template engine for python to run the code
