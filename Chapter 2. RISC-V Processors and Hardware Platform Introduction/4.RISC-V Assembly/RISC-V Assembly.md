### RISC-V assembly description

Usually, users seldom need to write assembler to develop applications, but in the case of operating system porting and some occasions where the efficiency of code execution is required, it is necessary to use assembly to implement the part of the programme. In addition to writing assembly code, you need to combine some pseudo-operations to write assembly code better. Next we will be divided into two parts to develop the introduction, assembly pseudo-operations and assembly instructions.

### Assembly pseudo-operations

1. Pseudo instruction 1

```assembly
.file ‘filename’
```

Example.

```assembly
.file ‘main.s’  
  
.section .text  
.global _start  
  
_start.  
    # User assembly code...
```

This directive tells the assembler that the name of the assembly source file is `main.s`, and the level of support for the `.file` directive may vary between assemblers and linkers. However, as a widely used assembly pseudo-instruction, it is supported in many toolchains, including those for the RISC-V architecture.

2. Pseudo-instruction 2

```assembly
.global symbol_name
.globl symbol_name
```

The purpose of ``.global symbol_name`` or ``.globl symbol_name`` (both are equivalent) is to mark the specified symbol as global so that it can be recognised and accessed by other source files or modules during linking.

Example.

```assembly
/* utils.s */
.section .text  
.global print_hello 
  
print_hello.  
    ecall         
    ecall             
    ecall              

/* main.s */
.section .text  
.global _start 
  
_start.  
    call print_hello  
  
    xori a0, a0, 0  
    xori a0, a0, 0    
    ecall
```

In the above example, the `print_hello` function in the `utils.s` file is marked as global through the `.global` pseudo directive, so that it can be called from within the `main.s` file. Similarly, the `_start` symbol in the `main.s` file is marked as global, which is usually required by the linker to specify the entry point of the programme.

3. Pseudo instruction 3

```assembly
.local symbol_name
```

The effect of the ``.local symbol_name`` pseudo-instruction is to mark the specified symbol as local, indicating that the visibility and scope of the symbol is restricted to the current assembly file or module in which it is defined. This helps prevent symbol naming conflicts and ensures that symbols are only accessed in the context in which they are needed.

Example:

```assembly
.section .text  
.local add_numbers ; Marks add_numbers as local.  
  
add_numbers.  
    ; Suppose you have some assembly code that adds two numbers together.  
    ; ...  
  
    ret ; return  
  
; In this file, we are free to use the add_numbers function as we wish!  
; However, in other assembly files, we can't call it directly because it's marked as localised
``
```

4. Pseudo instruction 4

```assembly
.type name 
.type description
```

The main use of the `.type` pseudo-instructions is to provide type information for symbols (e.g., functions, variables, etc.) in assembly code, which is useful to debuggers and other tools (e.g., performance analysers) because they help these tools better understand the structure and layout of a program.

Example 1:

```assembly
.section .text  
.global my_function  
.type my_function, @function  
  
my_function. 
	...
```

In this example, `.type _start, @function` instructs the assembler that `_start` is a function. This helps the assembler and linker handle operations such as function calls and returns correctly.

Example 2:

```assembly
.section .data  
.globl my_var  
.type my_var, @object ; Hypothetical pseudo-instruction usage for stating that my_var is an object (variable)  
my_var.  
    .word 1234 ; initialise my_var to 1234
``.

In this example, used to state that my_var is an object (variable).
```

5. Pseudo instruction 5

```assembly
.align integer
```

The `.align integer` pseudo-instruction serves to advance the address of the current program counter (PC) to a location aligned to the 2nd `integer` power byte. This address alignment is important for improving the efficiency of memory accesses, meeting specific hardware requirements, or meeting the loading needs of certain operating systems.

Example:

```assembly
.section .text  
.align 4  
start.  
    addi x0, x0, 0
```

In this example, `.align 4` advances the address of the `start` tag to the next 4-byte aligned position.

6. Pseudo instruction 6

```assembly
.zero integer
```

The `.zero integer` pseudo-operation serves to allocate `integer` bytes of space starting at the current program counter (PC) address and fills those spaces with zero values.

Example:

