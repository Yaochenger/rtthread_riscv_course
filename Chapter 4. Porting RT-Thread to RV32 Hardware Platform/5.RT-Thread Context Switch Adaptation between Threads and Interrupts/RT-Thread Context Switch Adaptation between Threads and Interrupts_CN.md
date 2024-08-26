# RT-Thread线程与中断切换移植接口适配(RISC-V)

除了线程与线程之间的上下文切换，在线程与中断之间的也会发生切换，当系统正在执行线程时，此时触发了一个中断，系统便会从当前线程的执行环境跳转至中断执行环境执行。

在中断环境中执行时，若此时释放一个信号量，系统存在等该该信号量的线程，此时这些线程会被唤醒然后执行系统调度函数，但此时并不会真正的切换，而是置一个标志，当完成中断后再执行。

对于RISC-V架构的芯片普遍支持向量中断与非向量中断的管理模式，为了更好的理解线程与中断之间的上下文切换过程，这里我们已非向量中断管理模式为例展开介绍。

线程与中断之间的切换代码同样采用汇编实现，接下来我们详细介绍这部分代码。

# 线程与中断上下文切换代码

在RT-Thread5.1.0版本中将中断与线程之间的切换代码的实现统一命名为`SW_handler`，所以在系统启动阶段需要将该函数的地址赋值给`mtvec`寄存器。

接下来我们逐步介绍RT-Thread对RISC-V线程与中断上下文代码的切换的实现。

首先是保存跳转至中断之前的线程的现场：

```assembly
    .global SW_handler

SW_handler:
    csrci mstatus, 0x8
#ifdef ARCH_RISCV_FPU
    addi    sp, sp, -32 * FREGBYTES
    FSTORE  f0, 0 * FREGBYTES(sp)
    FSTORE  f1, 1 * FREGBYTES(sp)
    FSTORE  f2, 2 * FREGBYTES(sp)
    FSTORE  f3, 3 * FREGBYTES(sp)
    FSTORE  f4, 4 * FREGBYTES(sp)
    FSTORE  f5, 5 * FREGBYTES(sp)
    FSTORE  f6, 6 * FREGBYTES(sp)
    FSTORE  f7, 7 * FREGBYTES(sp)
    FSTORE  f8, 8 * FREGBYTES(sp)
    FSTORE  f9, 9 * FREGBYTES(sp)
    FSTORE  f10, 10 * FREGBYTES(sp)
    FSTORE  f11, 11 * FREGBYTES(sp)
    FSTORE  f12, 12 * FREGBYTES(sp)
    FSTORE  f13, 13 * FREGBYTES(sp)
    FSTORE  f14, 14 * FREGBYTES(sp)
    FSTORE  f15, 15 * FREGBYTES(sp)
    FSTORE  f16, 16 * FREGBYTES(sp)
    FSTORE  f17, 17 * FREGBYTES(sp)
    FSTORE  f18, 18 * FREGBYTES(sp)
    FSTORE  f19, 19 * FREGBYTES(sp)
    FSTORE  f20, 20 * FREGBYTES(sp)
    FSTORE  f21, 21 * FREGBYTES(sp)
    FSTORE  f22, 22 * FREGBYTES(sp)
    FSTORE  f23, 23 * FREGBYTES(sp)
    FSTORE  f24, 24 * FREGBYTES(sp)
    FSTORE  f25, 25 * FREGBYTES(sp)
    FSTORE  f26, 26 * FREGBYTES(sp)
    FSTORE  f27, 27 * FREGBYTES(sp)
    FSTORE  f28, 28 * FREGBYTES(sp)
    FSTORE  f29, 29 * FREGBYTES(sp)
    FSTORE  f30, 30 * FREGBYTES(sp)
    FSTORE  f31, 31 * FREGBYTES(sp)
#endif
    /* save all from thread context */
#ifndef __riscv_32e
    addi sp, sp, -32 * REGBYTES
#else
    addi sp, sp, -16 * REGBYTES
#endif
    STORE x5,   5 * REGBYTES(sp)
    STORE x1,   1 * REGBYTES(sp)
    /* Mandatory set the MPIE of mstatus */
    li    t0,   0x80
    STORE t0,   2 * REGBYTES(sp)
    STORE x4,   4 * REGBYTES(sp)
    STORE x6,   6 * REGBYTES(sp)
    STORE x7,   7 * REGBYTES(sp)
    STORE x8,   8 * REGBYTES(sp)
    STORE x9,   9 * REGBYTES(sp)
    STORE x10, 10 * REGBYTES(sp)
    STORE x11, 11 * REGBYTES(sp)
    STORE x12, 12 * REGBYTES(sp)
    STORE x13, 13 * REGBYTES(sp)
    STORE x14, 14 * REGBYTES(sp)
    STORE x15, 15 * REGBYTES(sp)
#ifndef __riscv_32e
    STORE x16, 16 * REGBYTES(sp)
    STORE x17, 17 * REGBYTES(sp)
    STORE x18, 18 * REGBYTES(sp)
    STORE x19, 19 * REGBYTES(sp)
    STORE x20, 20 * REGBYTES(sp)
    STORE x21, 21 * REGBYTES(sp)
    STORE x22, 22 * REGBYTES(sp)
    STORE x23, 23 * REGBYTES(sp)
    STORE x24, 24 * REGBYTES(sp)
    STORE x25, 25 * REGBYTES(sp)
    STORE x26, 26 * REGBYTES(sp)
    STORE x27, 27 * REGBYTES(sp)
    STORE x28, 28 * REGBYTES(sp)
    STORE x29, 29 * REGBYTES(sp)
    STORE x30, 30 * REGBYTES(sp)
    STORE x31, 31 * REGBYTES(sp)
#endif
```

