进程与调度
## Intro and Misc
### Misc
OS管理下层CPU Mem I/O Files硬件资源，所以分为多个子系统来管理
**CPU管理子系统** -> 进程与调度
内存管理子系统
IO管理子系统
文件管理子系统

- 基础：进程描述与控制
- 实现：互斥与同步
- 避免：死锁与饥饿
- 解决：几个经典问题
	- 进程的生产者、消费者（生产者消费者问题）：多进程中间存在AB之间信息传递的生产消费关系
	- 读者写者问题
	- 哲学家进餐问题：死锁
	- > 将计算机中的资源管理会遇到的问题映射为生活中的问题方便理解
- 关于：进程通信
- 策略：进程调度

**关于进程数量**
> 32位 Linux 系统最多可以同时拥有 32768 个进程，其中包括用户进程和系统进程
> 这个限制是由内核中定义的**进程 ID (PID)** 数值的范围所决定的。在 32 位系统中，PID 的范围是 0 至 2^15-1（即 0 到 32767）。其中，PID 0 保留为内核进程（也称为 swapper 进程），PID 1 保留为 init 进程。
> 由于在 32 位系统中，每个进程都需要使用一个 PID，因此最多可以有 32768 个进程同时存在。但是，实际上系统可以运行的进程数会受到其他因素的限制，如系统资源、物理内存等。
> 值得注意的是，64位系统的 PID 范围更广，为 0 至 2^22-1，也就是说，64位系统可以支持更多的进程。

进程少了资源没有利用完，进程太多调度不过来，所以进程数量与机器性能的关系大概是正态分布。
### 程序执行顺序
- 程序顺序执行：顺序性 封闭性 可再现性
- 程序并发执行：间断性 非封闭性 不可再现性
- 程序并发执行Bernstein条件： $$R(P1)\cap W(P2) \cup  W(P1)\cap R(P2)   \cup W(P1)\cap W(P2) =\{\} $$ 三个并运算为空，A写B不能读，反之，AB不同时写。可以同时读。

### 原子程序
> 原语是在操作系统中调用核心层子程序的指令。与一般广义指令的区别在于它是不可中断的，而且总是作为一个基本单位出现。它与一般过程的区别在于：它们是“[原子操作](https://baike.baidu.com/item/%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C/1880992?fromModule=lemma_inlink)（primitive or atomic action）”。所谓原子操作，是指一个操作中的所有动作要么全做，要么全不做。换言之，它是一个不可分割的基本单位，因此，在执行过程中不允许被中断。原子操作在管态下执行，常驻内存。原语的作用是为了实现进程的通信和控制，系统对进程的控制如不使用原语，就会造成其状态的不确定性，从而达不到[进程](https://baike.baidu.com/item/%E8%BF%9B%E7%A8%8B/382503?fromModule=lemma_inlink)控制的目的。
## Process
### Concept
- also called task
- can be traced
### Characteristics
- Dynamic 动态性
- Concurrency 并发性
- Independent 独立性
### Structure
- Programs
- Data
- PCB *Process Control Block* 一种描述进程的数据结构，描述进程的所有信息
	- Process Identification
		- PID
		- PID of its parents
		- User ID (who create)
	- Process State Information
		- User-Visible Registers
		- Control and Status Registers
			- Program Counter
			- Condition codes
			- Status Information
		- Scheduling-related information
	- Data Structuring
	- Interprocess Communication
	- Process Privileges
	- Memory Management
	- Resource Ownership and Utilization
	- .......
进程=**程序+数据+PCB**（数据结构）
### States
进程有多种状态
#### two-state process model
- Running 执行
- Not-running 非执行
- ![[Screenshot 2023-09-05 at 11.02.07.png]]
#### five-state process model
问题：一些not running的程序，有的ready而有的是在等I/O
在等的就让他等
用一个队列来看，dispatcher不能高效分配
> Thus, using a single queue, the dispatcher could not just select the process at the oldest end of the queue. Rather, the dispatcher would have to scan the list looking for the process that is **not blocked** and that has **been in the queue the longest.**

解决方法：split the Not Running state into two states: Ready and Blocked.
- Running
	- 占用处理机
	- 一个入口 Dispatch
	- 三个出口 Timeout EventWait Release
- Ready 就绪
	- 准备执行 A process that is prepared to execute when given the opportunity.
- Blocked
	- 等待某件事后才准备（如等IO)
	- 阻塞状态也可能排队
