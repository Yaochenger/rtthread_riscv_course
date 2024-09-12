### Introduction to the RISC-V Instruction Set

The RISC-V instruction set was invented in 2010 by Professor Krste Asanovic of the University of California, Berkeley, USA, and developers such as Yunsup Lee.The RISC-V instruction set is a compact instruction set.

There are two types of instruction sets: complex instruction sets and compact instruction sets. Complex instruction sets are characterised by the fact that a single instruction is enough to do a complex task, whereas compact instruction sets subdivide a complex task into several instructions. The use of complex instruction set will increase the complexity of CPU design and reduce the capacity and complexity of the code to a certain extent, while the compact instruction set needs several instructions to complete the same matter, which will increase the capacity and even the complexity of the code. However, from the perspective of development trend, thin instruction set is the main development direction at present.

The unique feature of RISC-V instruction set is that it is a modular instruction set, RV32I instruction set is necessary to implement, only using RV32I instruction set can achieve a complete software stack. CPU designers can choose the instruction module to be implemented according to the actual needs.

### RISC-V instruction format

- RISC-V has six basic instruction formats: `R`, `I`, `S`, `B`, `U`, `J`.

  - R-type instructions - used for register-to-register arithmetic operations;
  - I-type instructions - for register-to-register arithmetic operations and read (load) memory operations;
  - S-type instructions - used to write (store) memory;
  - B-type instructions - for conditional jump operations;
  - U-type instructions - for high 20-bit immediate number (long immediate number) operations;
  - J-type instructions - for unconditional jump operations.

  There are only six formats of RISC-V instructions, and their instruction formats have the following characteristics:

  - All instructions are 32 bits in length, and the lower 7 bits of the instruction are fixed as opcode (operation code), which simplifies instruction decoding with this design.
  - The identifiers for reading and writing registers are in the same location, so the registers can be accessed before decoding the instruction.
  - The immediate numeric segment is a sign extension, and the sign bit is kept in the highest bit.

The following figure shows the composition of the 6 instruction structures.

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\formats.png)

Next we look at the meaning of each field in the above instruction structure:

- opcode: the opcode of the instruction, this field represents the type of the instruction.
- rd: destination operation register, this field holds the encoding of the destination register, which is used to hold the result of the operation.
- funct3: function code field, occupies 3bit.
- rs1: first source operand register.
- rs2: second source operand register.
- funct7: function code field, occupies 7bit.
- imm: immediate digital field.

After learning the format of RISC-V instructions, we will introduce each instruction separately in the next section.

### RISC-V instructions - R-type instructions

The lowest 7 bits of a RISC-V instruction are fixed to the opcode, and identifying the opcode identifies whether the instruction is an R-type instruction or not.

According to the funct3 field and funct7 field, the specific execution operation can be derived.

According to rs1, rs2 and rd, the specific operation can be executed.

The R-type instructions of RV32 are as follows:

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I.png)

Examples of the use of R-type instructions:

1.add the values in registers a1 and a2 and store the result in register a0 Example:

```assembly
//a0 = a1 + a2
add a0, a1, a2 
```

2.Subtract the value in register a1 from the value in register a2 and store the result in register a0. Example:

```assembly
//a0 = a1 - a2
sub a0, a1, a2
```

3.Shift the value in register a1 by a2 bits to the left, lower by 0, and store the result in register a0. Example:

```assembly
//a0 = a1 << a2
sll a0, a1, a2
```

4.Shift the value in register a1 right by a2 bits, high by 0, and store the result in register a0. Example:

```assembly
//a0 = a1 >> a2
srl a0, a1, a2 
```

5.Shift the value in register a1 arithmetically right by a2 bits, high complement the original sign bit, and store the result in register a0, example:

```assembly
//a0 = a1 >> a2
sra a0, a1, a2
```

6.If the value in register a1 is less than the value in register a2, set register a0 to 1, otherwise set it to 0. Example:

```assembly
//a1 < a2 ? a0 = 1 : a0 = 0
slt a0, a1, a2
```

7.Bitwise differentiate the values in registers a1 and a2 and store the result in register a0. Example:

```assembly
//a0 = a1 ^ a2
xor a0, a1, a2
```