```assembly
.section .bss  
.align 4  
my_array.  
    .zero 16 ; allocate 16 bytes of memory and initialise to 0
```

In this example, `.zero 16` is used in the `.bss` segment (which is normally used to store uninitialised global variables, but can be explicitly initialised in assembly with `.zero`), allocating 16 bytes of memory space and initialising it to 0.

7. Pseudo-instruction 7

```assembly
.byte expression [, expression]*
```

The `.byte` pseudo-instruction is used to insert one or more bytes at the current location and initialise them to the specified expression value. This pseudo instruction is useful when defining initialised data (such as constants, strings, etc.).

Example:

```c
.section .data  
my_bytes.  
    .byte 0x01, 0x02, 0x03, 0x04 ; define four bytes with values 0x01, 0x02, 0x03, 0x04
In this example, the `.byte` pseudo-instruction is followed by four expressions that define four consecutive bytes.

```

8. Pseudo-instruction 8

```assembly
.word expression [, expression]*
```

The ``.word`` pseudo-operation is an important instruction for data definition; its main function is to allocate a number of four-byte (word, i.e., 32-bit) spaces starting at the address of the current program counter (PC) and to fill these spaces with the values specified by the semicolon-separated expression. The address of the space allocation must be aligned with the four bytes.

Example:

```assembly
.section .data  
.word 0x12345678
```

In this example, the `.word` pseudo-operation defines a four-byte immediate number `0x12345678` in the `.data` segment.

9. Pseudo instruction 9

```assembly
.string ‘string’
```

