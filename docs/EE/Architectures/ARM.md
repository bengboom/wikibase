![[ARM instructions.pdf]]


### ARM Registers

| Register | Description                   |
|----------|-------------------------------|
| R0       | Argument / Return value       |
| R1-R3    | Argument / Temporary variables|
| R4-R11   | Saved variables               |
| R12      | Temporary variable            |
| R13(SP)  | Stack Pointer                 |
| R14(LR)  | Link Register                 |
| R15(PC)  | Program Counter               |


### ARM Instructions
**Data Processing Instructions** cond4 + op2 + func6 + Rn4 + Rd4 + Src2 (12)

1.  `ADD`: Adds two values. Opcode 0100
2.  `SUB`: Subtracts one value from another. Opcode 0010
3.  `MUL`: Multiplies two values. Opcode 0110
4.  `DIV`: Divides one value by another. Opcode 0111
5.  `AND`: Performs a bitwise AND operation. Opcode 0000
6.  `ORR`: Performs a bitwise OR operation. Opcode 1100
7.  `EOR`: Performs a bitwise exclusive OR operation. Opcode 0001
8.  `MOV`: Moves a value from one register to another. Opcode 1101

**Branch Instructions** cond4 + op2 + offset24

1.  `B`: Unconditional branch to a label. Opcode 1010
2.  `BL`: Branch with link to a label (used for function calls). Opcode 1011
3.  `BEQ`: Branch if equal. Condition 0000
4.  `BNE`: Branch if not equal. Condition 0001
5.  `BGT`: Branch if greater than. Condition 1100
6.  `BLT`: Branch if less than. Condition 1011
7.  `BGE`: Branch if greater than or equal to. Condition 1010
8.  `BLE`: Branch if less than or equal to. Condition 1101

**Load/Store Instructions** cond4 + op2 + I + P + U + B + W + L + Rn4 + Rd4 + offset12

1.  `LDR`: Load a word from memory into a register. Opcode 0101, L=1
2.  `STR`: Store a word from a register into memory. Opcode 0101, L=0
3.  `LDM`: Load multiple words from memory into registers. Opcode 1000, L=1
4.  `STM`: Store multiple words from registers into memory. Opcode 1000, L=0

**Comparison Instructions** cond4 + op2 + func6 + Rn4 + Rd4 + Src2 (12)

1.  `CMP`: Compare two values (subtract them and set flags, but don't store the result). Opcode 1010
2.  `TST`: Test bits (AND them and set flags, but don't store the result). Opcode 1000
3.  `TEQ`: Test for equality (EOR them and set flags, but don't store the result). Opcode 1001
4.  `CMN`: Compare negative (ADD them and set flags, but don't store the result). Opcode 1011

**Shift Instructions** cond4 + op2 + func6 + Rn4 + Rd4 + Src2 (12)

1.  `LSL`: Logical shift left. Opcode 0000 in func6
2.  `LSR`: Logical shift right. Opcode 0001 in func6
3.  `ASR`: Arithmetic shift right. Opcode 0010 in func6
4.  `ROR`: Rotate right. Opcode 0011 in func6