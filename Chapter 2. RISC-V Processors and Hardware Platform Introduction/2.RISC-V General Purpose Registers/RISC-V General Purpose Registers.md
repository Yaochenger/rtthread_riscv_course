### RISC-V Register Descriptions

The RISC-V instruction set defines 32 integer registers, x0~X31, and a program counter (PC) to hold the address of the current instruction. The width of the registers is determined by the width of the CPU's address bus. the width of the integer registers is 32 bits for RV32 and 64 bits for RV64. the following table shows the names of the 32 integer general-purpose registers, the ABI names, and the function descriptions:

| Register | ABI Name | Clarification                                                | Whether or not to keep the |
| -------- | -------- | ------------------------------------------------------------ | -------------------------- |
| x0       | zero     | 0 value register, hard-coded to 0, write data ignored, read data 0 | -                          |
| x1       | ra       | Used for return address                                      | No                         |
| x2       | sp       | Used for stack pointers.                                     | -                          |
| x3       | gp       | Used for global pointer）                                    | -                          |
| x4       | tp       | Used for thread pointers                                     | No                         |
| x5       | t0       | Used to store temporary data or alternate link registers     | Yes                        |
| x6~x7    | t1~t2    | Used to store temporary data registers                       | No                         |
| x8       | s0/fp    | Registers or frame pointer registers to be saved             | Yes                        |
| x9       | s1       | Need to save registers                                       | Yes                        |
| x10~x11  | a0~a1    | Function parameter or return value registers                 | No                         |
| x12~x17  | a2-a7    | Function Passing Argument Register                           | No                         |
| x18~x27  | s2-s11   | Registers to be saved                                        | Yes                        |
| x28~x31  | t3~t6    | Used to store temporary data registers                       | No                         |

If the system supports floating-point expansion, the system will increase 32 floating-point registers, usually if only single-precision floating-point support, the length of the floating-point register is 32bit, if double-precision floating-point support, the width of the floating-point register is 64bit, at this time, single-precision floating-point operations only use the lower 32bit of the floating-point registers.

The following table shows the names of the 32 floating-point registers, ABI names and function descriptions:

| Register | ABI Name | Clarification                                  | Whether or not to keep the |
| -------- | -------- | ---------------------------------------------- | -------------------------- |
| f0-7     | ft0-7    | Floating Point Temporary Register              | No                         |
| f8-9     | fs0-1    | Floating Point Saving Register                 | Yes                        |
| f10-11   | fa0-1    | Floating Point Parameter/Return Value Register | No                         |
| f12-17   | fa2-7    | Floating Point Parameter Register              | No                         |
| f18-27   | fs2-11   | Floating Point Saving Register                 | Yes                        |
| f28-31   | ft8-11   | Floating Point Temporary Register              | No                         |

When we do RTOS porting, we need to implement thread switching up and down, and we need to save the CPU scene when we do the context switching, and the so-called CPU scene is the registers mentioned above as well as some control state registers. Here we introduce the role of the above registers in the function call specification in the figure, which makes it easy for us to load the data in the saved stack space to the registers when restoring the scene.

