# RT-Thread线程间通讯

在裸机编程时我们通常使用共享内存的方式去实现线程间的通讯，简单共享内存的方式会存在由于业务裸机出现漏洞的原因导致临界资源被意外篡改，从而与预期的业务逻辑不符，使用共享内存的方式也会存在由于长时间等不到标志位置位从而使单个业务逻辑长时间占用CPU控制权，降低系统的实时性。使用 系统提供的线程间同步或通信的方式将会解决上述的问题。

## 1.线程间同步

RT-Thread通常可以使用信号量，互斥锁与事件集来是实现线程间的同步。信号量通常用协调收发处理速度不一致的情况，互斥锁可以用来对临界资源的保护，事件集用于多个事件之间的同步。当使用上述几种方式作为线程与线程之间的同步时，当获取不到指定对象时可以将当前使用该对象的线程挂起让出CPU的控制权，当等待的对象就绪时将因等待该对象而挂起的线程依次唤醒，从而使CPU可以高效的处理任务，CPU处于空闲的时间。

这里以常用的信号量举例：

信号量对象与线程对象一致我，同样分为静态初始化信号量与动态创建信号量。下文是静态创建信号量的函数：

```c
rt_err_t rt_sem_init(rt_sem_t    sem,
                     const char *name,
                     rt_uint32_t value,
                     rt_uint8_t  flag);
```

这里我们详细介绍一下该API的入口参数：

- 第一个参数是信号量的句柄，信号量的句柄保存着该信号量的详细的信息，包括可获取的信号量的数量，挂起在当前信号量的挂起链表的线程等，在静态初始化信号量的API下需要手动定义该句柄
- 第二个参数是信号量的名称，RT-Thread采用面向对象的管理方式，信号量是内核的一个对象，在初始化信号量对象时会将该名称赋值给信号量对象，可以通过名称找到该信号量对象从而获取该信号量对象的信息。
- 第三个参数是信号量的初始值，每获取一次信号量就会在初值的基础上减去1，当信号量得到值减到0时将获取不到信号量将当前的线程挂起。
- 第四个参数是信号量的调度方式，标志参数决定了当信号量不可用时，多个线程等待的排队方式。当选择 RT_IPC_FLAG_FIFO（先进先出）方式时，那么等待线程队列将按照先进先出的方式排队，先进入的线程将先获得等待的信号量；当选择 RT_IPC_FLAG_PRIO（优先级等待）方式时，等待线程队列将按照优先级进行排队，优先级高的等待线程将先获得等待的信号量。RT_IPC_FLAG_FIFO 属于非实时调度方式，除非应用程序非常在意先来后到，并且你清楚地明白所有涉及到该信号量的线程都将会变为非实时线程，方可使用 RT_IPC_FLAG_FIFO，否则建议采用 RT_IPC_FLAG_PRIO，即确保线程的实时性。

 具体的初始化一个信号量的方法如下：

```C
struct rt_semaphore sem_lock;

rt_sem_init(&sem_lock, "lock", 1, RT_IPC_FLAG_PRIO);
```

当我们不需要该信号量时，可以使用`rt_sem_detach`将信号量从对象容器中分离，由于使用静态的方式初始化一个信号量，所以使用上述API后，信号量占据的内存并不能被释放供其他对象使用。

动态创建信号量的方式如下：

```c
 static rt_sem_t dynamic_sem = RT_NULL;
 
 dynamic_sem = rt_sem_create("dsem", 0, RT_IPC_FLAG_PRIO);
```

与静态初始化信号量不同的是，动态创建信号量从系统管理的内存堆上分配内存，所以用户不需要手动定义信号量句柄。

同样当我们不在需要动态创建的定时器时可以调用`rt_sem_delete`将创建的信号量删除，动态创建的信号量的内存来自系统管理的堆内存，所以使用上述API删除后使用的内存空间会被释放供其他对象使用。

与线程对象不同的是，IPC对象会多一些读写API，当创建信号量后，我们需要对信号量进行操作，关于信号量涉及到信号量的获取，释放，与控制，下文我们详细的介绍这些API的使用方式。

首先介绍信号量的获取API，这里有两个相关的API`rt_sem_take`与`rt_sem_trytake`,这俩个API的区别在于前者可以设置等待时间，当获取不到信号时可以将当前线程挂起，等待设定时间或者一直等待，后者在于如果获取不到信号量就返回超时错误码，继续执行后续任务。