- New
	- just been created but has not yet been admitted to the pool of executable processes by the OS. Typically, a new process has not yet been loaded into main memory, although its process control block has been created.
- Exit
	- A process that has been released from the pool of executable processes by the OS, either because it halted or because it aborted for some reason.
	- exit 没有完全消失 PCB还在内存里 但是不能调度了
- ![[Screenshot 2023-09-05 at 11.14.12.png]]
	- 还有的没有画出来
	- 例如Ready->exit blocked->exit 因为父进程终止，子进程无论如何也要终止
	- 用队列来表示![[Screenshot 2023-09-05 at 11.32.57.png]]
	- Ready队列也可以按照优先级分为多个队列，从技术上讲可行

## Swapping
### Concepts
对换技术、交换技术 swapping-out swapping-in
内存空间有限，不一定能把整个进程都装入内存
于是：swapping involves moving part or all of a process from main memory to disk. When none of the processes in main memory is in the Ready state, the OS swaps one of the blocked processes out on to disk into a **suspend挂起** queue. This is a queue of existing processes that have been temporarily kicked out of main memory, or suspended. The OS then brings in another process from the suspend queue, or it honors a new-process request. Execution then continues with the newly arrived process.
进程PCB *Process Control Block*不能拿到外面，必须在内存，PCB会记录这个进程在哪
### Suspend vs. Blocked
**阻塞：在内存等待
挂起：在外存等待**
> 进程从磁盘读数据到内存，是存到内存的缓冲区 Disk Cache I/O Buffer
> 操作系统在管理File 内存调度等
> 如果进程被Suspended，那原有Block需要等待的I/O并不会影响
> 因为虽然进程进了外存，但阻塞条件独立于挂起条件，读的数据读到了
> BlockedSuspend -> ReadySuspend
> 等进程Activate后Ready

### seven-state process model

![[Screenshot 2023-09-05 at 11.44.57.png]]

图a有一定问题：因为阻塞、挂起之间独立，所以suspend不一定能直接进ready，所以要改进为图b，2x2排列组合四种状态

> 一般来讲，为了减少系统负载，操作系统会优先dispatch已有的进程

## Process Control
![[Screenshot 2023-09-07 at 10.42.53.png]]
每个进程都要调用各种资源，那操作系统需要很多表来记录各个资源占用情况、进程状态情况
tables ![[Screenshot 2023-09-07 at 10.47.49.png]]
- Memory Tables
	- Allocation of main memory to processes 分配情况
	- Allocation of second memory to processes
	- Protection attributes for access to shared memory regions 保护
- I/O Tables
	- I/O device is available or assigned 设备的使用情况
	- I/O 操作状态
	- I/O 传输的东西在内存中的起止地址
- File Tables (File System)
	- 已有的所有文件
	- location in secondary memory
	- current states
	- attributes, privilege
- Process Tables
	- Primary Process Table 建立索引，指向Process Image
	- Process Image 虚拟概念，包括program, data, etc.
	- PCB address
	- Process ID
	- Process state
	- Process location
		- program
		- data location
		- constants
		- stack

### Function of OS Kernel
- manage
	- process
	- memory
	- I/O
- support
	- interrupt handling
	- timing
	- primitive : atomic operation
	- accounting
	- monitoring

### Process Control Primitives 原语
最小运行单位，不能再切分的
- Process Switch
	- Clock Interrupt
	- I/O Interrupt
	- Memory fault 访问的数据在虚存中没在内存 产生I/O中断装进来
	- Trap 陷阱 error occurred
	- Supervisor call 管理程序调入
- Create and Terminate
	- Create: PID, allocate space, init PCB, update Queue
	- Termination: Many reasons
- Block and Wakeup
- Suspend and Activate

