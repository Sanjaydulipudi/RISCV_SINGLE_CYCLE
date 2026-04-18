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

## 1. riscv_cpu_top.v

Top-level integration module of the processor. It connects all submodules such as fetch unit, control unit, register file, ALU, memory, branch logic, and writeback stage to form a complete single-cycle CPU.

---

## 2. fetch_pc.v

Program Counter (PC) register module. It stores the current instruction address and updates it with the next PC value on every clock edge. It resets to zero during system reset.

---

## 3. fetch_next_pc.v

PC incrementer module. It calculates the sequential next instruction address by adding 4 to the current PC value, since each RISC-V instruction is 32 bits (4 bytes).

---

## 4. fetch_pc_select.v

PC selection multiplexer. It chooses the next PC source between normal sequential address (PC+4) and branch target address based on the branch_taken signal.

---

## 5. instruction_mem.v

Instruction memory module. It stores the machine code instructions of the program and outputs the instruction corresponding to the current PC address.

---

## 6. control_decoder.v

Main control unit of the processor. It decodes the instruction opcode and generates control signals such as register write enable, ALU source select, memory read/write enable, memory-to-register select, and branch control.

---

## 7. register_bank.v

32 × 32 register file module. It contains 32 general-purpose registers with two read ports and one write port. Register x0 is permanently hardwired to zero.

---

## 8. immediate_extend.v

Immediate generator module. It extracts immediate values from different RISC-V instruction formats (I-type, S-type, B-type) and performs sign extension to 32 bits.

---

## 9. execute_alu_decoder.v

ALU control decoder module. It interprets funct3, funct7, and opcode fields of the instruction and generates the ALU operation code required for arithmetic or logical execution.

---

## 10. execute_alu.v

Arithmetic Logic Unit (ALU). It performs arithmetic and logical operations such as addition, subtraction, AND, OR, XOR, and comparison based on the ALU control signal.

---

## 11. execute_branch_logic.v

Branch decision module. It compares register operands and determines whether a branch condition is satisfied for instructions like BEQ, BNE, BLT, and BGE.

---

## 12. data_memory.v

Data memory module used for load and store operations. It writes data into memory during store instructions and reads data during load instructions.

---

## 13. writeback_mux.v

Writeback multiplexer module. It selects whether the value written back to the register file comes from ALU result or data memory output.

---

## 14. tb_riscv_cpu_top.v

Top-level testbench used to simulate and verify the complete CPU functionality including arithmetic, memory access, and branch execution.

---

## 15. Other Individual Testbenches

Separate testbench files are used for validating each module independently such as ALU, register bank, control decoder, memory, and branch logic.


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