下述是可设置等待时间的获取信号量API

```c
rt_err_t rt_sem_take(rt_sem_t sem, rt_int32_t timeout);
```

- 第一个参数是信号量的句柄，这里传递我们要操作的信号量的句柄。
- 第二个参数用于设置超时时间，这里可以设置具体的时间，时间的单位是OS的时钟，系统将等待一段时间，如果超出等待时间还未被唤醒，将返回超时的错误码。也可以设置为RT-Thread预先定义的宏，`RT_WAITING_FOREVER` 与`RT_WAITING_NO`，若设置为前者，在当前线程获取不到信号量时，当前线程将被挂起直到有信号量被释放，当前线程将会按照设定的唤醒顺序被唤醒（按被挂起的顺序唤醒或按线程的优先级高低顺序唤醒），当设置为后者时，该API的行为与`rt_sem_trytake`一致，获取不到信号量时便挂起当前线程。

下述是尝试获取信号量的API，该API不会导致线程的挂起。

```c
rt_err_t rt_sem_trytake(rt_sem_t sem);
```

下述是释放信号量的API:

```c
rt_err_t rt_sem_release(rt_sem_t sem);
```

该API用于释放信号量，若当信号量的值等于零时，并且有线程等待这个信号量时，释放信号量将唤醒等待在该信号量线程队列中的第一个线程，由它获取信号量；否则将把信号量的值加 1。

`rt_sem_control`是信号量相关的最后的一个API，该API可用与复位信号量，设置信号量允许释放的最大值，该API使用率较低。

上述便是信号量的API的介绍，信号量的典型的使用场景是同步，所以这里信号量的同步举例介绍信号量的使用。

同步示例：

```c
#include <rtthread.h>

#define THREAD_PRIORITY         25
#define THREAD_TIMESLICE        5

static rt_sem_t dynamic_sem = RT_NULL;

#ifdef rt_align
rt_align(RT_ALIGN_SIZE)
#else
ALIGN(RT_ALIGN_SIZE)
#endif
static char thread1_stack[1024];
static struct rt_thread thread1;
static void rt_thread1_entry(void *parameter)
{
    static rt_uint8_t count = 0;

    while (1)
    {
        if (count <= 100)
        {
            count++;
        }
        else
            return;

        if (0 == (count % 10))
        {
            rt_kprintf("thread1 release a dynamic semaphore.\n");
            rt_sem_release(dynamic_sem);
        }
        if (count == 40)
        {
            rt_sem_delete(dynamic_sem);
        }
    }
}

#ifdef rt_align
rt_align(RT_ALIGN_SIZE)
#else
ALIGN(RT_ALIGN_SIZE)
#endif
static char thread2_stack[1024];
static struct rt_thread thread2;
static void rt_thread2_entry(void *parameter)
{
    static rt_err_t result;
    static rt_uint8_t number = 0;
    while (1)
    {
        result = rt_sem_take(dynamic_sem, RT_WAITING_FOREVER);
        if (result != RT_EOK)
        {
            rt_kprintf("thread2 take a dynamic semaphore, failed.\n");
            rt_sem_delete(dynamic_sem);
            return;
        }
        else
        {
            number++;
            rt_kprintf("thread2 take a dynamic semaphore. number = %d\n", number);
        }
    }
}

int semaphore_sample()
{
    dynamic_sem = rt_sem_create("dsem", 0, RT_IPC_FLAG_PRIO);
    if (dynamic_sem == RT_NULL)
    {
        rt_kprintf("create dynamic semaphore failed.\n");
        return -1;
    }
    else
    {
        rt_kprintf("create done. dynamic semaphore value = 0.\n");
    }

    rt_thread_init(&thread1,
                   "thread1",
                   rt_thread1_entry,
                   RT_NULL,
                   &thread1_stack[0],
                   sizeof(thread1_stack),
                   THREAD_PRIORITY, THREAD_TIMESLICE);

    rt_thread_startup(&thread1);

    rt_thread_init(&thread2,
                   "thread2",
                   rt_thread2_entry,
                   RT_NULL,
                   &thread2_stack[0],
                   sizeof(thread2_stack),
                   THREAD_PRIORITY - 1, THREAD_TIMESLICE);

    rt_thread_startup(&thread2);

    return 0;
}

MSH_CMD_EXPORT(semaphore_sample, semaphore sample);
```