对比：Change Process State 状态更改
- Save registers, update PCB, move PCB, Select another process
- Update selected process, Update data structures, Restore context of the selected process
>Process Switch, 是作用于进程之间的一种操作。当分派程序收回 当前进程的CPU 并准备把它分派给某个就绪进程时，该操作将被引用。 
>Mode Switch, 是进程内部所引用的一种操作。当进程映像所包含 的程序引用核心子系统所提供的系统调用时，该操作将被引用。
### Thread
![[Screenshot 2023-09-07 at 18.45.16.png]]
Key states: running ready blocked
- user-level threads
	- 在核外子系统实验
- kernel-level threads
	- Linux  OS/2
	- 核心子系统实现
- Combined Approaches
	- Solaris
- ![[Screenshot 2023-09-07 at 18.47.42.png]]

## Scheduling
### Aims
Response time
Throughput
Processor efficiency 处理机效率
Fairness 公平性 防止进程饥饿
### Types
批处理调度、分时调度、实时调度、多处理机调度
Long-term scheduling
Medium-term, short-term （长中短）程调度
![[Screenshot 2023-09-12 at 10.40.42.png]]
![[Screenshot 2023-09-12 at 10.40.52.png]]

#### Long-term scheduling
高级调度、作业调度
它为被调度作业或用户程序创建 进程、分配必要的系统资源，并将新创建的进程插入就绪队列， 等待Short-term scheduling
采用交换技术的系统将新创建的进程插入（就绪，挂起）队列， 等待Medium-term scheduling 
调度算法？选多少进系统？
#### Medium-term scheduling
配合swapping技术使用，提高内存利用率
#### Short-term scheduling
dispatcher, most frequent
Invoke when: clock interrupts, I/O, system call, signals
### Criteria
- User-oriented
	- Response Time 从提交到输出 用于分时
	- Turnaround time 从提交到完成 批处理系统
	- Deadlines 截止时间
		- 必须开始执行的最晚时间Starting deadline
		- 必须完成的最晚时间 Completion deadline
		- 评价实时系统
- System-Oriented
	- Balancing Resource 资源平衡
		- keep system resources busy(计算机在best-effort工作)
		- 长程、中程
	- Fairness
- Priorties
	- 优先级不同的多个ready队列
	- 可根据age生存期和执行历史修改优先级

### Algorithms
#### Decision Mode
- Nonpreemptive 非剥夺方式
	- Once a process is in the running state, it will continue until it terminates or blocks itself for I/O.
- Preemptive 剥夺方式
	- Currently running process may be interrupted and moved to the Ready state by the operating system
	- Allows for better service since any one process cannot monopolize the processor for very long.
	- 主要用于实时性要求较高的实时系统及性能要求 较高的批处理系统和分时系统
