---
title: MapReduce Paradigm
date: 2021-09-17 14:16:13
tags: ['Cloud Computing']
categories: ['self-study notes']
cover:
summary: MapReduce (Scheduling, Fault-tolerance)
img:
---

#### 1. What is MapReduce?

* Terms are borrowed from Functional Language (e.g., Lisp)

* Example: Sum of Square

  * (map square '(1 2 3 4))

    * Output: (1 4 9 16)
    * processes each record sequentially and independently

  * (reduce + '(1 4 9 16))

    * (+ 16 (+ 9 (+ 4 1)))

    * Output: 30
    * processes set of all records in batches

* **Map**

  * Parallelly Process a large number of individual records to generate intermediate key/value pairs

* **Reduce**

  * Each key assigned to one Reduce
  * Parallelly Processes and merges all intermediate values by partitioning keys

#### 2. MapReduce Scheduling

* Programming MapReduce

  * Externally: For user

    1. Write a Map program (short), write a Reduce program (short)
    2. Submit job: wait for result
    3. Need to know nothing about parallel/distributed programming

  * Internally: For the Paradigm and Scheduler

    1. Parallelize Map
    2. Transfer data from Map to Reduce
    3. Parallelize Reduce
    4. Implement Storage for Map input, Map output, Reduce input and Reduce output

    (Ensure that no Reduce starts before all Maps are finished. That is, ensure the ***barrier*** between the Map phase and Reduce phase)

* Inside MapReduce

  * For the cloud:
    1. Parallelize Map: each map task is independent of the other
       * All Map outpur records with same key assigned to same Reduce
    2. Transfer data from Map to Reduce:
       * All Map output records with same key assigned to same Reduce task
       * use partitioning function
    3. Parallelize Reduce: each reduce task is independent of the other
    4. Implement Storage for Map input, Map output, Reduce input, and Reduce output
       * Map input: from distributed file system
       * Map output: to local disk (at Map node); uses local file system
       * Reduce input: from (multiple) remote disks; uses local file systems
       * Reduce output: to distributed file system

* **The YARN Scheduler**

  * Used in Hadoop 2.x +

  * YARN = Yet Another Resource Negotiator

  * Treats each server as a collection of *containers*
    * Container = some CPU + some memory

  * Has 3 main components

    * Global *Resource Manager (RM)*
      * Scheduling

    * Per-server *Node Manager (NM)*
      * Daemon and server-specific functions

    * Per-application (job) *Application Master (AM)*

      * Container negotiation with RM and NMs

      * Detecting task failures of that job

  * <img src="MapReduce-Paradigm/Screen Shot 2021-09-17 at 1.58.30 PM.png" style="zoom:50%;" />

#### 3. MapReduce Fault-Tolerance

* Server Failure
  * NM heartbeats to RM
    * If server fails, RM lets all affected Ams know, and AMs take action
  * NM keeps track of each task running at its server
    * If task fails while in-progress, mark the task as idle and restart it
  * AM heartbeats to RM
    * On failure, RM restarts AM, which then syncs up with its running tasks
* RM Failure
  * Use old checkpoints and bring up secondary RM
* Heartbeats also used to piggyback container requests
  * Avoid extra messages
* Slow Servers
  * The slowest machine slows the entire job down
  * Dueto Bad Disk, Network Bandwidth, CPU, or Memory
  * Keep track of "progress" of each task (% done)
  * Perform backup (replicated) execution of straggler task: task considered done when first replica complete. Called **Speculative Execution**
* Locality
  * Since cloud has hierarchical topology (e.g. racks)
  * GFS/HDFS stores 3 replicas of each of chunks
    * Maybe on different racks
  * Mapreduce attempts to schedule a map task on
    * a machine that contains a replica of corresponding input data, or failing that,
    * on the same rack as a machine containing the input, or failing that,
    * Anywhere
