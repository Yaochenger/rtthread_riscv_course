### RT-Thread Base Function Porting Interface Adaptation (RISC-V)

In this subsection we will introduce function interfaces other than those for context switching, which are relatively simple to implement but play an important role in the system.

### Switching global interrupts

During system operation, many scenarios will use the global interrupt switch, for example, the protection of critical resources can be protected by switching interrupts in some scenarios. In simple porting scenarios, interrupt nesting is usually not supported, so you need to switch global interrupts off after entering an interrupt, and then switch global interrupts on after completing interrupt processing.

The interface to switch global interrupt in rt-thread is as follows:

```c
rt_base_t rt_hw_interrupt_disable(void);
void rt_hw_interrupt_enable(rt_base_t level);
```

Using the function interface to switch off global interrupts returns the function before switching off interrupts; using the function interface to enable global interrupts restores the state before switching off interrupts.

This part of the function interface is usually implemented in assembly to ensure that the code for switching global interrupts on and off is efficient and concise.

In the RISC-V architecture, the following implementation can be used:

```assembly
/*
 * rt_base_t rt_hw_interrupt_disable(void);
 */
    .globl rt_hw_interrupt_disable
rt_hw_interrupt_disable:
    csrrci a0, mstatus, 8
    ret

/*
 * void rt_hw_interrupt_enable(rt_base_t level);
 */
    .globl rt_hw_interrupt_enable
rt_hw_interrupt_enable:
    csrw mstatus, a0
    ret

```

### Thread Stack Frames

When a thread makes a context switch, it will be saved according to the constructed thread stack frame, only the order of the thread stack frame can be saved to achieve the correct recovery of the thread thread, the following code is the RT-Thread for RISC-V set up the thread stack frame, we will introduce the role of the key members of the following thread stack frame in detail.

```c
typedef struct rt_hw_stack_frame
{
    rt_ubase_t epc;        /* epc - epc    - program counter                     */
    rt_ubase_t ra;         /* x1  - ra     - return address for jumps            */
    rt_ubase_t mstatus;    /*              - machine status register             */
    rt_ubase_t gp;         /* x3  - gp     - global pointer                      */
    rt_ubase_t tp;         /* x4  - tp     - thread pointer                      */
    rt_ubase_t t0;         /* x5  - t0     - temporary register 0                */
    rt_ubase_t t1;         /* x6  - t1     - temporary register 1                */
    rt_ubase_t t2;         /* x7  - t2     - temporary register 2                */
    rt_ubase_t s0_fp;      /* x8  - s0/fp  - saved register 0 or frame pointer   */
    rt_ubase_t s1;         /* x9  - s1     - saved register 1                    */
    rt_ubase_t a0;         /* x10 - a0     - return value or function argument 0 */
    rt_ubase_t a1;         /* x11 - a1     - return value or function argument 1 */
    rt_ubase_t a2;         /* x12 - a2     - function argument 2                 */
    rt_ubase_t a3;         /* x13 - a3     - function argument 3                 */
    rt_ubase_t a4;         /* x14 - a4     - function argument 4                 */
    rt_ubase_t a5;         /* x15 - a5     - function argument 5                 */
#ifndef __riscv_32e
    rt_ubase_t a6;         /* x16 - a6     - function argument 6                 */
    rt_ubase_t a7;         /* x17 - a7     - function argument 7                 */
    rt_ubase_t s2;         /* x18 - s2     - saved register 2                    */
    rt_ubase_t s3;         /* x19 - s3     - saved register 3                    */
    rt_ubase_t s4;         /* x20 - s4     - saved register 4                    */
    rt_ubase_t s5;         /* x21 - s5     - saved register 5                    */
    rt_ubase_t s6;         /* x22 - s6     - saved register 6                    */
    rt_ubase_t s7;         /* x23 - s7     - saved register 7                    */
    rt_ubase_t s8;         /* x24 - s8     - saved register 8                    */
    rt_ubase_t s9;         /* x25 - s9     - saved register 9                    */
    rt_ubase_t s10;        /* x26 - s10    - saved register 10                   */
    rt_ubase_t s11;        /* x27 - s11    - saved register 11                   */
    rt_ubase_t t3;         /* x28 - t3     - temporary register 3                */
    rt_ubase_t t4;         /* x29 - t4     - temporary register 4                */
    rt_ubase_t t5;         /* x30 - t5     - temporary register 5                */
    rt_ubase_t t6;         /* x31 - t6     - temporary register 6                */
#endif
#ifdef ARCH_RISCV_FPU
    rv_floatreg_t f0;      /* f0  */
    rv_floatreg_t f1;      /* f1  */
    rv_floatreg_t f2;      /* f2  */
    rv_floatreg_t f3;      /* f3  */
    rv_floatreg_t f4;      /* f4  */
    rv_floatreg_t f5;      /* f5  */
    rv_floatreg_t f6;      /* f6  */
    rv_floatreg_t f7;      /* f7  */
    rv_floatreg_t f8;      /* f8  */
    rv_floatreg_t f9;      /* f9  */
    rv_floatreg_t f10;     /* f10 */
    rv_floatreg_t f11;     /* f11 */
    rv_floatreg_t f12;     /* f12 */
    rv_floatreg_t f13;     /* f13 */
    rv_floatreg_t f14;     /* f14 */
    rv_floatreg_t f15;     /* f15 */
    rv_floatreg_t f16;     /* f16 */
    rv_floatreg_t f17;     /* f17 */
    rv_floatreg_t f18;     /* f18 */
    rv_floatreg_t f19;     /* f19 */
    rv_floatreg_t f20;     /* f20 */
    rv_floatreg_t f21;     /* f21 */
    rv_floatreg_t f22;     /* f22 */
    rv_floatreg_t f23;     /* f23 */
    rv_floatreg_t f24;     /* f24 */
    rv_floatreg_t f25;     /* f25 */
    rv_floatreg_t f26;     /* f26 */
    rv_floatreg_t f27;     /* f27 */
    rv_floatreg_t f28;     /* f28 */
    rv_floatreg_t f29;     /* f29 */
    rv_floatreg_t f30;     /* f30 */
    rv_floatreg_t f31;     /* f31 */
#endif
}rt_hw_stack_frame_t;
```