#### Short-term Algorithms
比较算法的平均周转时间
##### FCFS *First Come First Served*
A short process may have to wait a very long time before it can execute.
- Favors CPU-bound processes. 
- 对计算型好，对I/O的不公平
- I/O processes have to wait until CPU-bound process completes
适合与其他算法混合起来调度
##### Round Robin
RR 轮转发 时间片法
给每个进程相同时间片
Processor-bound进程充分利用时间片， 而I/O-bound进程在两次I/O之间仅需很少的CPU时间，却需要等待一个时间片。
改进： **VRR** *Virtual Round Robin* 比RR公平 用于分时系统
增加辅助队列接受IO阻塞完成的过程，调度优先与就绪队列
##### Shortest Process Next SPN
非剥夺，总是执行**剩下队列中**最短的进程 先来了就执行了
- 进程执行时间难确定
- 对长进程不公平
- 需要考虑紧迫程度
##### Shortest Remaining Time SRT
剥夺版本的SPN
- 进程剩余时间难确定
- 对长进程不公平
- 短进程提前完成，平均周转比SPN短
##### Highest Response Ratio Next
响应比
$$RR = \frac{time\ spent\ waiting + expected\ service\ time} { expected \ service\ time}.$$
Choose next process with the highest response ratio
- 若进程的等待时间相同，则要求服务时间短的进程优先（SPN）
- 若进程要求的服务时间相同，则等待时间长的进程优先（FCFS）
- 长进程随着等待时间增加，必然会被调度。
- 但是，很难准确确定进程要求的执行时间，且每次调度之前需要 计算响应比，增加系统开销
##### Feedback 反馈调度法
1.设置多个就绪队列，各队的优先权不同（从上而下优先级逐级降低）
2.各个队列中进程执行的时间片不同，优先权愈高时间片愈短
3.新进程首先放入第一队列尾，按FCFS方法调度，若一个时间片 未完成，到下一队列排队等待； 
4.仅当第一队列空时，调度程序才开始调度第二队列的进程，依次类推。
##### 时间计算
在进程调度的背景下，了解不同调度算法的性能是非常重要的。其中一个关键的性能指标是**平均带权周转时间**（Average Weighted Turnaround Time），它提供了一个衡量调度算法效率的有用视角。 
平均带权周转时间 
- **定义**：平均带权周转时间是指每个进程的周转时间与其服务时间的比值的平均值。它反映了进程从提交到完成所经历的时间相对于实际执行时间的比率。 
- **计算方法**： 
	1. **周转时间**：对每个进程而言，其周转时间是完成时间减去到达时间。 
	2. **带权周转时间**：每个进程的周转时间除以其服务时间（即CPU占用时间）。
	3. **平均带权周转时间**：所有进程的带权周转时间的总和除以进程数。 
	- **公式**： 
		- 周转时间：$T_i = 完成时间_i - 到达时间_i$ 
		- 带权周转时间：$WT_i = \frac{T_i}{服务时间_i}$ 
		- 平均带权周转时间：$\text{平均带权周转时间} = \frac{\sum_{i=1}^{n} WT_i}{n}$ 
		- 其中，$n$ 是进程的总数。 
	  - **重要性**：平均带权周转时间是一个衡量进程调度效率的关键指标，特别是在进程执行时间差异较大的情况下。它有助于评估调度算法对长作业和短作业的公平性和效率。
### Real-time scheduling
experiments control, robotics...
指能及时响应外部事件的请求，在规定的时间内完成对该事件的 处理，并控制所有实时任务协调一致地运行的计算机系统
- 实时控制系统 这个要求更高 导弹、自动驾驶等
- 实时信息处理系统

实时任务
- periodic 周期性
- aperiodic 非周期性
#### Characteristics of Real-time System
- **Deterministic** **确定性**设置预设时间，**运行时间确定**
- Responsiveness 响应性 收到中断后多长时间去服务
- User control
	- user specifies priority
	- specify paging
	- 哪些进程必须驻留在内存
	- 磁盘调度
	- 进程权限
- Reliability 可靠性
#### Features
- Fast context switch
- Small size
- Ability to respond to external interrupts quickly.
- Multitasking with interprocess communication tools such as semaphores, signals and events.
- Files that accumulate（存储） data at a fast rate
- Use of special sequential files that can accumulate data at a fast rate.
- Preemptive scheduling base on priority.
- Minimization of intervals during which interrupts are disabled.
- Delay tasks for fixed amount of time. 任务延迟有阈值
- Special alarms and timeouts（报警和超时处理 ）
#### Scheduling Method
##### Real-time Scheduling
- Round Robin Preemptive Scheduler（基于时间片的轮转调度法）
	- 秒级
	- 广泛应用于分时系统，也可用于一般的实时信息处理系统
- Priority-driven Nonpreemptive Scheduler（基于优先级非剥夺调度法）
	- 为实时任务赋予较高的优先级，将它插入就绪队列队首，只要正在执行的进程释放Processor，则立即调度该实时任务执行。
	- 数百毫秒至数秒范围
- Priority-driven Preemptive Scheduler（基于优先级的剥夺调度法） 
	- 当实时任务到达后，可以在时钟中断时，剥夺正在执行的低优先级进程的执行，调度执行高优先级的任务
	- 响应时间较短，一般在几十毫秒或几毫秒
- Immediate Preemptive Scheduler（立即剥夺调度法）
	- 要求操作系统具有快速响应外部事件的能力。一旦出现外部中断， 只要当前任务未处于临界区，便立即剥夺其执行，把处理机分配 给请求中断的紧迫任务。
	- 调度时延可以降至100微秒，甚至更低
