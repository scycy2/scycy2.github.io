---
title: Process Scheduling
date: 2020-10-13 12:11:50
tags: ['Operating System']
categories: ['class notes']
cover:
summary: Process Classification and Process Scheduling
img:
---

#### 1. CPU密集型进程

1. CPU密集型也叫计算密集型，指的是系统的硬盘、内存性能相对CPU要好很多，此时，系统运作大部分的状况是CPU Loading 100%，CPU要读/写I/O(硬盘/内存)，I/O在很短的时间就可以完成，而CPU还有许多运算要处理，CPU Loading很高。
2. 在多重程序系统中，大部份时间用来做计算、逻辑判断等CPU动作的程序称之CPU bound。例如一个计算圆周率至小数点一千位以下的程序，在执行的过程当中绝大部份时间用在三角函数和开根号的计算，便是属于CPU bound的程序。
3. CPU bound的程序一般而言CPU占用率相当高。这可能是因为任务本身不太需要访问I/O设备，也可能是因为程序是多线程实现因此屏蔽掉了等待I/O的时间。

#### 2. IO密集型进程

1. IO密集型指的是系统的CPU性能相对硬盘、内存要好很多，此时，系统运作，大部分的状况是CPU在等I/O (硬盘/内存) 的读/写操作，此时CPU Loading并不高。
2. I/O bound的程序一般在达到性能极限时，CPU占用率仍然较低。这可能是因为任务本身需要大量I/O操作，而pipeline做得不是很好，没有充分利用处理器能力。

#### 3. CPU密集型 vs IO密集型

1. 计算密集型任务的特点是要进行大量的计算，消耗CPU资源，比如计算圆周率、对视频进行高清解码等等，全靠CPU的运算能力。这种计算密集型任务虽然也可以用多任务完成，但是任务越多，花在任务切换的时间就越多，CPU执行任务的效率就越低，**所以，要最高效地利用CPU，计算密集型任务同时进行的数量应当等于CPU的核心数。**

   计算密集型任务由于主要消耗CPU资源，因此，代码运行效率至关重要。Python这样的脚本语言运行效率很低，完全不适合计算密集型任务。对于计算密集型任务，最好用C语言编写。

2. 第二种任务的类型是IO密集型，涉及到网络、磁盘IO的任务都是IO密集型任务，这类任务的特点是CPU消耗很少，任务的大部分时间都在等待IO操作完成（因为IO的速度远远低于CPU和内存的速度）。对于IO密集型任务，任务越多，CPU效率越高，但也有一个限度。常见的大部分任务都是IO密集型任务，比如Web应用。

   IO密集型任务执行期间，99%的时间都花在IO上，花在CPU上的时间很少，因此，用运行速度极快的C语言替换用Python这样运行速度极低的脚本语言，完全无法提升运行效率。对于IO密集型任务，最合适的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C语言最差。

#### 4. 进程调度

1. **Classification by Time Horizon**

* **Long term**: applies to new processes and controls the **degree of multiprogramming** by deciding which processes to admit to the system when a good **mix** of **CPU** and **I/O bound processes** is favorable to keep all resources as busy as possible.
* **Medium term**: controls **swapping** and the **degree of multi-programming** (Memory management).
* **Short term (dispatcher)**: decide which process to run next.

2. **Classification by Approach**

* **Non-preemptive(非抢占式或非剥夺式)**: processes are only interrupted voluntarily
* **Preemptive(抢占式或剥夺式)**: processes can be **interrupted forcefully** or **voluntarily**
  * E.g. preemptive scheduling algorithm picks a process and lets it run for a maximum of some fixed time. (clock interrupt)
  * This requires context switches, which generates **overhead**
  * Prevents processes from **monopolizing(垄断，独占) the CPU**
  * **Most popular** modern operation systems are preemptive

#### 5. 性能评估

1. **User oriented criteria**: 

* **Response time(响应时间)**: minimize the time between creating the job and its first

  execution

* **Turnaround time(周转时间)**: minimize the time between creating the job and finishing it

2. **System oriented criteria**:

* **Throughput(吞吐量)**: maximize the number of jobs processed per hour
* **Fairness(公平)**: 
  * Are processing power/waiting time equally distributed?
  * Is there any process with excessively long waiting time? (**starvation**)

#### 6. 调度算法

1. Overview:

* **Algorithms**:
  * First Come First Served (FCFS) / First In First Out (FIFO) (**Batch System**)
  * Shortest job first (**Batch System**)
  * Round Robin (**Interactive System**)
  * Priority Queue (**Interactive System**)
* Performance measured used:
  * **Average response time(平均响应时间)**: the average of the time taken for all the processes to start
  * **Average turnaround time(平均周转时间)**: the average time taken for all the processes to finish

2. **First Come First Served**:

* Concept: a **non-preemptive algorithm** that operates as a **strict queueing mechanism** and schedules the processes in the same order that they were added to the queue
* Advantages:
  * **positional fairness** and easy to implement
* Disadvantages:
  * **Favours long processes** over short ones (think of the supermarket checkout!)
  * Could **compromise resource utilisation**, i.e., CPU vs. I/O devices

3. **Shortest Job First:**

* Concept: A **non-preemptive algorithm** that starts processes in order of **ascending processing time** using a provided/known estimate of the processing
* Advantages: results in the **optimal turn around time**
* Disadvantages:
  * **Starvation** might occur
  * **Fairness** is compromised
  * **Processing times have to be known** beforehand or estimated by

4. **Round Robin:**

* Concept: a **preemptive version of FCFS** that forces **context switches** at **periodic intervals** or **time slices (Time quantum)**
  * Processes run in the order that they were added to the queue
  * Processes are forcefully **interrupted by the timer**
* Advantages:
  * Improved **response time**
  * Effective for general purpose **time sharing systems**
* Disadvantages:
  * Increased **context switching** and thus overhead
  * Can **reduce to FCFS** (只要时间片足够长，就是FCFS算法)

5. **Priority Queues:**

* Concept: A **preemptive algorithm** that schedules processes by priority (high to low)
  * The process priority is saved in the **process control block**
* Advantages:
  * can **prioritise I/O bound jobs**
* Disadvantages:
  * low priority processes may suffer from **starvation** (with static priorities)