The construction of the thread stack frame is implemented according to the register structure defined by the RISC-V instruction architecture.

1. The role of the `epc` member of the thread stack: when the current thread is about to be switched, the value of the `ra` register is assigned to this member; when the thread is resumed, the value of this member is copied to the `mepc` register, and when the system executes the `mret` instruction, it assigns the value of the current register to the pc register, and the system will continue execution from the address pointed to by pc.

2. `ra` register is used to hold the pointer to the instruction that will continue to be executed after the function returns.

3. The `mstatus` register is used to set the mode in which the machine works, the floating point unit is turned on and off, global interrupts are turned on and off, and other functions.

4. The other integer registers are registers that may be used throughout the thread's operation, so these registers are saved in order.

5. With the `ARCH_RISCV_FPU` macro enabled, the thread stack also saves the floating point registers in order.

### Construct the thread stack

When the thread is not running, the initial thread site is empty, so you need to construct the thread's running site, the site refers to the current value of the registers handled by the thread during operation, when the thread is switched, you need to save the site of the currently running thread to the stack, and when the thread is running again, load the previously saved site, which is also the value of the processor's registers, from the stack into the system's registers. The effect of restoring the scene to continue execution is thus achieved.

Here we take a RISC-V 32-bit microcontroller as an example, the sample code is as follows:

```c
/**
 * This function will initialize thread stack
 *
 * @param tentry the entry of thread
 * @param parameter the parameter of entry
 * @param stack_addr the beginning stack address
 * @param texit the function will be called when thread exit
 *
 * @return stack address
 */
rt_uint8_t *rt_hw_stack_init(void       *tentry,
                             void       *parameter,
                             rt_uint8_t *stack_addr,
                             void       *texit)
{
    struct rt_hw_stack_frame *frame;
    rt_uint8_t         *stk;
    int                i;

    stk  = stack_addr + sizeof(rt_ubase_t);
    stk  = (rt_uint8_t *)RT_ALIGN_DOWN((rt_ubase_t)stk, REGBYTES);
    stk -= sizeof(struct rt_hw_stack_frame);

    frame = (struct rt_hw_stack_frame *)stk;

    for (i = 0; i < sizeof(struct rt_hw_stack_frame) / sizeof(rt_ubase_t); i++)
    {
        ((rt_ubase_t *)frame)[i] = 0xdeadbeef;
    }

    frame->ra      = (rt_ubase_t)texit;
    frame->a0      = (rt_ubase_t)parameter;
    frame->epc     = (rt_ubase_t)tentry;

    /* force to machine mode(MPP=11) and set MPIE to 1 */
#ifdef ARCH_RISCV_FPU
    frame->mstatus = 0x7880;
#else
    frame->mstatus = 0x1880;
#endif

    return stk;
}
```

Next we will break down the key implementation parts of the above code in detail:

```c
    stk  = stack_addr + sizeof(rt_ubase_t);
    stk  = (rt_uint8_t *)RT_ALIGN_DOWN((rt_ubase_t)stk, REGBYTES);
    stk -= sizeof(struct rt_hw_stack_frame);

    frame = (struct rt_hw_stack_frame *)stk;
```

The above code's implements the alignment of the thread stack space and also allocates space for the initial thread stack .

```c
    for (i = 0; i < sizeof(struct rt_hw_stack_frame) / sizeof(rt_ubase_t); i++)
    {
        ((rt_ubase_t *)frame)[i] = 0xdeadbeef;
    }
```

The above code initialises the initial stack space by initialising all stack space values to `0xdeadbeef`.

```c
    frame->ra      = (rt_ubase_t)texit;
    frame->a0      = (rt_ubase_t)parameter;
    frame->epc     = (rt_ubase_t)tentry;
```

The above code assigns values to key parameters in the stack space, `a0` refers to the parameters of the thread entry function, `epc` points to the address of the current thread entry function, and `ra` points to the function that is executed at the end of the function's lifecycle.

```c
#ifdef ARCH_RISCV_FPU
    frame->mstatus = 0x7880;
#else
    frame->mstatus = 0x1880;
#endif
```

The above code is used to set the running state of the system after startup, the above function will be called in the _thread_init function, the function to create threads and initialise threads will call the _thread_init function.