- Static table-driven （静态表驱动调度法) 
	- Determines at run time when a task begins execution. 	
- Static priority-driven preemptive (静态优先级剥夺调度法)
	- Traditional priority-driven scheduler is used.
- Dynamic planning-based (动态计划调度法)
- Dynamic best effort （动态最大努力调度法）
##### Deadline scheduling
- Scheduling tasks with the earliest deadline minimized the fraction of tasks that miss their deadlines
- What sort of preemption is allowed? 
	- When starting deadlines are specified, then a non-preemptive scheduler makes sense。在执行完强制子任务或临界区后，阻塞自己。 
	- For a system with completion deadlines, a preemptive strategy is most appropriate.

###### Periodic tasks with completion deadlines
- 由于此类任务是周期性的、可预测的，可采用静态表驱动之最早 截止时间优先调度算法，使系统中的任务都能按要求完成。
![[Screenshot 2023-09-14 at 11.46.22.png]]
###### Aperiodic tasks with starting deadlines
- 可以采用最早截止时间优先调度算法或允许CPU空闲的EDF调度算法。 
- Earliest Deadline with Unforced Idle Times(**允许CPU空闲**的EDF调度算法）
	- 指优先调度最早截止时间的任务，并将它执行完毕才调度下一个任务。即使选定的任务未就绪，允许CPU空闲等待，也不能调度其他任务。尽管CPU的利用率不高，但这种调度算法可以**保证系统中的任务都能按要求完成**。
![[Screenshot 2023-09-14 at 11.51.31.png]]
##### Rate Monotonic Scheduling
- Assigns priorities to tasks on the basis of their periods. 
- Highest-priority task is the one with the shortest period. 
- Period（任务周期），指一个任务到达至下一任务到达之间的时间范围。
- Rate （任务速度），即周期（以秒计）的倒数，以赫兹为单位
- 任务周期的结束，表示任务的硬截止时间。任务的执行时间不应超过任务周期
- CPU的利用率 = 任务执行时间 / 任务周期
- 在RMS调度算法中，如果以任务速度为参数，则优先级函数是一 个单调递增的函数
## Concurrency
### Mutual Exclusion and Synchronization

互斥解决资源竞争问题
**Critical sections 临界区**，是访问临界资源的那段代码
一次只允许一个进程使用临界区中的资源 **临界资源**

#### 互斥的要求
- Only one process at a time is allowed in the critical section for a resource
- A process that halts in its non-critical section must do so without interfering with other processes
- No deadlock or starvation
- A process must not be delayed access to a critical section when there is no other process using it
- No assumptions are made about relative process speeds or number of processes
- A process remains inside its critical section for a finite time only.
**空闲让进 忙则等待 有限等待 让权等待**

#### Software Application
- To illustrate most of the common bugs encountered in developing concurrent programs. 
- Is likely to have high processing overhead. **间接开销大**
- The risk of logical errors is significant

#### Hardware Support
##### Interrupt Disabling
能保证但代价非常高 临界区整个系统都屏蔽了中断
##### Special Machine Instruction
- 指令是原子性的
	- 在指令内完成检测和设置标志
	- reading and writing
	- reading and testing
	- Test and Set Instruction
		- ![[Screenshot 2023-09-21 at 10.45.19.png]]
		- 能实现互斥，Bolt  螺栓
		- 哪个进程能得到取决于调度，可能会出现饥饿
	- Exchange Instruction
		- ![[Screenshot 2023-09-21 at 10.48.46.png]]
	- Advantages
		- 无进程数量限制，不限制于单或多处理器分享内存
		- 简单易验证 可支持多个临界区
	- Disadvantages
		- Busy-waiting is employed 进入区一直是循环
		- 可能饥饿
		- 可能死锁
			- 低优先级进程占用高优先级所需的临界区，高优先级程序需要等待
#### Semaphores
- Semaphores
	- Special variable called a ***semaphore*** is used for signaling.
	- Queue is used to hold processes waiting on the semaphore
	- General Semaphore 记录性 一个整形和一个排队队列
		- ![[Screenshot 2023-09-21 at 11.21.59.png]]
		- 互斥信号量：用于申请或释放资源的使用权，常初始化为1。
		- 资源信号量用于申请或归还资源，可以初始化为大于1的正整数， 表示系统中某类资源的可用个数。
		- $s.count\geq 0$ 表示还可执行wait(s)而不会阻塞的进程数（可用资源数）
		- $s.count < 0$ 表示被阻塞进程的个数
		- 当用s来实现n个进程的互斥时，s.count 的取值范围为 $1～－(n-1)$
	- Binary Semaphore 二进制信号量
	- 工程实践证明，利用**信号量方法实现进程互斥是高效的**， 一直被广泛采用
##### 解决几个经典问题
###### Producer/Consumer Problem
生产者消费者共享buffer 一个取一个放
需要互斥进入缓冲区，保证执行结果稳定
两者还需要同步读写，满了不能写，空了不能读
> 想要设计系统需要学会自己判断进程对同步和互斥的需求

![[Screenshot 2023-09-21 at 11.31.38.png]]
![[Screenshot 2023-09-21 at 11.42.40.png]]

- 进程应该先申请资源信号量，再申请互斥信号量，顺序不能颠倒。
- 对任何信号量的wait与signal操作必须配对。同一进程中的多对wait与signal语句只能嵌套，不能交叉。
- wait永远先于signal
- **交叉唤醒 非常巧妙啊**
###### Reader/Writers Problem
- Any number of readers may simultaneously read the file.
- Only one writer at a time may write to the file.
- If a writer is writing to the file, no reader may read it.

**Readers have priority**: 指一旦有读者正在读数据，允许多个读者同时进入读数据，只有当全部读者退出，才允许写者进入写数据。可能写饥饿。![[Screenshot 2023-09-26 at 10.36.14.png]]
如果有进程在读，需要把写资源占用wait(wsem)阻塞写者，无进程读时释放。
所有读者共享readcount这个变量，x这个变量是实现readcount的访问互斥。在x信号量上可能有多个读者排队，等待访问readcount。

**Writers have priority**：指只要有一个writer申请写数据，则不再允许新的reader进入读数据。![[Screenshot 2023-09-26 at 10.44.51.png]]
> 这个自己看 不要求 五个信号量太多了

#### Monitors 管程
用信号量实现互斥，编程容易出错（wait, signal)
Monitor is a software module，由若干过程、局部于管程的数据、 初始化语句（组）组成
Chief characteristics
- Local data variables are accessible only by the monitor.
- Process enters monitor by invoking one of its procedures.
- Only one process may be executing in the monitor at a time
- ![[Screenshot 2023-09-26 at 10.48.16.png]]
#### Message Passing
既传输消息，又能实现互斥控制
操作：send(dest, msg); recv(src, msg);

