#### Lab7 report

## 列出你认为本实验中重要的知识点，以及与对应的OS原理中的知识点
> 熟悉ucore中的进程同步机制，了解操作系统为进程同步提供的底层支持；在ucore中理解信号量（semaphore）机制的具体实现；理解管程机制，在ucore内核中增加基于管程（monitor）的条件变量（condition variable）的支持；了解经典进程同步问题，并能使用同步机制解决进程同步问题

## 练习0：填写已有实验

> 使用了diff工具查找lab6与lab7间的区别，修改采用手动的方式，主要修改部分为lab6 /kern/trap/trap.c中调用的sched_class_proc_tick()改为调用run_timer_list()；另外在/kern/sync/monitor.h中make qemu会报错，查看代码发现错误部分是typedef重定义，将第二个
> 
    typedef struct monitor{
        ......
    } monitor_t;
> 修改为
> 
    struct monitor{
        ......
    };
> 即可。

## 练习1：理解内核级信号量的实现和基于内核级信号量的哲学家就餐问题（不需要编码）

完成练习0后，建议大家比较一下（可用kdiff3等文件比较软件）个人完成的lab6和练习0完成后的刚修改的lab7之间的区别，分析了解lab7采用信号量的执行过程。执行make grade，大部分测试用例应该通过。

## 请在实验报告中给出内核级信号量的设计描述，并说其大致执行流流程。

> 当多个（>1）进程可以进行互斥或同步合作时，一个进程会由于无法满足信号量设置的某条件而在某一位置停止，直到它接收到一个特定的信号（表明条件满足了）。为了发信号，需要使用一个称作信号量的特殊变量。为通过信号量s传送信号，信号量的V操作采用进程可执行原语semSignal(s)；为通过信号量s接收信号，信号量的P操作采用进程可执行原语semWait(s)；如果相应的信号仍然没有发送，则进程被阻塞或睡眠，直到发送完为止。
> 
> semaphore_t是最基本的记录型信号量（record semaphore)结构，包含了用于计数的整数值value，和一个进程等待队列wait_queue，一个等待的进程会挂在此等待队列上。
> 
    __down(semaphore_t *sem, uint32_t wait_state, timer_t *timer)
> 具体实现信号量的P操作，首先关掉中断，然后判断当前信号量的value是否大于0。如果是>0，则表明可以获得信号量，故让value减一，并打开中断返回即可；如果不是>0，则表明无法获得信号量，故需要将当前的进程加入到等待队列中，并打开中断，然后运行调度器选择另外一个进程执行。如果被V操作唤醒，则把自身关联的wait从等待队列中删除（此过程需要先关中断，完成后开中断）。
> 
    __up(semaphore_t *sem, uint32_t wait_state)
> 具体实现信号量的V操作，首先关中断，如果信号量对应的wait queue中没有进程在等待，直接把信号量的value加一，然后开中断返回；如果有进程在等待且进程等待的原因是semophore设置的，则调用wakeup_wait函数将waitqueue中等待的第一个wait删除，且把此wait关联的进程唤醒，最后开中断返回。

## 请在实验报告中给出给用户态进程/线程提供信号量机制的设计方案，并比较说明给内核级提供信号量机制的异同。

## 练习2: 完成内核级条件变量和基于内核级条件变量的哲学家就餐问题（需要编码）

## 请在实验报告中给出内核级条件变量的设计描述，并说其大致执行流流程。
> Hansan为管程所下的定义：“一个管程定义了一个数据结构和能为并发进程所执行（在该数据结构上）的一组操作，这组操作能同步进程和改变管程中的数据”。有上述定义可知，管程由四部分组成：
>> 
>> 管程内部的共享变量；
>> 
>> 管程内部的条件变量；
>> 
>> 管程内部并发执行的进程；
>> 
>> 对局部于管程内部的共享数据设置初始值的语句。
> 
> 局限在管程中的数据结构，只能被局限在管程的操作过程所访问，任何管程之外的操作过程都不能访问它；另一方面，局限在管程中的操作过程也主要访问管程内的数据结构。由此可见，管程相当于一个隔离区，它把共享变量和对它进行操作的若干个过程围了起来，所有进程要访问临界资源时，都必须经过管程才能进入，而管程每次只允许一个进程进入管程，从而需要确保进程之间互斥。
> 
> 一个条件变量CV可理解为一个进程的等待队列，队列中的进程正等待某个条件C变为真。每个条件变量关联着一个断言 "断言 (程序)")Pc。当一个进程等待一个条件变量，该进程不算作占用了该管程，因而其它进程可以进入该管程执行，改变管程的状态，通知条件变量CV其关联的断言Pc在当前状态下为真。因此对条件变量CV有两种主要操作：
>> 
>> wait_cv： 被一个进程调用，以等待断言Pc被满足后该进程可恢复执行. 进程挂在该条件变量上等待时，不被认为是占用了管程。
>> 
>> signal_cv：被一个进程调用，以指出断言Pc现在为真，从而可以唤醒等待断言Pc被满足的进程继续执行。
> 
> ucore中的管程机制是基于信号量和条件变量来实现的。ucore中的管程的数据结构monitor_t定义如下：
> 
    typedef struct monitor{
        semaphore_t mutex;      // the mutex lock for going into the routines in monitor, should be initialized to 1
        semaphore_t next;       // the next semaphore is used to down the signaling proc itself, and the other OR wakeuped
        //waiting proc should wake up the sleeped signaling proc.
        int next_count;         // the number of of sleeped signaling proc
        condvar_t *cv;          // the condvars in monitor
    } monitor_t;
> 管程中的成员变量mutex是一个二值信号量，是实现每次只允许一个进程进入管程的关键元素，确保了互斥访问性质。管程中的条件变量cv通过执行wait_cv，会使得等待某个条件C为真的进程能够离开管程并睡眠，且让其他进程进入管程继续执行；而进入管程的某进程设置条件C为真并执行signal_cv时，能够让等待某个条件C为真的睡眠进程被唤醒，从而继续进入管程中执行。管程中的成员变量信号量next和整形变量next_count是配合进程对条件变量cv的操作而设置的，这是由于发出signal_cv的进程A会唤醒睡眠进程B，进程B执行会导致进程A睡眠，直到进程B离开管程，进程A才能继续执行，这个同步过程是通过信号量next完成的；而next_count表示了由于发出singal_cv而睡眠的进程个数。
> 
> 管程中的条件变量的数据结构condvar_t定义如下：
> 
    typedef struct condvar{
        semaphore_t sem; // the sem semaphore is used to down the waiting proc, and the signaling proc should up the waiting proc
        int count;       // the number of waiters on condvar
        monitor_t * owner; // the owner(monitor) of this condvar
    } condvar_t;
> 
> 条件变量的定义中也包含了一系列的成员变量，信号量sem用于让发出wait_cv操作的等待某个条件C为真的进程睡眠，而让发出signal_cv操作的进程通过这个sem来唤醒睡眠的进程。count表示等在这个条件变量上的睡眠进程的个数。owner表示此条件变量的宿主是哪个管程。
> 
#### 请在实验报告中给出给用户态进程/线程提供条件变量机制的设计方案，并比较说明给内核级提供条件变量机制的异同。
