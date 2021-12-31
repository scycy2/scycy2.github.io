---
title: Introduction to Clouds
date: 2021-09-12 16:38:59
tags: ["Cloud Computing"]
categories: ['self-study notes']
cover:
summary: Introduction to clouds
img: /medias/featureimages/29.jpg
---

#### 1. What is a Cloud?

* Cloud = Lots of storage + compute cycles nearby

* A single-site cloud (aka "Datacenter") consists of
  * Compute nodes (grouped into racks)
  * Switches, connecting the racks
  * A network topology, e.g. hierarchical
  * Storage (backend) nodes connected to the network
  * Front-end for submitting jobs and receiving client requests
  * Software Services
* A geographically distributed cloud consists of
  * Multiple such sites
  * Each site perhaps with a different structure and services

#### 2. What's new in today's clouds

* Four features new in today's clouds
  * Massive scale.
  * On-demand access: Pay-as-you-go, no upfront commitment.
    * Anyone can access it.
  * Data-intensive Nature: What was MBs has now become TBs, PBs, and XBs.
    * Daily logs, forensics, Web data, etc.
    * Humans have data numbness: Wikipedia (large) compress is only about 10 GB!
  * New Cloud Programming Paradigms: MapReduce/Hadoop, NoSQL/Cassandra/MongoDB and many others.
    * High in accessibility and ease of programmability.
    * Lots of open-source

#### 3. New Aspects of Clouds

1. Massive Scale (easy to understand)
2. On-demand Access: *aaS Classification
   * HaaS: Hardware as a Service
     * You get access to barebones hardware machines, do whatever you want with them
     * Not always a good idea because of security risks
   * IaaS: Infrastructure as a Service
     * You get access to flexible computing and storage infrastructure. Virtualization is one way of achieving this. Often said to subsume HaaS.
   * PaaS: Platform as a Service
     * You get access to flexible computing and storage infrastructure, coupled with a software platform (often tightly)
   * SaaS: Software as a Service
     * You get access to software services, when you need them. Often said to subsume SOA (Service Oriented Architectures).
3. Data-intensive Computing
   * Computation-Intensive Computing
     * Example areas: MPI-based, High-performance computing, Grids
     * Typically run on supercomputers
   * Data-Intensive
     * Typically store data at datacenters
     * Use compute nodes nearby
     * Compute nodes run computation services
   * In data-intensive computing, the **focus shifts from computation to the data**: CPU utilization no longer the most important resource metric, instead I/O is (disk and/or network)
4. New Cloud Programming Paradigms
   * Easy to write and run highly parallel programs in new cloud programming paradigms

#### 4. Economics of Clouds

* Two categories of clouds
  * Can either be a public cloud, or private cloud
  * Private clouds are accessible only to company employees
  * Public clouds provide service to any paying customer
* To Outsource or Own?