#### Passing methods
- Sender and receiver may or may not be blocked (waiting for message).
- Blocking send, blocking receive 
	- Both sender and receiver are blocked until message is delivered. 
	- Called a rendezvous(紧密同步，汇合)
- Nonblocking send, blocking receive
	- Sender continues processing such as sending messages as quickly as possible. 
	- Receiver is blocked until the requested message arrives. 
- Nonblocking send, nonblocking receive
	- Neither party is required to wait

#### Addressing 寻址
- Direct
	- given destination
	- receiver know source
- Indirect
	- send to mailbox(queue), a shared data structure

若采用nonblocking send, blocking receive 共享mail mutex
![[Screenshot 2023-09-26 at 10.56.52.png]]

用消息传递方法解决生产者消费者问题-- using two mailboxes
Mayconsume: Producer存放数据，供消费者取走，buffer，先清空
Mayproduce: 存放空消息的buffer空间
#### 互斥总结
- 利用wait、signal原语对Semaphore互斥操作实现,powerful and flexible
- 可用软件方法实现互斥，如Dekker 算法、Peterson算法等 （但增加处理负荷）
- 可用硬件或固件方法实现互斥，如屏蔽中断、Test and Set 指令等 （属于可接受的忙等）

### Deadlock and Starvation
死锁：
![[Screenshot 2023-09-19 at 10.59.36.png]]
死锁比饥饿更严重
Permanent blocking of a set of processes that either compete for system resources or communicate with each other.

