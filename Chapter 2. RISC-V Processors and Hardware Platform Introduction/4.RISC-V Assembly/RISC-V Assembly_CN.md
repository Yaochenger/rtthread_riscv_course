# RISC-V汇编说明

通常用户开发应用程序很少需要编写汇编程序，但在操作系统移植和一些对代码执行效率有要求的场合，就需要使用汇编去实现该部分程序。在编写汇编代码除了汇编指令还需要结合一些伪操作来更好的编写汇编代码。接下来我们将分两部分来展开介绍，汇编伪操作与汇编指令。

# 汇编伪操作

1.伪指令1

```assembly
.file "filename"
```

示例:

```assembly
.file "main.s"  
  
.section .text  
.global _start  
  
_start:  
    # 用户汇编代码...
```

该指令告诉汇编器该汇编源文件名称为`main.s`,不同的汇编器和链接器对`.file`指令的支持程度可能有所不同。但是，作为一种广泛使用的汇编伪指令，它在许多工具链中都得到了支持，包括用于RISC-V架构的工具链。

2.伪指令2

```assembly
.global symbol_name
.globl symbol_name
```

`.global symbol_name` 或 `.globl symbol_name`（两者是等价的）的作用是将指定的符号（symbol）标记为全局的，这样它就可以在链接过程中被其他源文件或模块所识别和访问。

示例:

```assembly
/* utils.s */
.section .text  
.global print_hello 
  
print_hello:  
    li a7, 10         
    ecall             
    ret              

/* main.s */
.section .text  
.global _start 
  
_start:  
    call print_hello  
  
    li a7, 10  
    xori a0, a0, 0    
    ecall
```

在上面的例子中，`utils.s` 文件中的 `print_hello` 函数通过 `.global` 伪指令被标记为全局的，因此它可以在 `main.s` 文件中被调用。同样地，`main.s` 文件中的 `_start` 符号也被标记为全局的，这通常是链接器的要求，用于指定程序的入口点。

3.伪指令3

```assembly
.local symbol_name
```

`.local symbol_name` 伪指令的作用是将指定的符号（symbol）标记为局部的，表示该符号的可见性和作用域被限制在定义它的当前汇编文件或模块内。这有助于防止符号命名冲突，并确保符号只在需要它们的上下文中被访问。

示例：

```assembly
.section .text  
.local add_numbers  ; 将add_numbers标记为局部符号  
  
add_numbers:  
    ; 假设这里有一些将两个数相加的汇编代码  
    ; ...  
  
    ret  ; 返回  
  
; 在这个文件中，我们可以自由地使用add_numbers函数  
; 但是，在其他汇编文件中，我们不能直接调用它，因为它被标记为局部的
```

4.伪指令4

```assembly
.type name 
.type description
```

`.type` 伪指令的主要用途是在汇编代码中为符号（如函数、变量等）提供类型信息，这些信息对于调试器和其他工具（如性能分析器）来说是有用的，因为它们可以帮助这些工具更好地理解程序的结构和布局。

示例1：

```assembly
.section .text  
.global my_function  
.type my_function, @function  
  
my_function: 
	...
```

在这个示例中，`.type _start, @function`指示汇编器`_start`是一个函数。这有助于汇编器和链接器正确地处理函数调用和返回等操作。

示例2：

```assembly
.section .data  
.globl my_var  
.type my_var, @object  ; 假设的伪指令用法，用于说明my_var是一个对象（变量）  
my_var:  
    .word 1234          ; 初始化my_var为1234
```

在这个示例中，用于说明my_var是一个对象（变量）。

5.伪指令5

```assembly
.align integer
```

`.align integer`伪指令的作用是将当前程序计数器（PC）的地址推进到2的`integer`次方字节对齐的位置。这种地址对齐对于提高内存访问效率、满足特定硬件要求或满足某些操作系统的加载需求等方面非常重要。

示例：

```assembly
.section .text  
.align 4  
start:  
    addi x0, x0, 0
```

在这个示例中，`.align 4`将`start`标签的地址推进到下一个4字节对齐的位置。

6.伪指令6

```assembly
.zero integer
```

