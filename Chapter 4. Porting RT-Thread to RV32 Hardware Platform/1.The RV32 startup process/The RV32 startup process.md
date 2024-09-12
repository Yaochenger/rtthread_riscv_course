### RISC-V boot process description

The boot process of RISC-V MCUs is also very simple and easy to understand, and the regular boot process of RISC-V MCUs is introduced here. The regular boot process involves data copying from the storage media, mode configuration and some initialisation of the system.

Next, we will introduce a few essential operations in the RISC-V boot process.

#### 1. Initialise gp global pointer registers

The following piece of code is executed during the startup phase:

```assembly
	la gp, __global_pointer$
```

代码中的__global_pointer$符号来自于启动文件，示例如下：

```livescript
	.data :
	{
    	*(.gnu.linkonce.r.*)
    	*(.data .data.*)
    	*(.gnu.linkonce.d.*)
		. = ALIGN(8);
    	PROVIDE( __global_pointer$ = . + 0x800 );
    	*(.sdata .sdata.*)
		*(.sdata2.*)
    	*(.gnu.linkonce.s.*)
    	. = ALIGN(8);
    	*(.srodata.cst16)
    	*(.srodata.cst8)
    	*(.srodata.cst4)
    	*(.srodata.cst2)
    	*(.srodata .srodata.*)
    	. = ALIGN(4);
		PROVIDE( _edata = .);
	} >RAM AT>FLASH
```

We can see that the symbol is defined in the data segment. In describing the role of this register we need to introduce a compilation technique: linker slack; the relative address field in a jump-and-link instruction is 20 bits wide, so a single instruction is enough to jump far into the relative address field. Usually the compiler generates two instructions for each external function jump, and in many cases a single instruction will suffice. Optimising from two instructions to one saves both overheads, so the linker scans the code a few times to replace two instructions with one if possible. This reduces the distance between the function and the location where it is called, and the linker scans the replacement several times until the code no longer changes. This process is called linker slack.

For data accesses within ±2KiB of the gp pointer, the compiler adds the `-msmall-data-limit=n` parameter, with which the compiler puts static variables with less than n bytes of memory space into the `.sdata` or `.sdata.*` section, and then the linker concentrates this portion of the static variables in the `__global_ pointer$` +/- 2K.

This way, for data accesses in the ±2KiB range of the gp pointer, the RISC-V linker replaces the lui and auipc instructions with pointers in the global pointer register.

#### 2. Initialise the stack pointer

The sample code is as follows:

```assembly
	la sp, _eusrstack 
```

After completing the above operations, the system stacking and stacking will be performed in the address space pointed by the pointer in the sp register.

#### 3. Data Segment Copy and BSS Segment Initialisation

As in other architectures, the microcontroller has to copy the data segments to RAM at the boot stage and initialise uninitialised global variables in RAM.

The following assembly code is the implementation of this process:

```assembly
2:
	/* Load data section from flash to RAM */
	la a0, _data_lma
	la a1, _data_vma
	la a2, _edata
	bgeu a1, a2, 2f
1:
	lw t0, (a0)
	sw t0, (a1)
	addi a0, a0, 4
	addi a1, a1, 4
	bltu a1, a2, 1b
2:
	/* Clear bss section */
	la a0, _sbss
	la a1, _ebss
	bgeu a0, a1, 2f
1:
	sw zero, (a0)
	addi a0, a0, 4
	bltu a0, a1, 1b
```

The symbols `_data_lma` ,`_data_vma` in the above code are from the linking script, the linking script is the setup of the layout of our code in the memory, here is the location of the above symbols in the linking script.

```
	.dalign :
	{
		. = ALIGN(4);
		PROVIDE(_data_vma = .);
	} >RAM AT>FLASH	

	.dlalign :
	{
		. = ALIGN(4); 
		PROVIDE(_data_lma = .);
	} >FLASH AT>FLASH

	.data :
	{
    	*(.gnu.linkonce.r.*)
    	*(.data .data.*)
    	*(.gnu.linkonce.d.*)
		. = ALIGN(8);
    	PROVIDE( __global_pointer$ = . + 0x800 );
    	*(.sdata .sdata.*)
		*(.sdata2.*)
    	*(.gnu.linkonce.s.*)
    	. = ALIGN(8);
    	*(.srodata.cst16)
    	*(.srodata.cst8)
    	*(.srodata.cst4)
    	*(.srodata.cst2)
    	*(.srodata .srodata.*)
    	. = ALIGN(4);
		PROVIDE( _edata = .);
	} >RAM AT>FLASH

	.bss :
	{
		. = ALIGN(4);
		PROVIDE( _sbss = .);
  	    *(.sbss*)
        *(.gnu.linkonce.sb.*)
		*(.bss*)
     	*(.gnu.linkonce.b.*)		
		*(COMMON*)
		. = ALIGN(4);
		PROVIDE( _ebss = .);
	} >RAM AT>FLASH
```

#### 4. Machine operating mode and exception address setting

Before starting the system, the operating mode of the machine must be set explicitly. The operating mode of the risc-v architecture is set by the control status register mstatus, which is also involved in the configuration of the FPU and global interrupts.

The assembly code to configure this register is as follows:：

```assembly
   	li t0, 0x7800
   	csrw mstatus, t0
```

The interrupt controller of RISC-V usually supports vector interrupts and non-vector interrupts, the specific use of which mode of operation in the work depends on the implementation of the chip implementation of the registers configured interrupt management mode, here we only introduce when the system triggers an exception to jump to the execution of the base address, exceptions and interrupts are uniformly referred to as exceptions in the RISC-V.

When the system is configured for vector management mode, the exception base address register (mtvec) holds the base address of the vector table; when the system is configured for non-vector management mode, the exception base address register (mtvec) holds the unified handling function after an exception;

The assembly code to configure this register is as follows:

```assembly
 	la t0, _vector_base
    ori t0, t0, 3           
	csrw mtvec, t0
```

#### 5. Custom Configuration

The risc-v architecture specification allows chip designers to customise registers. By configuring these registers, the chip can be configured with some custom features, such as instruction prediction, hardware stacking, etc. The following is some sample code for configuring custom registers:

```assembly
/* Configure pipelining and instruction prediction */
    li t0, 0x1f
    csrw 0xbc0, t0

/* Enable interrupt nesting and hardware stack */
	li t0, 0x1f
	csrw 0x804, t0
```

The 0xbc0 and 0x804 in the above code are the numbers of the customised registers.

#### 6. Load Setup

After completing the above configuration, the system clock initialisation and other configurations are usually performed, and then the mepc register is assigned a value. After executing the mret instruction, the value of the mepc register is assigned to the pc register, and the system jumps to the address pointed to by the pointer in the pc register, and at the same time, the configuration of the mstatus register is also loaded to allow the system to work in user-configured mode.

The above is the overall boot process of a RISC-V architecture chip.

### RISC-V boot flowchart

![](figures/start_flowchart.png)

