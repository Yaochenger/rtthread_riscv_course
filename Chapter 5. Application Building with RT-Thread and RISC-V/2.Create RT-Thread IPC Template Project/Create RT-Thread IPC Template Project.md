### RT-Thread inter-thread communication

In bare-metal programming, we usually use shared memory to achieve inter-thread communication. Simple shared memory can cause critical resources to be tampered with due to vulnerabilities in the bare-metal of the business, which may be inconsistent with the expected business logic, and shared memory can also cause the CPU to be occupied for a long time by individual business logic due to long waits for a flag location, which may reduce the real-time performance of the system. The use of shared memory can also reduce the real-time performance of the system by causing individual business logic to occupy CPU control for long periods of time due to long waits for flag locations. Using the inter-thread synchronisation or communication provided by the system will solve these problems.

#### 1. Inter-Thread Synchronisation

RT-Thread can usually use signals, mutual exclusion locks and event sets to achieve inter-thread synchronisation. Signals are often used to co-ordinate the processing speed between threads, mutex locks can be used to protect critical resources, and event sets can be used to synchronise between multiple events. When using the above ways as a synchronisation between threads, when the specified object can not be obtained when the current use of the object can be hung up the thread to give control of the CPU, when waiting for the object is ready to wait for the object and hung up threads in turn to wake up, so that the CPU can efficiently handle the task, the CPU is in the idle time.

Here is an example of a commonly used semaphore:

Signal objects are the same as thread objects. They are also divided into static initialised signal quantities and dynamically created signal quantities. The following is the function to create a signal statically:

```c
rt_err_t rt_sem_init(rt_sem_t    sem,
                     const char *name,
                     rt_uint32_t value,
                     rt_uint8_t  flag);
```

Here we introduce the entry parameters of the API in detail:

- The first parameter is the handle of the semaphore, the handle of the semaphore holds the detailed information of the semaphore, including the number of available semaphores, the threads that are pending in the current semaphore's pending list and so on, which needs to be defined manually under the API of static initialisation of semaphores.
- The second parameter is the name of the semaphore, RT-Thread adopts object oriented management, the semaphore is an object of the kernel, when initialising the semaphore object, the name will be assigned to the semaphore object, the semaphore object can be found by the name so as to get the information of the semaphore object.
- The third parameter is the initial value of the semaphore, every time you get a semaphore, it will subtract 1 from the initial value, when the semaphore gets to 0, it will not be able to get the semaphore and the current thread will hang up.
- The fourth parameter is the scheduling method of the semaphore, the flag parameter determines the queuing method of multiple threads waiting when the semaphore is not available. When RT_IPC_FLAG_FIFO is selected, then the waiting thread queue will be queued according to FIFO, and the thread that enters first will get the waiting signal first; when RT_IPC_FLAG_PRIO is selected, the waiting thread queue will be queued according to priority, and the waiting thread with high priority will get the waiting signal first. RT_IPC_FLAG_FIFO is a non-real-time scheduling method, unless the application cares a lot about first-come-first-served and you clearly understand that all threads involving the semaphore will become non-real-time threads, you can use RT_IPC_FLAG_FIFO, otherwise it is recommended that you use RT_IPC_FLAG_PRIO, which is to ensure the real-time nature of the threads.

 The specific method to initialise a semaphore is as follows:

```C
struct rt_semaphore sem_lock;

rt_sem_init(&sem_lock, "lock", 1, RT_IPC_FLAG_PRIO);
```

When we don't need the semaphore, we can use `rt_sem_detach` to detach the semaphore from the object container. Since a semaphore is initialised statically, the memory occupied by the semaphore is not freed for use by other objects after using the above API.

Dynamically creating a semaphore is done as follows:

```c
 static rt_sem_t dynamic_sem = RT_NULL;
 
 dynamic_sem = rt_sem_create("dsem", 0, RT_IPC_FLAG_PRIO);
```

Unlike static initialised signals, dynamically created signals allocate memory from the system managed memory heap, so the user doesn't need to manually define signal handles.