`.zero integer`伪操作的作用是从当前程序计数器（PC）地址处开始分配`integer`个字节的空间，并用0值填充这些空间。

示例：

```assembly
.section .bss  
.align 4  
my_array:  
    .zero 16  ; 分配16个字节的内存，并初始化为0
```

在这个示例中，`.zero 16`被用于`.bss`段（通常用于存储未初始化的全局变量，但在汇编中可以通过`.zero`进行显式初始化）中，分配了16个字节的内存空间，并将其初始化为0。

7.伪指令7

```assembly
.byte expression [, expression]*
```

`.byte`伪指令用于在当前位置插入一个或多个字节，并将它们初始化为指定的表达式值。这个伪指令在定义初始化的数据（如常量、字符串等）时非常有用。

示例：

```
.section .data  
my_bytes:  
    .byte 0x01, 0x02, 0x03, 0x04  ; 定义四个字节，值分别为0x01, 0x02, 0x03, 0x04
```

在这个示例中，`.byte`伪指令后面跟了四个表达式，分别定义了四个连续的字节。

8.伪指令8

```assembly
.word expression [, expression]*
```

`.word`伪操作是一个用于数据定义的重要指令，它的主要作用是从当前程序计数器（PC）地址处开始分配若干个四字节（word，即32位）的空间，并且用分号分隔的expression指定的值填充这些空间。空间分配的地址一定与四字节对齐。

示例：

```assembly
.section .data  
.word 0x12345678
```

这个例子中，`.word`伪操作在`.data`段中定义了一个四字节的立即数`0x12345678`。

9.伪指令9

```assembly
.string “string”
```

`.string` 伪指令（或有时可能以 `.ascii`、`.asciz`、或 `.byte`（用于逐字节定义，但不如 `.string` 直观）的形式出现，具体取决于汇编器的实现）用于定义并初始化一个字符串常量。`.string` 伪指令会自动在字符串的末尾添加一个空字符（null terminator, `\0`），这是C语言风格字符串的标准做法。

```assembly
.section .rodata  
hello_message:  
    .string "Hello, world!"
```

`.string "Hello, world!"` 伪指令定义了一个包含文本 "Hello, world!" 的字符串，并在其末尾自动添加了一个空字符 `\0`。

10.伪指令10

```assembly
.option {rvc,norvc,push,pop}
```

option伪操作用于设定某些架构特定的选项，使得汇编器能够识别此选项并按照选项的定义采取相应的行为。rvc、norvc是RISC-V架构特有的选项，用于控制是否生成16位宽的压缩指令.

“.option rvc”伪操作表示接下来的汇编程序可以被汇编生成16位宽的压缩指令.

“.option norvc”伪操作表示接下来的汇编程序不可以被汇编生成16位宽的压缩指令

push、pop用于临时性的保存或者恢复.option伪操作指定的选项。

“.option push”伪操作暂时将当前的选项设置保存起来，从而允许之后使用.option伪操作指定新的选项；而“.option pop”伪操作将最近保存的选项设置恢复出来重新生效。

11.伪指令11

```assembly
.section name [, subsection]
```

`.section` 伪指令用于指定接下来的指令或数据应该被放置在目标文件的哪个段（section）中。段是目标文件中用于组织代码和数据的一种结构，它们在链接过程中被合并到可执行文件或库中。

示例：

```assembly
.section .text  
start:  
    addi x0, x0, 0      
  
.section .data  
my_var:  
    .word 0x12345678    
```

上述示例代码中，.text指示后续的代码被加载至text段，.data  指示后续的数据放在data段。

至此汇编文件中典型的伪操作指令已经介绍完毕，合理使用汇编伪操作可以使汇编代码更加直观规范，便于调试。

# 汇编指令

RISC-V的汇编指令相较于其他精简指令集有较容易理解的特点。例如RISC-V的访存指令相比于ARM的访存指令仅包含单个数据的访存指令，而没有加载多个数据的指令也不存在其他伴附加的行为。在RISC-V指令集小节中已经对常用的RISC-V汇编指令进行了示例说明，这里我们介绍一些在汇编过程中较为复杂的一些指令：