**No efficient solution.**
#### 产生原因
死锁与进程的推进顺序有关。
##### 可重用资源
Reusable resources
- Used by one process at a time and not depleted(耗尽)by that use. 
- Processes obtain resources that they later release for reuse by other processes.
- Processors, I/O channels, main and secondary memory, files, databases, and semaphores.
- **Deadlock occurs if each process holds one resource and requests the other.**
##### 可消耗资源
Consumable Resources
- Created (produced) and destroyed (consumed) by a process. 
- Interrupts, signals, messages, and information in I/O buffers
- Deadlock occurs if receive is blocking![[Screenshot 2023-09-26 at 11.09.00.png]]
死锁产生必要条件：
- Mutual Exclusion
- Hold-and-wait
- No preemption
导致：（充分条件）
- Circular wait
#### Deadlock Prevention
##### 间接方法
禁止互斥（不行）
禁止“保持并等待”
禁止“不剥夺”条件
##### 直接方法
禁止环路等待发生：低效，影响执行速度，阻碍资源正常分配。
即禁止“环路等待”条件：可以将系统的所有资源按类型不同进行线性排队，并赋予不同的序号。进程对某类资源的申请只能按照序号递增的方式进行。
#### Deadlock Avoidance
预防：限制，降低系统性能
避免：在为进程分配资源之前通过计算，判断是否会导致死锁，不会才分配资源。在新进程创建或已有进程新增资源请求时都做检查。

Resource Allocation Denial算法：Banker's Algorithm
> 银行放贷的时候，风险大，银行就不会给你贷款，资金是银行的资源，跟操作系统资源分配情景类似

系统状态
- safe state 存在一个进程执行序列不会导致系统死锁 只要安全则可避免死锁
- unsafe state 反之 并非所有不安全状态都是死锁状态
记录几张表：申请矩阵、当前资源分配情况、空闲资源，检查可否在资源范围内执行完所有任务。![[Screenshot 2023-09-26 at 12.00.15.png]]
Implementation of Banker's algorithm
- 数组 $available[j]=k$
- 最大需求矩阵 claim matrix $Max(i,j)=k$ 进程i对j资源需求数
- 分配矩阵allocation matrix $allocation(i,j)=k$
- 需求矩阵 $Need[i,j]=k$, $need=max-allocation$
- ![[Screenshot 2023-09-28 at 10.59.52.png]]
- 安全性算法：设置两个工作向量
	- 设置一个数组Finish[n]。当Finish[i]＝True（0 ≤ i ≤ n，n为 系统中的进程数）时，表示进程Pi可以获得其所需的全部资 源，而顺利执行完成。
	- 设置一个临时向量Work，表示系统可提供给进程继续运行 的资源的集合。安全性算法刚开始执行时， Work := Available
	- 从进程集合中找到一个满足下列条件的进程： Finish[i] = false； 并且 Need ≤ Work；
	- 若找到满足该条件的进程，则执行：“当进程Pi获得资源后，将顺利执行直至完成，并释放其所拥 有的全部资源，故应执行以下操作： Work := Work + Allocationi； 以及 Finish[i] := True 然后重复本步骤”，否则执行“如果所有进程的Finish[i] = True，则表示系 统处于安全状态, 否则系统处于不安全状态”
	- **贪心思想**

要求：
- 进程必须申明所需资源总量
- 进程之间独立，无同步
- 系统必须提供固定数量的资源
- 进程占用资源时不能退出
#### Deadlock Detection
检测死锁不同于预防死锁，不限制资源访问方式和资源申请。
OS周期性地执行死锁检测例程，检测系统中是否出现“环路等待”
检测到之后？
- Abort all deadlocked processes
- Back up them to checkpoint, restart
- successively abort/preempt resources
选什么deadlock process进行abort
- least amount of processor time consumed so fat
- least output
- most estimated time remaining
- least resources allocated so fat
- lowest priority
#### Deadlock Giving-up(不管死锁)
可以从上面看到，预防死锁的开销非常之大。
个人操作系统懒得考虑预防死锁。对于个人电脑**PC机，死机了就死机了**，损失也不大，**重启**即可。
> 所以女生找你修电脑，你回复“重启解决99%问题”是有操作系统原理上的道理的。
#### 哲学家进餐问题
描述：有5个哲学家，他们的生活方式是交替地进行思考和进餐。 哲学家们共用一张圆桌，分别坐在周围的五张椅子上。圆桌中间放有一大碗面条，每个哲学家分别有1个盘子和1支叉子。如果哲学家想吃面条，则必须拿到靠其最近的左右两支叉子。进餐完毕，放下叉子继续思考。