Similarly when we don't need the dynamically created timer anymore we can call `rt_sem_delete` to delete the created semaphore, dynamically created semaphores get their memory from the system managed heap memory, so the memory space used after deletion using the above mentioned APIs will be freed for other objects to use.

Unlike thread objects, IPC objects have more read/write APIs. When we create a semaphore, we need to operate the semaphore, which involves acquiring, releasing, and controlling the semaphore, and we will introduce the use of these APIs in detail below.

First of all, we introduce the signal acquisition API, there are two related API `rt_sem_take` and `rt_sem_trytake`, the difference between these two APIs is that the former can be set to wait for a certain period of time, so that when the signal is not acquired, you can hang the current thread, wait for a set period of time, or wait for a certain period of time, and the latter is that if you cannot acquire the signal, you will be able to return to the timeout error code. The latter returns a timeout error code if the signal is not obtained, and continues to execute the subsequent tasks.

The following are the APIs for fetching signals that can set the wait time.

```c
rt_err_t rt_sem_take(rt_sem_t sem, rt_int32_t timeout);
```

- The first parameter is the handle of the semaphore, here pass the handle of the semaphore we want to operate.
- The second parameter is used to set the timeout time, here you can set a specific time, the unit of time is the OS clock, the system will wait for a period of time, if the waiting time is exceeded and has not been woken up, it will return the timeout error code. You can also set RT-Thread predefined macros, `RT_WAITING_FOREVER` and `RT_WAITING_NO`, if you set the former, if the current thread can not get the signal, the current thread will be hung until the signal is released, and the current thread will be woken up according to the set wake up order (according to the order of the hung or according to the priority order of the thread). When the latter is set, the API behaves like `rt_sem_trytake` and hangs the current thread if the semaphore is not fetched.

The following is the API for trying to get a semaphore, which does not cause the thread to hang.

```c
rt_err_t rt_sem_trytake(rt_sem_t sem);
```

The following is the API for releasing semaphores.

```c
rt_err_t rt_sem_release(rt_sem_t sem);
```

This API is used to release a semaphore, if the value of the semaphore is equal to zero and there is a thread waiting for the semaphore, releasing the semaphore will wake up the first thread waiting in the semaphore thread queue, and it will get the semaphore; otherwise, it will add 1 to the semaphore's value.

`rt_sem_control` is the last API related to semaphores, it can be used to reset the semaphore and set the maximum value of the semaphore that can be released, but this API is less used.

The above is the introduction of the API of semaphores, the typical use of semaphores is synchronisation, so here is an example of the use of semaphores for synchronisation.

Synchronisation example:

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

Example Details:

Here we use static initialisation to initialise two threads and dynamically create a semaphore. Thread 2 receives the semaphore and thread 1 releases the semaphore. When thread 2 fails to get the semaphore, thread 2 will hang, and thread 1 releases the semaphore and wakes up thread 2. This way, thread 1 releases the semaphore and thread 2 receives the semaphore synchronously, and thread 2 does not need to query the semaphore all the time to give the control of the CPU to other tasks.

#### 2. Inter-thread communication

RT-Thread can usually use message mailboxes and message queues to communicate between threads. Unlike shared memory under bare metal, the way of inter-thread communication under OS will hang the threads when no message is received, thus giving up the control of the CPU, and then the hung threads will be restored when there is a message, so that the CPU can efficiently process the tasks and the CPU is idle. idle time.

RT-Thread's commonly used methods of inter-thread communication are message mailboxes and message queues. Here we take the message mailbox as an example: the message mailbox is characterised by its small resource consumption and the size of the data passed is fixed at 4 bytes. In 32-bit platform, the length of the pointer is 32 bits, so we can pass pointers to pass data larger than 4 bytes, in the receiving process with the same data type gap can be.

The following is the API for statically initialising a message mailbox object:

```c
rt_err_t rt_mb_init(rt_mailbox_t mb,
                    const char  *name,
                    void        *msgpool,
                    rt_size_t    size,
                    rt_uint8_t   flag);
```