The `.string` pseudo-instruction (or sometimes it may appear as `.ascii`, `.asciz`, or `.byte` (for byte-by-byte definitions, but not as intuitive as `.string`, depending on the assembler's implementation) is used to define and initialise a string constant. The `.string` pseudo-instruction automatically adds a null terminator (`\0`) to the end of the string, which is standard practice for C-style strings.

```
.section .rodata  
hello_message.  
    .string ‘Hello, world!’
```

The `.string ‘Hello, world!’` pseudo-instruction defines a string containing the text ‘Hello, world!’ with a null character `\0` automatically added at the end.

10. Pseudo-instruction 10

```c
.option {rvc,norvc,push,pop}
```

The option pseudo-operation is used to set certain architecture-specific options so that the assembler recognises the option and behaves accordingly. rvc, norvc are RISC-V architecture-specific options that control whether or not to generate 16-bit wide compressed instructions.

The ‘.option rvc’ pseudo-operation indicates that the following assembler can be assembled to generate 16-bit wide compressed instructions.

The ‘.option norvc'pseudo-operation indicates that the following assembler may not be assembled to generate 16-bit wide compressed instructions.

push and pop are used to temporarily save or restore the options specified by the .option pseudo-operation.

The ‘.option push’ pseudo-operation temporarily saves the current option settings, allowing new options to be specified later with the .option pseudo-operation, while the ‘.option pop’ pseudo-operation restores the recently saved option settings to take effect. pop’ pseudo-operation restores the recently saved option settings to take effect again.

11. Pseudo instruction 11

```assembly
.section name [, subsection]
```

The ``.section`` pseudo directive is used to specify in which section of the target file the next instruction or data should be placed. Sections are structures in the target file used to organise code and data that are merged into the executable or library during the linking process.

Example:

```assembly
.section .text  
start.  
    addi x0, x0, 0      
  
.section .data  
my_var.  
    .word 0x12345678    
```

the above sample code, .text instructions for subsequent code is loaded into the text section, .data instructions for subsequent data in the data section.

So far the typical assembly file pseudo-operation instructions have been introduced, the reasonable use of assembly pseudo-operation can make the assembly code more intuitive specification, easy to debug.

### Assembly instructions

RISC-V's assembly instructions have easier to understand features compared to other compact instruction sets. For example, RISC-V's access instructions contain only single data access instructions compared to ARM's access instructions, and instructions that do not load more than one piece of data do not have other companion behaviours. In the RISC-V instruction set subsection has been commonly used RISC-V assembly instructions for example, here we introduce some of the more complex in the assembly process of some of the instructions:

1.Instruction 1

```assembly
jal rd, lable
```

The above assembly instruction is used to jump unconditionally to the function corresponding to the lable label, write the address of the next instruction to the rd register and save it as the return address. This instruction is a J-type instruction and the immediate numeric field in the instruction is generated by the compiler during the compilation phase.

2. Instruction 2

```assembly
 jalr rd, imm(rs1)
```

The data stored in the rs1 register is summed with the imm immediate number to get the final jump address. The jalr instruction is used to jump to the programme pointed to by this address, and the address of the next instruction is written to the rd register and saved as the return address.

3. Instruction 3

```assembly
lui rd, imm
```

This instruction constructs a 32bit length immediate number, the high 20 bits of this data are the immediate number in the instruction, the low 12 bits are cleared to zero, and then the expanded immediate number is put into the target register.

Example:

```assembly
lui x1, 0x12345  
```

In this example, the ``lui`` instruction shifts ``0x12345``, a 20-bit immediate number, 12 bits to the left and loads the result, ``0x12345000``, into register ``x1``.

4.Instruction 4

```assembly
auipc rd, imm
```

This instruction constructs a 32bit length immediate number, the high 20bit of this immediate number corresponds to the immediate number in the instruction, and the low 12bit is cleared to zero, the auipc instruction adds the expanded immediate number to pc, and stores the added immediate number in the rd target register.

```assembly
auipc x2, 0x1000  
```

In this example, assume that the current value of PC is `0x80001000` (this is an assumption; the actual value will be determined by the execution state of the program). The `auipc` instruction expands the `0x1000`, a 20-bit immediate number symbol, to 32 bits (since RISC-V is a 32-bit architecture) and then adds it to the current value of the PC. If the `0x1000` symbol is expanded to `0x00001000` (in this example, since it is a positive number, the symbol expansion does not change it), then the result of the addition is `0x80001000 + 0x00001000 = 0x80002000`, and this result is stored in register `x2`.

5. Directive 5 (pseudo-directive)

```assembly
li rd, imm
```

This is a pseudo-instruction, and the assembler will generate several instructions for loading the immediate number into the target register rd, depending on the specific situation.

6. Instruction 6

```assembly
la rd, lable
```

This is a pseudo-instruction, the assembler will generate multiple instructions to load the address of the function represented by lable into the target register rd according to the specific situation.

These are some of the more frequently used instructions in RISC-V assembly. In addition to the regular registers in the RISC-V specification, there are also a number of control status registers (CSRs). Without modifying the existing instruction type field, RISC-CV adopts the addition of the corresponding CSR assembly instructions to achieve the operation of the CSR registers.

7. Instruction 7

```assembly
csrrw rd, csr, rs
```

This instruction reads the value of the csr register into the rd destination register and then writes the value of the rs1 register into the csr register.

8. Instruction 8

```assembly
csrrwi rd, csr, zimm
```

This instruction reads the value of the csr register into the rd destination register, and then writes the value of the immediate number zimm into the csr register. zimm has only 5 bits, and zero expansion is required for recalculation.

9. Instruction 9

```assembly
csrrs rd, csr, rsThis instruction reads the value of the csr register into the rd destination register, then performs a bitwise or operation between the value of the rs register and the value of the csr register, and writes the result to the csr register.
```

10. Instruction 10

```assembly
csrrsi rd, csr, zimm
```

This instruction reads the value of the csr register into the rd destination register, zero-extends the 5-bit immediate number zimm to 32 bits, and then performs a bitwise-or operation between the value of the extended immediate number and the value of the csr register and writes the result to the csr register.

11. Directive 11

```assembly
csrrc rd, csr, rs
```

This instruction reads the value of the csr register into the rd destination register, inverts the value of the rs register bitwise and performs a bitwise sum operation with the value of the csr register, and writes the result to the csr register.

12. Instruction 12

```assembly
csrrci rd, csr, zimm
```

This instruction reads the value of the csr register into the rd destination register, zero-extends the 5-bit immediate number zimm to 32 bits and inverts it bit-wise to perform a bit-wise sum operation with the value of the csr register, and writes the result to the csr register.

The above is our RISC-V assembly of an overall introduction, I believe that we can get to this RISC-V assembly has an overall macro-understanding.