示例详解：

这里我们使用静态初始化的方式初始化了两个线程并动态创建了一个信号量。在线程2中接收信号量，在线程1中释放信号量，当线程2获取不到信号量时，线程2将被挂起，线程1释放信号量后将线程2唤醒，这样线程1的释放与线程2的接收形成了同步，同时线程2也不需要一直查询是否有信号量，可以让出CPU的控制权交给其他任务执行。

## 2.线程间通讯

RT-Thread通常可以使用消息邮箱与消息队列的的方式进行线程与线程之间的通讯，与裸机下的共享内存不同的是使用OS下的线程间通讯的方式在接收不到消息时将线程将被挂起，从而让出CPU的控制权，当有消息时再将挂起的线程恢复，从而使CPU可以高效的处理任务，CPU处于空闲的时间。

RT-Thread的常使用的线程间通讯的方式有消息邮箱与消息队列。这里我们以消息邮箱举例：消息邮箱的特点是占用资源小，传递的数据大小固定为4字节。在32位平台下，指针的长度为32位，所以我们可以通过传递指针的方式传递大于4字节的数据，在接收过程中以同样的数据类型间隙即可。

下文是静态初始化一个消息邮箱对象的API：

```c
rt_err_t rt_mb_init(rt_mailbox_t mb,
                    const char  *name,
                    void        *msgpool,
                    rt_size_t    size,
                    rt_uint8_t   flag);
```

- 第一个参数为消息邮箱的句柄指针，这里使用的是静态初始化的方式，需要手动定义消息邮箱的句柄
- 第二个参数是消息邮箱对象的名称，可以通过名称找到消息邮箱的句柄，从而查看消息邮箱的详细信息。
- 第三个参数是存放消息的起始地址，接收到的消息将储存到该地址指向的内存空间。
- 第四个参数是允许存放消息的个数的最大值。
- 第五个参数是当有线程等待邮件时的等待方式，它可以取如下数值： RT_IPC_FLAG_FIFO 或 RT_IPC_FLAG_PRIO，RT_IPC_FLAG_FIFO 属于非实时调度方式，除非应用程序非常在意先来后到，并且你清楚地明白所有涉及到该邮箱的线程都将会变为非实时线程，方可使用 RT_IPC_FLAG_FIFO，否则建议采用 RT_IPC_FLAG_PRIO，即确保线程的实时性。

当不想使用消息邮箱时，我们可以使用下述函数分离消息邮箱：

```c
rt_err_t rt_mb_detach(rt_mailbox_t mb);
```

使用上述函数将消息邮箱从对象管理容器中分离，因为是静态创建的消息邮箱对象，所以并不会释放消息邮箱占用的空间。

同样的可以使用动态的方式创建消息邮箱，动态创建的API如下：

```c
rt_mailbox_t rt_mb_create(const char *name, rt_size_t size, rt_uint8_t flag);
```

- 第一个参数是消息邮箱的名字，在创建消息邮箱对象时将该名字赋值给消息邮箱对象，可以通过该名字找到该消息邮箱对象。
- 第二个参数是消息邮箱的大小，用于指定最大可以接收的邮件的数量，每个邮件占据4字节。
- 第三个参数与静态初始化消息邮箱中的作用一样。

同样在不再使用该邮箱的地方可以删除该邮箱对象，使用`rt_mb_delete`删除，此时可以将创建邮箱动态申请的内存进行释放，供其他对象使用。

与信号量的行为一致，消息邮箱同样有对对象进行读写操作的API，例如`rt_mb_send`,`rt_mb_send_wait` 与`rt_mb_urgent`用于发送邮件，`rt_mb_recv`用于接收邮件，接下来详细介绍这些API以及API的使用方式。

下述是发送邮件的API：

```c
rt_err_t rt_mb_send(rt_mailbox_t mb, rt_ubase_t value);
```

- 第一个参数是消息邮箱的句柄
- 第二个参数是邮件的内容，大小为4字节

与该API功能类似的API为下述的API：

```c
rt_err_t rt_mb_send_wait(rt_mailbox_t mb,
                         rt_ubase_t  value,
                         rt_int32_t   timeout);
```

与上述的发送API不同的是，该发送API增加了超时参数，如果邮箱满了可以设置为等待一段时间或者一直等，等待期间会将当前的线程挂起，让出CPU的控制权，当邮箱有空间时再将线程唤醒，从新获得使用CPU控制权的机会。