- The first parameter is a pointer to the handle of the message mailbox, here we use static initialisation, you need to define the handle of the message mailbox manually.
- The second parameter is the name of the message mailbox object, you can find the handle of the message mailbox through the name, so as to view the details of the message mailbox.
- The third parameter is the starting address to store the message, the received message will be stored to the memory space pointed to by this address.
- The fourth parameter is the maximum number of messages allowed to be stored.
- The fifth parameter is the waiting mode when there is a thread waiting for the message, which can take the following values: RT_IPC_FLAG_FIFO or RT_IPC_FLAG_PRIO, RT_IPC_FLAG_FIFO is a non-real-time scheduling mode unless the application is very concerned about the first-come-first-served, and you clearly understand that all the threads that are involved in the mailbox will become RT_IPC_FLAG_FIFO for non-real-time threads, otherwise RT_IPC_FLAG_PRIO is recommended, i.e. to ensure that threads are real-time.

When we don't want to use the message mailbox, we can use the following function to detach the message mailbox:

```c
rt_err_t rt_mb_detach(rt_mailbox_t mb);
```

Using the above function to detach the message mailbox from the object management container does not free up the space occupied by the message mailbox as it is created statically.

Similarly the message mailbox can be created dynamically, the API for dynamic creation is as follows:

```c
rt_mailbox_t rt_mb_create(const char *name, rt_size_t size, rt_uint8_t flag);
```

- The first parameter is the name of the message mailbox, which will be assigned to the message mailbox object when it is created, and the message mailbox object can be found by this name.
- The second parameter is the size of the message mailbox, used to specify the maximum number of messages that can be received, each message occupies 4 bytes.
- The third parameter is the same as in the static initialisation of the message mailbox.

Again the mailbox object can be deleted where it is no longer used, using `rt_mb_delete`, which frees the memory dynamically claimed by the creation of the mailbox for use by other objects.

In line with the behaviour of semaphores, message mailboxes also have APIs for reading and writing objects, such as `rt_mb_send`, `rt_mb_send_wait` and `rt_mb_urgent` for sending messages, `rt_mb_recv` for receiving messages, the next section describes these APIs in detail as well as how they are used.

The following is the API for sending emails:

```c
rt_err_t rt_mb_send(rt_mailbox_t mb, rt_ubase_t value);
```

- The first parameter is the handle of the message mailbox
- The second parameter is the content of the message, which is 4 bytes in size.

The APIs that are similar to this API are the following APIs:

```c
rt_err_t rt_mb_send_wait(rt_mailbox_t mb,
                         rt_ubase_t  value,
                         rt_int32_t   timeout);
```

The difference with the above sending API is that this sending API adds a timeout parameter, if the mailbox is full you can set it to wait for a period of time or keep waiting, during the waiting period it will hang the current thread, giving up control of the CPU, and then wake the thread when there is space in the mailbox to get a new chance to use the CPU control.

The last API related to sending mail is `rt_mb_urgent`, which differs from the start of the sending API in that it televises the message being sent to the top of the message chain, so that incoming messages are the first to be received.

The previous section describes how to send a message, and the next section describes how to receive a message, the API for receiving a message is as follows:

```c
rt_err_t rt_mb_recv(rt_mailbox_t mb, rt_ubase_t *value, rt_int32_t timeout);
```

- The first parameter is the handle of the message mailbox
- The second parameter is the variable used to receive the message, here is the address of the data to receive the message.
- The third parameter is used to set the timeout time, when the message is not received, the current thread will be hanged for a set time, the unit of time is the system clock.

The above is the introduction of message mailbox API, the typical use of message mailbox is the information interaction between threads, so here to use message mailbox for inter-thread messaging as an example.

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

In this example, we create two threads and a mailbox, thread 2 is responsible for sending the mailbox, thread 1 receives the mail and prints out the data pointed to by the mail address, thus realising the message passing between threads in a straight line, the following is the result of the above code:

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



