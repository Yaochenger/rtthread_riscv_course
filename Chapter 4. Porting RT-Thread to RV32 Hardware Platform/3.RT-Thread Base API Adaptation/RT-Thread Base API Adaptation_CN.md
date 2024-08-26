# RT-Thread基础函数移植接口适配(RISC-V)

本小节我们将介绍除上下文切换的函数接口之外的函数接口，这些函数接口的实现相对简单但在系统中起着很重要的作用。

# 开关全局中断

在系统运行过程中，许多场景会使用到全局中断的开关，例如临界资源的保护在一些场景下可以通过开关中断进行保护。在简单的移植场景下通常是不支持中断嵌套的，所以在进入中断后需要将全局中断关闭，完成中断处理后再将全局中断打开。

rt-thread中的开关全局中断的函数接口如下：

```c
rt_base_t rt_hw_interrupt_disable(void);
void rt_hw_interrupt_enable(rt_base_t level);
```

使用关闭全局中断的函数接口后，会返回关中断之前的函数；使用使能全局中断的函数接口时，会将关中断之前的状态进行复原。

这部分函数接口的实现通常使用汇编实现以保证开关全局中断代码的高效简洁。

在RISC-V架构下通常可以使用下述的实现方式：

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

# 线程栈帧

当线程进行上下文切换时便会根据构造的线程栈帧去保存，只有线程栈帧的顺序去保存才能实现线程线程的正确恢复，下述代码是RT-Thread针对RISC-V设置的线程栈帧，接下来我们详细介绍下述线程栈帧中关键成员的作用。

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

线程栈帧的构造是根据RISC-V 指令架构定义的寄存器结构实现的。

1.线程栈中的`epc`成员的作用：当前线程即将被切走时，将`ra`寄存器的值赋予该成员， 当该线程被恢复时，将该成员的值拷贝至`mepc`寄存器，当系统执行`mret`指令后会将当前寄存器的值赋值给pc寄存器，系统将从pc指向的地址处继续执行。

2.`ra`寄存器用于保存函数返回后继续执行的指令的指针。

3.`mstatus`寄存器用于设置机器工作的模式，浮点单元的开启于关闭，全局中断的开启与关闭等功能。

4.其他整数寄存器都是在整个线程运行过程中可能会使用到的寄存器，所以这部分寄存器均按顺序保存即可。

5.使能了`ARCH_RISCV_FPU`宏后，线程栈同样会将浮点寄存器按顺序进行保存。

# 构造线程栈

当线程未运行时，线程初始的现场是空的，所以需要人为构造线程的运行现场，现场指的是线程在运行过程中处理的寄存器的当下的值，在线程进行切换时，就需要将当前运行的线程的现场保存到栈中，在该线程再次运行时从栈中将之前保存的现场也就是处理器的寄存器值加载至系统的寄存器中，从而达到恢复现场继续执行的效果。

这里以RISC-V 32位的微控制器举例，示例代码如下：

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

接下来我们将详细分下上述代码中的关键实现部分：

```c
    stk  = stack_addr + sizeof(rt_ubase_t);
    stk  = (rt_uint8_t *)RT_ALIGN_DOWN((rt_ubase_t)stk, REGBYTES);
    stk -= sizeof(struct rt_hw_stack_frame);

    frame = (struct rt_hw_stack_frame *)stk;
```

上述代码的实现了线程栈空间的对齐，同时为初始的线程栈(现场)分配了空间。

```c
    for (i = 0; i < sizeof(struct rt_hw_stack_frame) / sizeof(rt_ubase_t); i++)
    {
        ((rt_ubase_t *)frame)[i] = 0xdeadbeef;
    }
```

上述代码对初始的栈空间进行了初始化，将栈空间的值均初始化为`0xdeadbeef`。

```c
    frame->ra      = (rt_ubase_t)texit;
    frame->a0      = (rt_ubase_t)parameter;
    frame->epc     = (rt_ubase_t)tentry;
```

上述代码对栈空间中的关键参数进行赋值，`a0`指的是线程入口函数的参数，`epc`指向当前线程入口函数的地址，`ra`指向函数结束生命周期时执行的函数。

```c
#ifdef ARCH_RISCV_FPU
    frame->mstatus = 0x7880;
#else
    frame->mstatus = 0x1880;
#endif
```

上述代码通过对线程栈中`mstatus`寄存器的现场赋值，达到设置线程启动时运行的状态与模式。

上述函数会在_thread_init函数调用，创建线程和初始化线程的函数均会调用 _thread_init这个函数。

