### RT-Thread Thread and Interrupt Switching Ported Interface Adaptation (RISC-V)

In addition to context switching between threads, switching between threads and interrupts also occurs. When the system is executing a thread, and an interrupt is triggered at this time, the system jumps from the current thread's execution environment to the interrupt execution environment.

Execution in the interrupt environment, if this time to release a signal quantity, the system exists, such as the signal quantity of the thread, at this time, these threads will be woken up and then execute the system scheduling function, but at this time will not really switch, but to set a flag when the completion of the interrupt and then executed.

For RISC-V architecture chips generally support vector interrupt and non-vector interrupt management mode, in order to better understand the context switching process between threads and interrupts, here we have non-vector interrupt management mode as an example to start the introduction.

The code for switching between threads and interrupts is also implemented in assembly, and we will introduce this part of the code in detail.

### Thread and interrupt context switching code

In RT-Thread 5.1.0, the interrupt-to-thread switching code is named `SW_handler`, so you need to assign the address of this function to the `mtvec` register during the system startup phase.

Next we step by step through RT-Thread's implementation of switching code between RISC-V threads and interrupt contexts.

The first step is to save the site of the thread before jumping to the interrupt:

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

It is worth noting that, based on the characteristics of the open source development of the RISC-V instruction set, manufacturers in the design of their own RISC-V chip will be done to customise the design, so some of the RISC-V architecture of the chip in the interrupt will not close the interrupt, so in order to the framework of the universality of the interrupt, the first to enter the interrupt to close the global interrupt operation.

After the thread site is saved in the thread stack, it is switched to the interrupt stack space for execution, and after the interrupt processing is completed, the interrupt and thread stack are exchanged again. The implementation code is as follows:

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

As mentioned above, when executing in an interrupt environment, if a semaphore is released at this time, there are threads in the system waiting for this semaphore, and these threads will be woken up at this time and then execute the system scheduling function, but at this time there will not be a real switch, but rather, a flag is set, and the interrupt is completed and then executed.

This flag is checked after the interrupt has finished executing. If the flag is not set, the thread is resumed directly, and the resumed thread is the one that was interrupted by the interrupt. If the flag is set, thread switching will be performed, and the thread that will be resumed after the interrupt is finished will be the thread that was ready during the interrupt processing. The specific code is as follows:

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

After the completion of the above code for thread recovery, thread recovery code is shown below:

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