值得注意的是，基于RISC-V指令集的开源开发的特点，厂家在设计自家的RISC-V芯片时会做进行自定义设计，所以有些RISC-V架构的芯片在进入中断时并不会关闭中断，所以为了框架的普适性，在进入中断时首先进行关闭全局中断的操作。

将线程现场保存线程栈中后，则切换切换至中断栈空间执行，完成中断处理后则再次进行中断与线程栈的交换。实现代码如下：

```assembly
    /* switch to interrupt stack */
    csrrw sp,mscratch,sp
    /* interrupt handle */
    call  rt_interrupt_enter
    /* Do the work after saving the above */
    jal   rt_hw_do_after_save_above

    call  rt_interrupt_leave
    /* switch to from thread stack */
    csrrw sp,mscratch,sp
```

在上文提到，在中断环境中执行时，若此时释放一个信号量，系统存在等该该信号量的线程，此时这些线程会被唤醒然后执行系统调度函数，但此时并不会真正的切换，而是置一个标志，当完成中断后再执行。

在中断执行完毕后将会检查该标志，如果该标志未置位，则直接进行线程的恢复，被恢复的线程是被中断打断的线程。如果该标志置位，则进行线程切换，此时中断结束后被恢复的线程将是在中断处理期间就绪的线程。具体代码如下：

```assembly
 /* Determine whether to trigger scheduling at the interrupt function */
    la    t0, rt_thread_switch_interrupt_flag
    lw    t2, 0(t0)
    beqz  t2, 1f
    /* clear the flag of rt_thread_switch_interrupt_flag */
    sw    zero, 0(t0)

    csrr  a0, mepc
    STORE a0, 0 * REGBYTES(sp)

    la    t0, rt_interrupt_from_thread
    LOAD  t1, 0(t0)
    STORE sp, 0(t1)

    la    t0, rt_interrupt_to_thread
    LOAD  t1, 0(t0)
    LOAD  sp, 0(t1)

    LOAD  a0,  0 * REGBYTES(sp)
    csrw  mepc, a0
```

完成上述代码后进行线程的恢复,线程恢复的代码如下所示：