1.指令1

```assembly
jal rd, lable
```

上述汇编指令用于无条件的跳转至lable标签所对应的函数，将下一条指令的地址写入rd寄存器，保存为返回地址。该指令属于 J型指令，指令中的立即数字段在编译阶段由编译器生成。

2.指令2

```assembly
 jalr rd, imm(rs1)
```

rs1寄存器所存的数据与imm立即数相加得到最终的跳转地址，使用jalr指令跳转至该地址指向的程序，将下一条指令的地址写入 rd寄存器，保存为返回地址。

3.指令3

```assembly
lui rd, imm
```

该指令会构造一个32bit长度的立即数，这个数据的高20位为指令中的立即数，低12位清零，然后将扩展后的立即数放到目标寄存器中。

示例：

```assembly
lui x1, 0x12345  
```

在这个例子中，`lui`指令将`0x12345`这个20位的立即数左移12位，并将结果`0x12345000`加载到寄存器`x1`中。

4.指令4

```assembly
auipc rd, imm
```

该指令构造一个32bit长度的立即数，这个立即数的高 20bit对应指令中的立即数，低12位清零，auipc指令将扩展后的立即数与pc相加，将相加后的立即数存放在rd目标寄存器中。

```assembly
auipc x2, 0x1000  
```

在这个例子中，假设PC的当前值是`0x80001000`（这只是一个假设值，实际值会由程序的执行状态决定）。`auipc`指令会将`0x1000`这个20位的立即数符号扩展为32位（因为RISC-V是32位架构），然后与PC的当前值相加。如果`0x1000`符号扩展后为`0x00001000`（在这个例子中，因为它是正数，所以符号扩展不会改变它），那么相加的结果就是`0x80001000 + 0x00001000 = 0x80002000`，这个结果会被存储到寄存器`x2`中。

5.指令5(伪指令)

```assembly
li rd，imm
```

这是一条伪指令，汇编器会根据具体的情况对应生成多条指令用于实现将该立即数加载至目标寄存器rd.

6.指令6

```assembly
la rd，lable
```

这是一条伪指令，汇编器会根据具体的情况对应生成多条指令用于实现将lable表示的函数的地址加载至目标寄存器rd。

上述就是RISC-V汇编中使用频率较高的的一些指令。在RISC-V规范中除了常规的寄存器，还有许多的控制状态寄存器(CSR)，在不修改现有的指令类型字段的情况下，RISC-CV采取了新增相应的CSR汇编指令去实现对CSR寄存器的操作。

7.指令7

```assembly
csrrw rd, csr, rs
```

该指令将csr寄存器的值读入rd目的寄存器，然后将rs1寄存器的值写入csr寄存器。

8.指令8

```assembly
csrrwi rd, csr, zimm
```

该指令将csr寄存器的值读入rd目的寄存器，然后把立即数zimm的值写入csr寄存器。zimm仅有5位，再计算时需进行零扩展。

9.指令9

```assembly
csrrs rd, csr, rs
```

该指令将csr寄存器的值读入rd目的寄存器，然后将rs寄存器的值与csr寄存器的值进行按位或运算，并将结果写入csr寄存器。

10.指令10

```assembly
csrrsi rd, csr, zimm
```

该指令将csr寄存器的值读入rd目的寄存器，把5位立即数zimm零扩展至32位，然后将扩展后的立即数的值与csr寄存器的值进行按位或运算，并将结果写入csr寄存器。

11.指令11

```assembly
csrrc rd, csr, rs
```

该指令将csr寄存器的值读入rd目的寄存器，将rs寄存器的值按位取反后与csr寄存器的值进行按位与运算，并将结果写入csr寄存器。

12.指令12

```assembly
csrrci rd, csr, zimm
```

该指令将csr寄存器的值读入rd目的寄存器，把5位立即数zimm零扩展至32位并按位取反后与csr寄存器的值进行按位与运算，并将结果写入csr寄存器。

上述就是我们对RISC-V汇编的一个整体介绍，相信大家到这能对RISC-V汇编有一个整体宏观的了解。