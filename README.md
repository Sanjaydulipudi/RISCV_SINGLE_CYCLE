# RISC-V Single Cycle CPU in Verilog HDL

## Overview

This project presents the design and implementation of a **32-bit Single Cycle RISC-V Processor** using **Verilog HDL** in **Xilinx Vivado**. The processor follows a modular hardware design methodology where each functional block is implemented as an independent Verilog module and then integrated into a complete CPU datapath.

The processor executes instructions in **one clock cycle per instruction**, meaning fetch, decode, execute, memory access, and write-back all occur within a single cycle.

This project demonstrates fundamental processor architecture concepts including:

* Instruction Fetch
* Program Counter Control
* Instruction Decode
* Register File Operations
* Immediate Generation
* Arithmetic Logic Unit (ALU)
* Branch Decision Logic
* Data Memory Access
* Write Back Stage

---

# Project Objective

The objective of this project is to build a working RISC-V CPU core from scratch to understand internal processor operation at RTL level.

This project helps in learning:

* Computer Architecture
* CPU Datapath Design
* Control Unit Logic
* Verilog HDL Coding
* Simulation and Verification
* FPGA Design Flow using Vivado

---

# Processor Type

| Parameter     | Value                       |
| ------------- | --------------------------- |
| ISA           | RISC-V RV32I (Basic Subset) |
| Architecture  | Single Cycle                |
| Data Width    | 32-bit                      |
| Address Width | 32-bit                      |
| Design Style  | Modular RTL                 |
| HDL           | Verilog                     |

---

# Single Cycle Architecture

In this processor, each instruction completes in one clock cycle.

## Instruction Flow

```text
Fetch → Decode → Execute → Memory → Writeback
```

All five operations occur in one cycle.

---

# Supported Instructions

## Arithmetic Instructions

* ADD
* SUB
* AND
* OR
* XOR
* SLT

## Immediate Instructions

* ADDI

## Memory Instructions

* LW
* SW

## Branch Instructions

* BEQ
* BNE
* BLT
* BGE

---

# Module Description

---

## 1. riscv_cpu_top.v

Top-level processor module integrating all stages.

### Responsibilities:

* Connect all submodules
* Route control/data signals
* Manage instruction execution flow

---

## 2. fetch_pc.v

Program Counter Register.

### Function:

Stores current instruction address.

```text
PC <= next_pc
```

---

## 3. fetch_next_pc.v

PC Incrementer.

### Function:

```text
PC + 4
```

Moves to next sequential instruction.

---

## 4. fetch_pc_select.v

PC Selection MUX.

### Function:

Selects next PC between:

* Sequential PC+4
* Branch target address

---

## 5. instruction_mem.v

Instruction ROM storing machine code program.

### Function:

Outputs instruction based on PC address.

---

## 6. control_decoder.v

Main Control Unit.

### Generates:

* reg_write
* alu_src
* mem_read
* mem_write
* mem_to_reg
* branch

---

## 7. register_bank.v

32 x 32 Register File.

### Features:

* Two read ports
* One write port
* x0 always zero

---

## 8. immediate_extend.v

Immediate Generator.

### Supports:

* I-Type
* S-Type
* B-Type immediates

---

## 9. execute_alu_decoder.v

ALU Control Unit.

### Generates ALU operation code from:

* opcode
* funct3
* funct7

---

## 10. execute_alu.v

Arithmetic Logic Unit.

### Performs:

* ADD
* SUB
* AND
* OR
* XOR
* SLT

---

## 11. execute_branch_logic.v

Branch Comparator.

### Supports:

* BEQ
* BNE
* BLT
* BGE

Outputs branch_taken signal.

---

## 12. data_memory.v

RAM for load/store instructions.

### Supports:

* Read data
* Write data

---

## 13. writeback_mux.v

Writeback selector.

Selects:

* ALU Result
* Memory Data

---

# Datapath Flow

```text
PC
 ↓
Instruction Memory
 ↓
Control Decoder
 ↓
Register File
 ↓
Immediate Generator
 ↓
ALU
 ↓
Data Memory
 ↓
Writeback MUX
 ↓
Register File
```

---

# Branch Flow

```text
Register Compare
 ↓
Branch Logic
 ↓
PC Select MUX
 ↓
Next PC
```

---

# Sample Program Executed

```assembly
addi x1, x0, 5
addi x2, x0, 10
add  x3, x1, x2
sw   x3, 0(x0)
lw   x3, 0(x0)
beq  x3, x3, label
addi x4, x0, 1
label:
addi x5, x0, 99
```

---

# Simulation Verification

The processor was simulated successfully in Xilinx Vivado.

## Verified Operations

* Register writes
* ALU arithmetic
* Memory store/load
* Branch taken
* PC redirection
* Final register values

---

# Example Results

| Register | Value |
| -------- | ----- |
| x1       | 5     |
| x2       | 10    |
| x3       | 15    |
| x5       | 99    |

---

# Tools Used

| Tool                 | Purpose                |
| -------------------- | ---------------------- |
| Verilog HDL          | RTL Design             |
| Xilinx Vivado 2025.1 | Simulation & Synthesis |
| Waveform Viewer      | Debugging              |

---

# Key Learning Outcomes

This project provides practical understanding of:

* Processor datapath design
* Control signal generation
* RISC-V instruction formats
* Register file architecture
* Branching mechanisms
* RTL debugging
* Hardware verification flow

---

# Limitations of Single Cycle CPU

Because all instructions complete in one cycle:

* Slow clock frequency
* Long critical path
* Poor performance scaling

---

# Future Improvements

## Planned Next Version

### 5-Stage Pipelined RISC-V CPU

Stages:

```text
IF | ID | EX | MEM | WB
```

---

# Author

**DULIPUDI LAASHMITH SANJAY**

B.Tech Graduate
Electronics / VLSI / Digital Design Enthusiast

---

# License

Open-source for learning and educational use.

---

# Final Note

This project forms a strong foundation for advanced CPU design including pipelined processors, cache integration, FPGA implementation, and SoC development.