```assembly
1:
    LOAD  x1,   1 * REGBYTES(sp)

    /* Set the mode after MRET */
    li    t0, 0x1800
    csrs  mstatus, t0
    LOAD  t0,   2 * REGBYTES(sp)
    csrs  mstatus, t0

    LOAD  x4,   4 * REGBYTES(sp)
    LOAD  x5,   5 * REGBYTES(sp)
    LOAD  x6,   6 * REGBYTES(sp)
    LOAD  x7,   7 * REGBYTES(sp)
    LOAD  x8,   8 * REGBYTES(sp)
    LOAD  x9,   9 * REGBYTES(sp)
    LOAD  x10, 10 * REGBYTES(sp)
    LOAD  x11, 11 * REGBYTES(sp)
    LOAD  x12, 12 * REGBYTES(sp)
    LOAD  x13, 13 * REGBYTES(sp)
    LOAD  x14, 14 * REGBYTES(sp)
    LOAD  x15, 15 * REGBYTES(sp)
#ifndef __riscv_32e
    LOAD  x16, 16 * REGBYTES(sp)
    LOAD  x17, 17 * REGBYTES(sp)
    LOAD  x18, 18 * REGBYTES(sp)
    LOAD  x19, 19 * REGBYTES(sp)
    LOAD  x20, 20 * REGBYTES(sp)
    LOAD  x21, 21 * REGBYTES(sp)
    LOAD  x22, 22 * REGBYTES(sp)
    LOAD  x23, 23 * REGBYTES(sp)
    LOAD  x24, 24 * REGBYTES(sp)
    LOAD  x25, 25 * REGBYTES(sp)
    LOAD  x26, 26 * REGBYTES(sp)
    LOAD  x27, 27 * REGBYTES(sp)
    LOAD  x28, 28 * REGBYTES(sp)
    LOAD  x29, 29 * REGBYTES(sp)
    LOAD  x30, 30 * REGBYTES(sp)
    LOAD  x31, 31 * REGBYTES(sp)

    addi  sp, sp, 32 * REGBYTES
#else
    addi  sp, sp, 16 * REGBYTES
#endif

#ifdef ARCH_RISCV_FPU
    FLOAD   f0, 0 * FREGBYTES(sp)
    FLOAD   f1, 1 * FREGBYTES(sp)
    FLOAD   f2, 2 * FREGBYTES(sp)
    FLOAD   f3, 3 * FREGBYTES(sp)
    FLOAD   f4, 4 * FREGBYTES(sp)
    FLOAD   f5, 5 * FREGBYTES(sp)
    FLOAD   f6, 6 * FREGBYTES(sp)
    FLOAD   f7, 7 * FREGBYTES(sp)
    FLOAD   f8, 8 * FREGBYTES(sp)
    FLOAD   f9, 9 * FREGBYTES(sp)
    FLOAD   f10, 10 * FREGBYTES(sp)
    FLOAD   f11, 11 * FREGBYTES(sp)
    FLOAD   f12, 12 * FREGBYTES(sp)
    FLOAD   f13, 13 * FREGBYTES(sp)
    FLOAD   f14, 14 * FREGBYTES(sp)
    FLOAD   f15, 15 * FREGBYTES(sp)
    FLOAD   f16, 16 * FREGBYTES(sp)
    FLOAD   f17, 17 * FREGBYTES(sp)
    FLOAD   f18, 18 * FREGBYTES(sp)
    FLOAD   f19, 19 * FREGBYTES(sp)
    FLOAD   f20, 20 * FREGBYTES(sp)
    FLOAD   f21, 21 * FREGBYTES(sp)
    FLOAD   f22, 22 * FREGBYTES(sp)
    FLOAD   f23, 23 * FREGBYTES(sp)
    FLOAD   f24, 24 * FREGBYTES(sp)
    FLOAD   f25, 25 * FREGBYTES(sp)
    FLOAD   f26, 26 * FREGBYTES(sp)
    FLOAD   f27, 27 * FREGBYTES(sp)
    FLOAD   f28, 28 * FREGBYTES(sp)
    FLOAD   f29, 29 * FREGBYTES(sp)
    FLOAD   f30, 30 * FREGBYTES(sp)
    FLOAD   f31, 31 * FREGBYTES(sp)

    addi    sp, sp, 32 * FREGBYTES
#endif
    mret
```





