---
title: Thread
date: 2020-10-14 13:35:03
tags: ['Operating System']
categories: ['class notes']
cover:
summary: Thread
img: /medias/featureimages/8.jpg
---

#### 1. Thread Usage

* Thread can be viewed as **miniprocesses** within a process
* Why do we need multiple threads in a process?
  * Multiple **related activities** apply to the **same resources**, these resources should be accessible/shared (share address space and data)
  * Easy to create and destroy.
    * 10 - 100 times faster than creating a process

#### 2. User Threads (Many-to-One)

* **Thread management** (creating, destroying, scheduling, thread control block manipulation) is carried out **in user space** with the help of a **user library**
* The process maintains a **thread table** managed by the **runtime system** without the **kernel's knowledge**
  * Similar to **process table**
  * Used for **thread switching**
  * Tracks thread related information
* They can be implemented on **OS** that **does not support multithreading**
* <img src="Thread/Screen Shot 2020-12-21 at 9.15.22 PM.png" style="zoom:50%;" />

* Advantages:
  * Threads are in user space (i.e., **no mode switches** required)
  * **Full control** over the thread scheduler (e.g. website server)
  * **OS independent** (threads can run on OS that do not support them)
  * The runtime system can **switch local blocking threads** in user space (e.g. wait for another thread to complete)
* Disadvantages:
  * **Blocking system calls** suspend the entire process (user threads are mapped onto a single process, managed by the kernel)
  * **Page fault** result in blocking the process
  * **No true parallelism** (a process is scheduled on a single CPU)\

#### 3. Kernel Threads (One-to-One)

* <img src="Thread/Screen Shot 2020-12-21 at 9.41.55 PM.png" style="zoom:50%;" />
* The **kernel manages** the threads, user application accesses threading facilities through **API** and **System calls**
  * **Thread table** is in the kernel, containing thread control blocks (subset of process control blocks)
  * If a **thread blocks**, the kernel chooses thread from same or different process
* Advantages:
  * **True parallelism** can be achieved
  * No run-time system needed
* Frequent **mode switches** take place, resulting in lower performance

#### 4. Hybrid Implementation

* User threads are **multiplexed** onto kernel threads
* Kernel sees and schedules the kernel threads (a limited number)
* User application sees user threads and creates/schedules these (an "unrestricted" number)
* <img src="Thread/Screen Shot 2020-12-22 at 2.14.12 PM.png" style="zoom:50%;" />

