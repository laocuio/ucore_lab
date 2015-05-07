##### Lab6 report

###列出你认为本实验中重要的知识点，以及与对应的OS原理中的知识点
> 操作系统调度管理机制
> ucore系统调度器框架
> Round-Robin调度算法


### 练习0：填写已有实验

> 使用meld工具手动修改lab6，修改了部分代码。其中，在proc.c的初始化中，由于RR算法的要求，要将proc->run_link初始化为一个空链表，而不是NULL。

### 练习1: 使用 Round Robin 调度算法（不需要编码）

## 请理解并分析sched_calss中各个函数指针的用法，并接合Round Robin 调度算法描ucore的调度执行过程
    struct sched_class {
       const char *name;       // 初始化
       void (*init) (struct run_queue *rq);       // 将进程 p 插入队列 rq
       void (*enqueue) (struct run_queue *rq, struct proc_struct *p); // 将进程 p 从队列 rq 中删除
       void (*dequeue) (struct run_queue *rq, struct proc_struct *p); // 返回 运行队列 中下一个可执行的进程
       struct proc_struct* (*pick_next) (struct run_queue *rq);        // timetick 处理函数
       void (*proc_tick)(struct  run_queue* rq, struct proc_struct* p);
    };
> 1. kern_init对默认的RR算法进行初始化
> 2. trap.c调用sched_class_proc_tick()。
> 3. wakeup_proc函数把一个就绪进程放入到就绪进程队列中的工作。在RR调度算法中，RR_enqueue把某进程的进程控制块指针放入到rq队列末尾，且如果进程控制块的时间片为0，则需要把它重置为rq成员变量max_time_slice。
> 4. schedule函数把当前继续占用CPU执行的运行进程放放入到就绪进程队列中，从就绪进程队列中选择一个“合适”就绪进程，把这个“合适”的就绪进程从就绪进程队列中摘除。
> 5. RR_pick_next函数选取就绪进程队列rq中的队头队列元素，并把队列元素转换成进程控制块指针。
> 6. RR_dequeue函数把就绪进程队列rq的进程控制块指针的队列元素删除，并把表示就绪进程个数的proc_num减一。
> 7. RR_proc_tick函数在每次timer到时后，trap函数将会间接调用此函数来把当前执行进程的时间片time_slice减一。如果time_slice降到零，则设置此进程成员变量need_resched标识为1，这样在下一次中断来后执行trap函数时，会由于当前进程程成员变量need_resched标识为1而执行schedule函数，从而把当前执行进程放回就绪队列末尾，而从就绪队列头取出在就绪队列上等待时间最久的那个就绪进程执行。

### 练习2: 实现 Stride Scheduling 调度算法（需要编码）

> 按照注释一步步进行即可，主要是一些细节问题需要注意，piazza提供了很大的帮助

### 备注
> lab2 lab4 lab5 提交时没有交report，在此一并补上，麻烦助教批改。
