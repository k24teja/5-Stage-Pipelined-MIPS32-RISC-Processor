#  Design of 5-Stage Pipelined MIPS32 RISC Processor

This repository contains the design and Verilog implementation of a **MIPS32 ISA-based RISC Processor** using a **5-stage pipelined architecture**.

---

##  Table of Contents

* MIPS32 Architecture
* Addressing Modes
* Instructions Considered
* Instruction Encoding
* Stages of Execution
* Example Program (Testbench)
* Known Issues and Limitations
* References

---

##  MIPS32 Architecture

* 32 × 32-bit General Purpose Registers (`R0` to `R31`)
* `R0` is hardwired to logic `0`
* 32-bit Program Counter (PC)
* No flag registers (carry, zero, sign, etc.)
* Limited addressing modes
* Only **Load/Store instructions access memory**
* Memory is **word-addressable (32-bit)**

---

##  Addressing Modes

| Addressing Mode      | Example Instruction |
| -------------------- | ------------------- |
| Register Addressing  | `ADD R1, R2, R3`    |
| Immediate Addressing | `ADDI R1, R2, 200`  |
| Base Addressing      | `LW R5, 150(R7)`    |
| PC Relative          | `BEQZ R3, Label`    |
| Pseudo Direct        | `J Label`           |

---

##  Instructions Considered

### 🔹 Load & Store

```
LW R2,124(R8)     // R2 = Mem[R8 + 124]
SW R5,-10(R25)   // Mem[R25 - 10] = R5
```

### 🔹 Arithmetic & Logic (Register)

```
ADD R1,R2,R3
SUB R12,R10,R8
AND R20,R1,R5
OR  R11,R5,R6
MUL R5,R6,R7
SLT R5,R11,R12
```

### 🔹 Arithmetic & Logic (Immediate)

```
ADDI R1,R2,25
SUBI R5,R1,150
SLTI R2,R10,10
```

### 🔹 Branch Instructions

```
BEQZ R1,Loop
BNEQZ R5,Label
```

### 🔹 Jump

```
J Loop
```

### 🔹 Miscellaneous

```
HLT   // Halt execution
```

---

##  Instruction Encoding

* Instructions follow MIPS32 format
* Fields include: `opcode`, `rs`, `rt`, `rd`, `shamt`, `funct`
* Immediate values are **sign-extended to 32 bits**
* Register operands are fetched in parallel during decode stage
  <img width="966" height="542" alt="instruction_encoding" src="https://github.com/user-attachments/assets/5f0a05bf-1ec7-4713-b724-289c2c2e12d2" />


---
##  Pipelied diagram 
  <img width="966" height="542" alt="Pipelined_diagram" src="https://github.com/user-attachments/assets/7a5dd684-897c-406e-a60b-ef599c7cf0b4" />

##  Stages of Execution

The processor uses a **5-stage pipeline**:

1. **IF (Instruction Fetch)**

   * Fetch instruction from memory
   * Increment PC

2. **ID (Instruction Decode / Register Fetch)**

   * Decode instruction
   * Read registers

3. **EX (Execution / Address Calculation)**

   * Perform ALU operations

4. **MEM (Memory Access / Branch Completion)**

   * Load/store operations

5. **WB (Write Back)**

   * Write results to register file

---
##  Example Program (Testbench)

### Steps:

* Initialize `R1 = 10`
* Initialize `R2 = 20`
* Initialize `R3 = 25`
* Compute sum → store in `R5`

---

### Instructions

| Assembly          | Hex Code   |
| ----------------- | ---------- |
| ADDI R1,R0,10     | `2801000a` |
| ADDI R2,R0,20     | `28020014` |
| ADDI R3,R0,25     | `28030019` |
| OR R7,R7,R7 (NOP) | `0ce77800` |
| OR R7,R7,R7 (NOP) | `0ce77800` |
| ADD R4,R1,R2      | `00222000` |
| OR R7,R7,R7 (NOP) | `0ce77800` |
| ADD R5,R4,R3      | `00832800` |
| HLT               | `fc000000` |

---

### Consol Output

```
R1 - 10
R2 - 20
R3 - 25
R4 - 30
R5 - 55
```

---

##  Known Issues and Limitations

The current design does **not handle pipeline hazards**, including:

*  Structural Hazards
*  Data Hazards
*  Control Hazards

 These are temporarily avoided using **NOP (dummy) instructions**

---

##  References

* NPTEL Course: *Hardware Modeling using Verilog*
* Instructor: **Prof. Indranil Sengupta (IIT KGP)**


