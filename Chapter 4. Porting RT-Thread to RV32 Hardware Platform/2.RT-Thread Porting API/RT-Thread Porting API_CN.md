# RT-Thread内核移植函数接口

RT-Thread支持各种架构的处理器平台，系统会开放出与移植相关的函数接口，这部分函数接口需要根据具体的处理器平台去实现。

在一个新的RISC-V架构的芯片上将RT-Thread运行起来就是指将RT-Thread内核在该芯片上可以具备线程管理和调度，内存管理，线程间同步与通信，定时器管理等功能。

# libcpu抽象层移植接口

RT-Thread 的 libcpu 抽象层向下提供了一套统一的 CPU 架构移植接口，这部分接口包含了全局中断开关函数、线程上下文切换函数、时钟节拍的配置和中断函数等等内容。下表是 CPU 架构移植需要实现的接口和变量。

|                          函数和变量                          |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|           rt_base_t rt_hw_interrupt_disable(void);           |                         关闭全局中断                         |
|        void rt_hw_interrupt_enable(rt_base_t level);         |                         打开全局中断                         |
| rt_uint8_t *rt_hw_stack_init(void *tentry, void *parameter, rt_uint8_t *stack_addr, void *texit); | 线程栈的初始化，内核在线程创建和线程初始化里面会调用这个函数 |
|        void rt_hw_context_switch_to(rt_uint32_t to);         | 没有来源线程的上下文切换，在调度器启动第一个线程的时候调用，以及在 signal 里面会调用 |
| void rt_hw_context_switch(rt_uint32_t from, rt_uint32_t to); |     从 from 线程切换到 to 线程，用于线程和线程之间的切换     |
| void rt_hw_context_switch_interrupt(rt_uint32_t from, rt_uint32_t to); |  从 from 线程切换到 to 线程，用于中断里面进行切换的时候使用  |
|         rt_uint32_t rt_thread_switch_interrupt_flag;         |                表示需要在中断里进行切换的标志                |
| rt_uint32_t rt_interrupt_from_thread, rt_interrupt_to_thread; |      在线程进行上下文切换时候，用来保存 from 和 to 线程      |

将上述的抽象层函数接口在当前的RISC-V平台下实现，便完成了RT-Thread在该RISC-V平台的移植。