最后一个与发送邮件相关的API是`rt_mb_urgent`,这个API与开始的发送API的区别是，该API会将发送的消息茶道消息链表的最前面，接收消息时可以率先接收到该消息。

上文介绍了如何发送邮件，接下来介绍如何接收邮件，接收邮件的API如下：

```c
rt_err_t rt_mb_recv(rt_mailbox_t mb, rt_ubase_t *value, rt_int32_t timeout);
```

- 第一个参数是消息邮箱的句柄
- 第二个参数是用于接收邮件的变量，这里传的是接收邮件的数据的地址
- 第三个参数是用于设置超时时间，当接收不到消息时会将当前的线程挂起设定时间，时间的单位时系统时钟。

上述便是消息邮箱的的API的介绍，消息邮箱的典型的使用场景是线程间的信息交互，所以这里以使用消息邮箱进行线程间的消息传递为例。

```c
#include <rtthread.h>

#define THREAD_PRIORITY      10
#define THREAD_TIMESLICE     5

static struct rt_mailbox mb;
static char mb_pool[128];

static char mb_str1[] = "I'm a mail!";
static char mb_str2[] = "this is another mail!";
static char mb_str3[] = "over";

#ifdef rt_align
rt_align(RT_ALIGN_SIZE)
#else
ALIGN(RT_ALIGN_SIZE)
#endif
static char thread1_stack[1024];
static struct rt_thread thread1;

static void thread1_entry(void *parameter)
{
    char *str;

    while (1)
    {
        rt_kprintf("thread1: try to recv a mail\n");

        if (rt_mb_recv(&mb, (rt_ubase_t *)&str, RT_WAITING_FOREVER) == RT_EOK)
        {
            rt_kprintf("thread1: get a mail from mailbox, the content:%s\n", str);
            if (str == mb_str3)
                break;

            rt_thread_mdelay(100);
        }
    }
    rt_mb_detach(&mb);
}

#ifdef rt_align
rt_align(RT_ALIGN_SIZE)
#else
ALIGN(RT_ALIGN_SIZE)
#endif
static char thread2_stack[1024];
static struct rt_thread thread2;

static void thread2_entry(void *parameter)
{
    rt_uint8_t count;

    count = 0;
    while (count < 10)
    {
        count ++;
        if (count & 0x1)
        {
            rt_mb_send(&mb, (rt_uint32_t)&mb_str1);
        }
        else
        {
            rt_mb_send(&mb, (rt_uint32_t)&mb_str2);
        }

        rt_thread_mdelay(200);
    }
    rt_mb_send(&mb, (rt_uint32_t)&mb_str3);
}

int mailbox_sample(void)
{
    rt_err_t result;

    result = rt_mb_init(&mb,
                        "mbt",                      
                        &mb_pool[0],                
                        sizeof(mb_pool) / sizeof(rt_ubase_t),
                        RT_IPC_FLAG_PRIO);         
    if (result != RT_EOK)
    {
        rt_kprintf("init mailbox failed.\n");
        return -1;
    }

    rt_thread_init(&thread1,
                   "thread1",
                   thread1_entry,
                   RT_NULL,
                   &thread1_stack[0],
                   sizeof(thread1_stack),
                   THREAD_PRIORITY, THREAD_TIMESLICE);

    rt_thread_startup(&thread1);

    rt_thread_init(&thread2,
                   "thread2",
                   thread2_entry,
                   RT_NULL,
                   &thread2_stack[0],
                   sizeof(thread2_stack),
                   THREAD_PRIORITY, THREAD_TIMESLICE);

    rt_thread_startup(&thread2);
    return 0;
}

MSH_CMD_EXPORT(mailbox_sample, mailbox sample);
```

再这个例子中我们创建了两个线程与一个邮箱，线程2负责发送邮箱， 线程1接收到邮件后，将邮件地址指向的数据打印出来，实现了线程与线程直线的消息传递，下述是上述代码运行的结果：

```shell
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:I'm a mail!
msh >thread1: try to recv a mail
thread1: get a mail from mailbox, the content:this is another mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:I'm a mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:this is another mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:I'm a mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:this is another mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:I'm a mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:this is another mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:I'm a mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:this is another mail!
thread1: try to recv a mail
thread1: get a mail from mailbox, the content:over
```



