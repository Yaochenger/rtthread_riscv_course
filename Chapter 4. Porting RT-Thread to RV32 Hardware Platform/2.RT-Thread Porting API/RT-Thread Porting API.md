### RT-Thread Kernel Porting Function Interfaces

RT-Thread supports a variety of processor platforms, and the system will open up a porting-related function interface, which needs to be implemented according to the specific processor platform.

Running RT-Thread on a new RISC-V architecture chip means that the RT-Thread kernel can be equipped with thread management and scheduling, memory management, thread synchronisation and communication, timer management and other functions on the chip.

### libcpu abstraction layer porting interfaces

RT-Thread's libcpu abstraction layer provides a unified set of CPU architecture porting interfaces down the line, which includes global interrupt switching functions, thread context switching functions, clock beat configuration and interrupt functions, etc. The following table shows the interfaces and variables that need to be implemented for CPU architecture porting. The following table shows the interfaces and variables that need to be implemented for CPU architecture migration.

|                   Functions and variables                    |                         Descriptions                         |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|           rt_base_t rt_hw_interrupt_disable(void)            |                  Turn off global interrupts                  |
|         void rt_hw_interrupt_enable(rt_base_t level)         |                  Turn on global interrupts                   |
| rt_uint8_t *rt_hw_stack_init(void *tentry, void *parameter, rt_uint8_t *stack_addr, void *texit) | Thread stack initialisation, which is called by the kernel inside thread creation and thread initialisation. |
|         void rt_hw_context_switch_to(rt_uint32_t to)         | Context switching for unsourced threads is called when the scheduler starts the first thread, and inside signal |
| void rt_hw_context_switch(rt_uint32_t from, rt_uint32_t to)  | Switching from from thread to to thread for thread to thread switching |
| void rt_hw_context_switch_interrupt (rt_uint32_t from, rt_uint32_t to) | Switching from from thread to to thread, used when switching inside an interrupt. |
|         rt_uint32_t rt_thread_switch_interrupt_flag          |    Flag indicating the need to switch between interrupts     |
| rt_uint32_t rt_interrupt_from_thread, rt_interrupt_to_thread | Used to store the from and to threads when the threads switch contexts. |

By implementing the above abstraction layer function interfaces under the current RISC-V platform, the porting of RT-Thread to this RISC-V platform is completed.