要求：设计一个合理的算法，使全部哲学家都能进餐（非同时）。算法必须避免死锁和饥饿，哲学家互斥共享叉子。

直接分配，根据逻辑写傻程序会导致死锁
![[Screenshot 2023-09-28 at 11.24.17.png]]
一种解决方案：只允许4个同时进餐，则至少有一个哲学家可以拿到两支叉子进餐
![[Screenshot 2023-09-28 at 11.24.52.png]]

> 进程调度将计算机中最贵的资源cpu高效的利用起来

## 本章作业
### 课堂练习
三个进程P1、P2、P3互斥使用一个包含N（N>O）个单元的缓冲区。
- ﻿P1每次用produce0生成一个正整数并用put（）送入缓冲区的某一空单元中；
- ﻿﻿P2每次用getodd()从该缓冲区取出一个奇数并用countodd()统计奇数个数；
- ﻿P3每次用geteven()从缓冲区取出一个偶数并用counteven()统计偶数个数。
- ﻿请用信号量机制实现这三个进程的同步与互斥活动，并说明所定义信号量的含义。要求用伪代码描述。  
```cpp
//shared data
semaphore mutex=1;//互斥访问共享缓冲区;
semaphore empty=N;//共享缓冲区同步信号量
semaphore odd=0;//奇数缓冲区同步信号量
semaphore even=0;//偶数缓冲区同步信号量

P1
 	while true:
        int num=produce();//生产一个数据num
 
        wait(empty);//消耗掉一个空单元
        wait(mutex);//互斥访问缓冲区;
        
        put(num);//将数据num放入缓冲区
        
        signal(mutex);
        if(num%2!=0)//奇数
            signal(odd);
        else//偶数
            signal(even);
P2
	while true:
        wait(odd);//有奇数时才能取出奇数
  		wait(mutex);//互斥访问缓冲区
       
        getodd();//取出奇数
        
        signal(mutex);
        signal(empty);//产生一个空单元
      
        countodd();//奇数个数加一
P3
	while true:
        wait(even);//有偶数时才能取出偶数
        wait(mutex);
        
        geteven();
        
        signal(mutex);
        signal(empty);
      
        counteven();
```
### 作业题1
图书馆有N个座位，一张登记表，要求（1）阅读者进入时登记，取得座位号；（2）出来时注销。请用P。V操作描述一个读者的使用过程。
> P V 操作是原语， P占用 V释放
> Dijkstra, one of the inventors of _semaphores_, used P and V. The letters come from the Dutch words Probeer (try) and Verhoog (increment).

```cpp
semaphore Seat=N, Sheet=1;

Process Reader(int id){
	//Enter
	P(Seat);
	
	P(Sheet);
	GetSeatNumber();
	V(Sheet);

	Read()

	//Quit
	P(Sheet);
	Logout();
	V(Sheet);

	V(Seat);
}

Reader(1,2,3...n);
```

### 作业题2
桌上有一空盘，最多允许存放**一只**水果。爸爸可向盘中放一个苹果或放一个桔子；儿子专等吃盘中的桔子，女儿专等吃苹果。用P、V操作实现爸爸，儿子，女儿三个并发进程的同步。因为是一只水果，资源量只有一个，自身就带有“同时只允许一个人访问”的意思。
```cpp
semaphore: disk=1, isApple=0, isOrange=0;

Process Father{
	while(1){
		FruitKind = GetFruit();//Apple or Orange
		P(disk);
		if(FruitKind == Apple)
			V(isApple);
		else
			V(isOrange);
	}
}

Process Son{
	P(isOrange);
	V(disk);
	Eat();
}

Process Daughter{
	P(isApple);
	V(disk);
	Eat();
}
```