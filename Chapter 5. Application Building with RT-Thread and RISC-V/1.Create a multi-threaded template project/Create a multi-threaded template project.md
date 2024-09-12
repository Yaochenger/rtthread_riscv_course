### RT-Thread Multi-Threaded Programming

This subsection will introduce RT-Thread multi-thread programming. Unlike bare-metal front- and back-end systems, multi-threading of the operating system can switch the entire large task logic into several small task logics, while greatly reducing the coupling of individual thread logics. The next section describes the method of creating threads in RT-Thread as well as an introduction to RT-Thread threads.

#### 1. Static initialisation of threads

Static initialisation of threads, the stack space for static threads is determined at the beginning of the system, usually the stack space for static threads is located in the BSS segment, after the system starts up, since this part of the memory space is not subject to memory management, it can not be released after use even if this part of the space is no longer in use. Static initialisation of a thread can be checked during the compilation phase of the program to see if there is enough space to initialise the thread, and not being memory managed also reduces the risk of its space being written over.

The RT-Thread static initialisation thread API is as follows:

```c
rt_err_t rt_thread_init(struct rt_thread *thread,
                        const char       *name,
                        void (*entry)(void *parameter),
                        void             *parameter,
                        void             *stack_start,
                        rt_uint32_t       stack_size,
                        rt_uint8_t        priority,
                        rt_uint32_t       tick);
```

Here we describe the entry parameters of the API in detail:

- The first parameter is the thread's handle, which holds the thread's status information, and we can get the thread's detailed information by checking the handle.

- The second parameter is the name of the thread, RT-Thread adopts object-oriented management, the thread is an object of the kernel, the name will be assigned to the thread object when it is initialised, and the thread object can be found by the name to get the information of the thread object.

- The third parameter is the entry function of the thread. After the thread is created, the actual user logic is implemented in this function.

- The fourth parameter is the parameter of the thread entry function, which can be accessed by pointer in the thread function.

- The fifth parameter is the start address of the thread stack, RT-Thread has its own thread stack for saving local variables and context saving when switching thread contexts.

- The sixth parameter is the size of the thread stack, this parameter needs to be set according to the size of the local variables occupied by the specific thread, RT-Thread's PS command can check the current percentage of the thread stack, users can use this command to check the current percentage of the thread stack and then re-adjust the size of the thread stack.

- The seventh parameter is the thread priority, RT-Thread supports thread preemption, the smaller the number set for thread priority means the higher the priority of the thread, the priority of the thread needs to be assigned according to the importance of the thread.

- The eighth parameter is the thread's time slice, if the system is currently running the highest priority thread there are multiple threads of equal priority, the system will use the time slice rotation method of execution, multiple threads take turns to run a set time slice of time, the current thread time slice is over will be the control of the system to the next thread of equal priority, the unit of time slice is the system clock tick.

The specific method of initialising a thread is as follows:

```c
#define THREAD_PRIORITY         25
#define THREAD_STACK_SIZE       512
#define THREAD_TIMESLICE        5

#ifdef rt_align
rt_align(RT_ALIGN_SIZE)
#else
ALIGN(RT_ALIGN_SIZE)
#endif
static char thread2_stack[1024];
static struct rt_thread thread2;

rt_thread_init(&thread2,
                   "thread2",
                   thread2_entry,
                   RT_NULL,
                   &thread2_stack[0],
                   sizeof(thread2_stack),
                   THREAD_PRIORITY - 1, THREAD_TIMESLICE);

rt_thread_startup(&thread2); /* start thread #2 */
```

At the end of the initialisation thread use `rt_thread_startup` to add the current thread to the system's thread-ready chain for scheduling, and if the current thread has the highest priority, it will be switched over to the current thread instantly.

When we don't need the current thread we can use the `rt_thread_detach` function to detach the thread from the thread-ready chain table so that the thread will not participate in the thread scheduling, due to the use of static initialisation of the thread, so just the current thread does not participate in the thread scheduling, but the memory used by the thread is not freed.

#### 2. Dynamically Created Threads

Dynamically created threads and statically initialised threads both create a thread, the difference is that dynamically created threads are dynamically allocated resources and can be deleted when they are not needed to free up the resources they occupy.

The RT-Thread dynamic thread creation API is as follows:

```c
rt_thread_t rt_thread_create(const char *name,
                             void (*entry)(void *parameter),
                             void       *parameter,
                             rt_uint32_t stack_size,
                             rt_uint8_t  priority,
                             rt_uint32_t tick);
```

The API for dynamically creating threads differs from static initialisation in that there is no need to manually pass in the starting address of the thread stack, while the handle of the thread becomes the return value of the function.

The exact way to create a thread is as follows:

```c
#define THREAD_PRIORITY         25
#define THREAD_STACK_SIZE       512
#define THREAD_TIMESLICE        5

static rt_thread_t tid1 = RT_NULL;

tid1 = rt_thread_create("thread1",
                            thread1_entry, RT_NULL,
                            THREAD_STACK_SIZE,
                            THREAD_PRIORITY, THREAD_TIMESLICE);
    if (tid1 != RT_NULL)
        rt_thread_startup(tid1);
```

When using the dynamic thread creation method, the space needed to create the thread will be allocated from the heap memory space managed by RT-Thread, and the thread's handle will be returned after successful creation, and the user can choose whether or not to start the thread by determining whether or not the handle is empty.

When the thread is not needed, the user can use `rt_thread_delete` to delete the current thread, and the space of the current thread is released for other dynamic memory requests.

#### 3. Multi-threaded programming example

Through the above, we have stayed aware of the way RT-Thread creates and initialises threads. Here we will give an example for learning RT-Thread multithreading.

```c
#include <rtthread.h>

#define THREAD_PRIORITY         25
#define THREAD_STACK_SIZE       512
#define THREAD_TIMESLICE        5

/* thread handler */
static rt_thread_t tid1 = RT_NULL;

/* thread #1 entry function */
static void thread1_entry(void *parameter)
{
    rt_uint32_t count = 0;

    while (1)
    {
        rt_kprintf("thread1 count: %d\n", count ++);
        rt_thread_mdelay(500);
    }
}

#ifdef rt_align
rt_align(RT_ALIGN_SIZE)
#else
ALIGN(RT_ALIGN_SIZE)
#endif
static char thread2_stack[1024];
static struct rt_thread thread2;

static void thread2_entry(void *param)
{
    rt_uint32_t count = 0;

    for (count = 0; count < 10 ; count++)
    {
        /* thread #2 prints counting numbers */
        rt_kprintf("thread2 count: %d\n", count);
    }
    rt_kprintf("thread2 exit\n");
}

int thread_sample(void)
{

    tid1 = rt_thread_create("thread1",
                            thread1_entry, RT_NULL,
                            THREAD_STACK_SIZE,
                            THREAD_PRIORITY, THREAD_TIMESLICE);

    if (tid1 != RT_NULL)
        rt_thread_startup(tid1);

    rt_thread_init(&thread2,
                   "thread2",
                   thread2_entry,
                   RT_NULL,
                   &thread2_stack[0],
                   sizeof(thread2_stack),
                   THREAD_PRIORITY - 1, THREAD_TIMESLICE);

    rt_thread_startup(&thread2); /* start thread #2 */

    return 0;
}
```

In the above code, thread 1 will run periodically and thread 2 will automatically exit and release resources after 10 times. It should be noted that the thread entry function needs to add `rt_thread_delay`, `rt_thread_yield` and other system APIs to let the thread out of the thread, if the thread can not take the initiative to give up the right to control the CPU will affect the real-time system.