8.Bitwise or the values in registers a1 and a2 and store the result in register a0. Example:

```assembly
//a0 = a1 | a2
or  a0, a1, a2
```

9.Compare the values in registers a1 and a2 bitwise and store the result in register a0. Example:

```assembly
//a0 = a1 & a2
and a0, a1, a2
```

### RISC-V instructions - Type I instructions

The lowest 7 bits of a RISC-V instruction are fixed to the opcode, and the opcode identifies whether the instruction is a Type I instruction or not.

According to the funct3 field, the specific execution operation can be derived.

The destination register is 5 bits wide and is in the 7-11 bits of the instruction.

rs1 is the first source operand register, bit width 5bit, at 15-19bit of the instruction.

imm[11:0] holds the 12-bit immediate number, at 20-31bit of the instruction.

The RV32I's Type I instruction instructions are as follows:

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I_I.png)

Example of the use of I-type instructions:

1.Add register a1 and immediate number 0x5 and store the result in register a0. Example:

```assembly
//a0 = a1 + 0x5
addi a0, a1, 0x5
```

2.Subtract the immediate number 0x05 from the value in register a1 and store the result in register a0. Example:

```assembly
//a0 = a1 - 0x05
subi a0, a1, 0x05
```

3.Shift the value in register a1 by 0x05 bits to the left, lower by 0, and store the result in register a0. Example:

```assembly
//a0 = a1 << 0x05(低位补0)
slli a0, a1, 0x05
```

4.Shift the value in register a1 right by 0x05 bits, high by 0, and store the result in register a0. Example:

```assembly
//a0 = a1 >> 0x05(高位补0)
srli a0, a1, 0x05
```

5.The value in register a1 is arithmetically shifted to the right by 0x05 bits, the high bit is complemented by the original sign bit, and the result is stored in register a0. Example:

```assembly
//a0 = a1 >> 0x05 (算术右移，高位补原来的符号位)
srai a0, a1, 0x05
```

6.If the value in register a1 is less than the immediate number 0x05, set register a0 to 1, otherwise set it to 0. Example:

```assembly
//a1 < 0x05 ? a0 = 1 : a0 = 0
slti a0, a1, 0x05
```

7.Bitwise differentiate between the value in register a1 and the immediate number 0x05 and store the result in register a0. Example:

```assembly
//a0 = a1 ^ 0x05
xori a0, a1, 0x05
```

8.Bitwise or the value in register a1 and immediate number 0x05 and store the result in register a0. Example:

```assembly
//a0 = a1 | 0x05
ori a0, a1, 0x05
```

9.Compare the values in register a1 and immediate number 0x05 bitwise and store the result in register a0. Example:

```assembly
//a0 = a1 & 0x05
andi a0, a1, 0x05
```

The load instruction:

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I_I_load.png)

Example of using the I-load instruction:

1.Add 0 to the value of register x1 as an address, take an 8-bit value from the memory corresponding to this address, and assign this value to register x10. Example:

  ```  assembly
lb x10,  0(x1)  
  ```

2.Add 0 to the value of register x1 as an address, take the 16-bit value from the memory corresponding to this address, and assign this value to register x10. Example:

```assembly
lh x10, 0(x1)
```

3.Add 0 to the value of register x1 as an address, take the 32-bit value from the memory corresponding to this address, and assign this value to register x10. Example:

```assembly
lw x10, 0(x1)
```

4.Add 0 to the value of register x1 as the address, take the 8-bit unsigned value from the memory corresponding to this address, and assign this value to register x10. Example:

```assembly
lbu x10, 0(x1)
```

5.Add 0 to the value of register x1 as an address, take the 16-bit unsigned value from the memory corresponding to this address, and assign this value to register x10. Example:

```assembly
lhu x10, 0(x1)
```

### RISC-V instructions - S-type instructions

The lowest 7 bits of a RISC-V instruction are fixed to the opcode, and the opcode identifies whether the instruction is an S-type instruction or not.

According to the funct3 field, the specific execution operation can be derived.

The destination register is 5 bits wide and is in the 7-11 bits of the instruction.

rs1 is the first source operand register, bit width 5bit, at bit 15-19 of the instruction.

