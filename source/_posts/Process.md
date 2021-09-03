---
title: Process
date: 2020-12-21 13:20:47
tags: ["Operating System", "Context Switching"]
categories: ["class notes"]
cover:
summary: Process
img:
---

#### 1. Definition

* The simplified definition: "a process is a **running instance** of a program"
  * A program is **passive** entity and "sits" on a disk, not doing anything.
  * A process has **control structures** associated with it, may be **active**, and may have **resources** assigned to it (e.g. I/O devices, memory, processor). It has a program, input, output and a state.
  * A single **processor** may be shared among several **processes**.
* A process is registered with the OS using its **"control structures"**: i.e. an entry in the OS's **process table** to a **process control blocks** (PCB)
* The **process control block** contains all information necessary to **administer the process** and is **essential** for **context switching** in **multiprogramming systems**

#### 2. Menory Image of Processes

* A **process' memory image** contains:
  * The program **code** (could be shared between multiple processes running the same code)
  * A **data** segment: process-specified data (input and output, global variable)
  * call **stack**: keep track of active subroutines (function parameters, local variables)
  * **Heap**: hold intermediate computation data generated during run time
* Every process has its own **logical address space**, in which the **stack** and **heap** are placed at **opposite sides** to allow them to grow

#### 3. Process States and Transitions

* Diagram

  <img src="Process-进程/Screen Shot 2020-12-21 at 1.50.28 PM.png" style="zoom:50%;" />

* State transitions include:

  * 1  **New -> ready**: admit the process and commit to execution
  * 2  **Running -> blocked**: e.g. process is waiting for input or carried out a system call
  * 3  **Ready -> running**: the process is selected by the **process sceduler**
  * 4  **Blocked -> ready**: event happens, e.g. I/O operation has finished
  * 5  **Running -> ready**: the process is preempted, e.g., by a **timer interrupt** or by **pause**
  * 6  **Running -> exit**: process has finished, e.g. program ended or exception encountered

#### 4. Context Switching (Multiprogramming)

* Modern computers are **multiprogramming** systems:

* Assuming a **single processor system**, the instructions of individual processes are executed **sequentially**

  * CPU's rapid switching back and forth from process to process is called **multiprogramming**.
  * Multiprogramming is achieved by **alternating** processes and **context switching**
  * **True parallelism** requires **mutiple processors**

* When a **context switch** takes place, the system **saves the state** of the old process and **loads the state** of the new process (created **overhead**)

  * **Saved** => the process control block is **updated**
  * **(Re-)started** => the process control block **read**

* A **trade-off** exists between **"responsiveness"** and **"overhead"**

  * **Short time slices** result in **good response time** but **low effective "utilisation"**
  * **Long time slices** result in **poor response time** but **better effective "utilisation"**

* The OS uses **process control block** and a **process table** to manage processes and maintain their information

* A **process control block** contains three types of **attributes**:

  * **Process identification** (PID, UID, Parent PID)
  * **Process state information** (user registers, program counters, stack pointer, program status word, memory management information, files, etc.)
  * **Process control information** (process state, scheduling information, etc.)

* **Process control blocks** are **kernel data structures**, i.e. they are **protected** and only accessible in **kernel mode!**

  * Allowing user applications to access them directly could **compromise their integrity**
  * The **operating system manages** them on the user's behalf through **system calls**

* Switching Processes

  * <img src="Process-进程/Screen Shot 2020-12-21 at 3.42.30 PM.png" style="zoom:50%;" />

  * 1  Save process state (program counter, registers)
  * 2  Update PCB (running -> ready)
  * 3  Move PCB to appropriate queue (ready/blocked)
  * 4  Run scheduler, select new process
  * 5  Update to running state in PCB
  * 6  Update memory structures
  * 7  Restore process
