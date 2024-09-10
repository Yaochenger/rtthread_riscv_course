# RT-Thread多线程编程

本小节将介绍RT-Thread多线程编程，与裸机的前后台系统不同，操作系统的多线程可以将整个大的任务逻辑切换成若干个小的任务逻辑，同时极大的降低了各个线程逻辑的耦合，接下来将介绍RT-Thread创建线程的方法以及RT-Thread线程的介绍。

## 1.静态初始化线程

静态初始化线程，静态线程的栈空间是在系统初期就确定的，通常静态线程的栈空间位于BSS段，在系统启动后由于这部分内存空间不受内存管理，所以在使用后就不能被释放，即使这部分空间不再使用。静态初始化一个线程可以在程序编译阶段检查出是否有足够的空间去初始化该线程，不受内存管理也降低了其空间被写坏的风险。

RT-Thread静态初始化线程API如下：

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

这里我们详细介绍一下该API的入口参数：

- 第一个参数是线程的句柄，该句柄保存着线程的状态信息，我们可以通过查看该句柄来获取线程的详细信息。

- 第二个参数是线程的名称，RT-Thread采用面向对象的管理方式，线程是内核的一个对象，在初始化线程对象时会将该名称赋值给线程对象，可以通过名称找到该线程对象从而获取该线程对象的信息。

- 第三个参数是线程的入口函数，创建线程后，实际的用户逻辑在该函数中实现。

- 第四个参数是线程入口函数的参数，在线程函数中可以通过指针的方式去访问该参数。

- 第五个参数是线程栈的起始地址，RT-Thread每个线程都有其专属的线程栈用于保存局部变量与线程上下文切换时得上下文保存。

- 第六个参数是线程栈得大小，这个参数需要根据具体得线程得局部变量得占用大小来设置，RT-Thread的PS命令可以查看当前线程栈的是使用百分比，用户可通过该命令查看当前的线程栈使用百分比后重新调整线程栈的大小。

- 第七个参数是线程的优先级，RT-Thread支持线程的抢占，线程优先级设置的数字越小代表该线程的优先级越高，线程的优先级需要根据线程的重要程度自行分配。

- 第八个参数是线程的时间片，若系统当前运行的最高优先级的线程存在多个同等优先级的线程，系统将采用时间片轮转的方式执行，多个线程轮流运行设定时间片的时间，当前线程时间片结束后将系统的控制权交给下一个同等优先级的线程，时间片的单位是系统时钟tick。

具体的初始化一个线程的方法如下：

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

在初始化线程结束后使用`rt_thread_startup`将当前线程加入到系统的就绪链表进行调度，如果当前线程的优先级最高，将即刻切换到当前线程运行。

当我们不需要当前线程时可以使用`rt_thread_detach`函数将该线程从线程就绪链表上分离，这样该线程将不参与线程的调度，由于使用静态初始化的线程，所以只是当前线程不参与线程调度，但线程使用的内存不会被释放。

## 2.动态创建线程

动态创建线程与静态初始化线程都会创建一个线程，不同之处是动态创建线程的资源是动态分配的，在不需要使用时可以将其删除从而释放其占用的资源。

RT-Thread动态创建线程API如下：

```c
rt_thread_t rt_thread_create(const char *name,
                             void (*entry)(void *parameter),
                             void       *parameter,
                             rt_uint32_t stack_size,
                             rt_uint8_t  priority,
                             rt_uint32_t tick);
```

动态创建线程的API与静态初始化的不同之处在于，不需要手动传递线程栈的起始地址，同时线程的句柄成为了函数的返回值。

具体的创建一个线程的方法如下：

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

使用动态创建线程的方式时，将从RT-Thread管理的堆内存空间中分配创建线程需要的空间 ，创建成功后会返回线程的句柄，通过判断句柄是否为空来选择是否要启动该线程。

当不需要该线程时，用户可以使用`rt_thread_delete`删除当前线程，当前线程的空间被释放，可供其它动态申请内存的行为使用。

## 3.多线程编程示例

通过上述的学习，我们已经留了解了RT-Thread创建线程与初始化线程的方式。这里我们将给出一个示例用于学习RT-Thread多线程编程。

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

上述代码中，线程1将周期运行，线程2在运行10次以后自动退出并释放资源。需要注意的时在线程入口函数中需要添加`rt_thread_delay`,`rt_thread_yield`等让出线程的系统API，线程若无法主动让出CPU控制权将影响系统的实时性。