rs2 is the second source operand register, bit-wise 5bit, at 25-31bit of the instruction.

imm[4:0]+imm[11:5] hold 12-bit immediate numbers.

The RV32I S-type instructions are as follows:

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I_S.png)

Example of using I-type instruction:

1.Add 0 to the value of register x1 as the address, and store the value of register x10 into the memory corresponding to the above address (only the lower 8 bits of the value of x10 will be written), Example:

```assembly
sb x10, 0(x1)
```

2.Add 0 to the value of register x1 as the address, and store the value of register x10 into the memory corresponding to the above address (only the lower 16 bits of the value of x10 will be written), Example:

```assembly
sh x10, 0(x1)
```

3.Add 0 to the value of register x1 as the address, and store the value of register x10 into the memory corresponding to the above address (only the lower 32 bits of the value of x10 will be written), Example:

```assembly
sw x10, 0(x1)
```

### RISC-V instructions - Type B instructions

The lowest 7 bits of a RISC-V instruction are fixed to the opcode, and identifying the opcode identifies whether the instruction is a B-type instruction or not.

According to the funct3 field, the specific execution operation can be derived.

rs1 is the first source operand register, bit width 5bit, in the instruction 15-19bit.

rs2 is the second source operand register, bit width 5bit, at 25-31bit of the instruction.

imm[4:1]+imm[11]+imm[10:5]+imm[12] hold 12-bit immediate numbers.

The RV32I B-type instructions are as follows:

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I_B.png)

Example of using B-type instruction:

1.If the value of register `a1` is equal to the value of register `a2`, jump to `Label`, example:

```assembly
beq a1,a2,Label
```

2.If the value of register `a1` is not equal to the value of register `a2`, then jump to `Label`, example:

```assembly
bne a1,a2,Label 
```

3.If the value of register `a1` is less than the value of register `a2`, then jump to `Label`, example:

```assembly
blt a1,a2,Label
```

4.If the value of register `a1` is greater than the value of register `a2`, then jump to `Label`, example:

```assembly
bgt a1,a2,Label
```

5.If the value of register `a1` is greater than or equal to the value of register `a2`, then jump to `Label`, example:

```
bge a1,a2,Label
```

6.If the value of register `a1` is less than or equal to the value of register `a2`, then jump to `Label`, example:

```assembly
ble a1,a2,Label
```

### RISC-V instructions - U-type instructions

The lowest 7 bits of a RISC-V instruction are fixed to the opcode, which can be used to identify the instruction as a U-type instruction.

The destination register is 5 bits wide and is in the 7-11 bits of the instruction.

imm[31:12]: stores the high 20-bit immediate number, which is in the 12-31 bit of the instruction.

The U-type instruction of RV32I is as follows:

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I_U.png)

Example of using U instruction:

1.Get the high 20 bits of the immediate number, the low bit is complemented by 0. The immediate number range is: 0x00~0xFFFFF, example:

```assembly
lui  x10, 0x65432
```

### RISC-V instructions - J-type instructions

The lowest 7 bits of a RISC-V instruction are fixed to the opcode, which can be used to identify the instruction as a U-type instruction.

The destination register is 5 bits wide and is in bits 7-11 of the instruction.

imm[19:12]+imm[11]+imm[10:1]+imm[20]: store 20-bit immediate numbers.

The J-type instruction jal of the RV32I is as follows (the jalr instruction in the figure below belongs to the I-type instruction and is used here only as an analogy to the jal instruction):

![](E:\_RISC-V RT-Thread课程\rtthread_riscv_course\Chapter 2. RISC-V Processors and Hardware Platform Introduction\1.Introduction to the RISC-V Command Set\figures\RV32I_J.png)

Examples of using J-type instructions:

1.jumps to the location specified by the label `symbol` and stores the address of the next instruction in register `ra`, Example:

```assembly
jal ra, symbol
```

2.Jump to the address of pc + 100 * 2, and set ra as the return address pc relative addressing, corresponding to the location-independent code, example:

```assembly
jal ra, 100
```

3.Jump to the value of register `x10` plus 40 (bytes) and store the address of the next instruction in register `ra`, Example:

```assembly
jalr ra, 40(x10)
```

