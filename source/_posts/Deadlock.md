---
title: Deadlock
date: 2020-10-20 12:55:14
tags: ["Operating System"]
categories: ['class notes']
cover:
summary: Deadlock
img:
mathjax: true
---

#### 1. Definition

* A set of processes is deadlocked if **each process** in the set is waiting for **an event** that only **the other process** in the set can cause
  * Each **deadlocked process** is **waiting for** a resource held by **another deadlocked process** (which cannot run and hence cannot release the resources)
  * This can happen between **any number of processes** and for **any number of resources**

#### 2. How do they occur?

* On occasions, **multiple processes** will **require access** to **multiple mutually exclusive resources**
* Process A and B request the resources in **opposite orders** and end up in deadlock
  * Deadlocks can occur on the **same machine** or between **mutiple machines** (e.g. resources are requested over the network) and **any number of resources**
* A resource (e.g. a devide, a data record, file, semaphore) can be **acquired, used,** and **released**
  * A resource can be **preemptable**, i.e., it can be forcefully taken away from the process without permanent adverse effect
  * A resource can be **non-preemptable**, i.e., it cannot be taken away from a process without permanent adverse effect
* **Deadlocks only occur** for **non-preemptable resources** since preemptable resources can be temporarily taken away to recover from the deadlock
* If a non-preemptable resource is requested but not available, the **process is made to wait**

#### 3. Minimum Conditions

* **Four conditions** must hold for deadlocks to occur:
  * **Mutual exclusion**: a resource can be assigned to at most one process at a time
  * **Hold and wait condition**: a resource can be held whilst requesting new resources
  * **No preemption**: resources cannot be forcefully taken away from a process
  * **Circular wait**: there is a circular chain of two or more processes, waiting for a resource held by the other processes
* **No deadlocks** can occur if one of the conditions is **not satisfied**

#### 4. Detecting Deadlocks (A Graph Based Approach)

* Deadlocks can be modeled using **directed graphs**:
  * **Resources** are represented by **squares** and **processes** are represented by **circles**
  * A directed arc from a **square** (resource) and a **circle** (process) means that the resource was **requested and granted**, i.e. is allocated to the process
  * A directed arc from a **circle** (process) to a **square** (resource) indecates that the process has requested the resource and is **waiting to obtain it**
* A **cycle** in the graph means that a **deadlock** occurs for the respective resources and processes

#### 5. Matrix approach (detection)

* A **matrix based algorithm** is used when **multiple "copies" of** the same **resource** exist
* Let $P_1$ to $P_n$ denote $n$ **processes**
* Let there exist $m$ **resources** classes
  * Let $E_i$ denote the "existing" resources of type $i$
  * Let $A_i$ denote the number of allocated and available resources ($A_i$) of type $i$ is equal to $E_i$
* The algorithm uses **two vectors** describing the existing and available resources ($E$ and $A$), and **two matrices** to describe the **current** and **requested** resources per process ($C$ and $R$)
* Graph Approach (cycle detection algorithm)
  * Select a process for which the requested resource allocation can be satisfied
    * $R_{ij}\leq A_i$ for all j in [1..m]
    * Run the process and 'mark' it as completed
  * Reclaim the process's resources by adding row $C_i$ to $A_i$
  * Repeat the above and terminate the algorithm if no 'unmarked' or 'completable' process can be found

#### 6. Recover from Deadlocks

* **Preemption (抢占)**: the resource is forcefully removed from one of the processes (this is likely to have an ill effect)
  * Take a resource away from a process, have another process use it, and then give it back. (difficult/impossible)
* **Rollback mechanisms (回滚)**: build in check points that allow the process to be restarted (periodically)
  * The check points contain the **memory image** and **resource states**
  * Multiple checkpoints should be maintained (not overwrite)
  * The process that owns the "deadlocked" resources is rolled back
* **Kill** a strategically chosen **process** to release its resources
  * The process should be easy to restart
  * The process can be re-run from the beginning with no ill effect

#### 7. Avoidance

* Deadlock detection approaches are **reactive**, i.e., they only detect and recover from deadlocks, but do not **prevent** them
* **Deadlocks can be avoided** by carefully considering resource allocation
* A **safe state** is one where there is **a feasible order** for the processes to finish **without deadlock**
* A state might **not** be **in deadlocked**, but **still** be **unsafe**
* The "**bankers algorithm**" (Dijkstra) can be used to avoid deadlocks
  * Find customer closet to max resource allocation
  * Check whether remaining resources can satisfy the maximum request
  * Return the resources from this process to the set of available resources
  * Find next process closest to its max resource allocation
* The state is safe *iff* all processes can finish, even when they request all their resources at the same time
* The bankers algorithm for **multiple resources** is a genrealisation of the bankers algorithm for a single resource
  * **Two matrices** are maintained, one for the allocated resources $C$, one for the remaining resources that are required $R$
  * **Three vectors** are maintained: the existing resources, the possessed resources, the available resources
  * Find a row in $R$ for which all corresponding resource requirements are less or equal to $A$ (selection order has no influence)
    * If no row satisfies this, the state is **unsafe**
    * else 'run the process' and add its resources to $A$
  * Repeat the above until all processes have finished
    * The state is **safe** if **all processes** complete successfully
