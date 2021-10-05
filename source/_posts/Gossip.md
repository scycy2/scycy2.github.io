---
title: Gossip
date: 2021-10-05 14:52:48
tags: ['Cloud Computing']
categories: ['self-study notes']
cover:
summary: Gossip Protocol (Multicast Problem)
img: /medias/featureimages/32.jpg
mathjax: true
---

#### 1. Multicast Problem

* Multicast

  <img src="Gossip/Screen Shot 2021-10-05 at 12.06.15 PM.png" style="zoom:50%;" />

* Fault-Tolerance and Scalability

  <img src="Gossip/Screen Shot 2021-10-05 at 12.12.36 PM.png" style="zoom:50%;" />

  * The protocal that is executed at the individual nodes, as well as the senders, what is known as the multicast protocol.
  * The multicast protocal typically sits at the application level, meaning that is does not deal with the underlying network. IP multicast is what's available in the underlying network, typically this is implemented in routers and switches.

* Centralized

  <img src="Gossip/Screen Shot 2021-10-05 at 12.18.26 PM.png" style="zoom:50%;" />

  * Problem 1: when it comes to fault tolerance, if the sender fails when it's halfway through its for loop, essentially, only half the receivers would have received the multicast.
  * Problem 2: the overhead on the sender is very high. Imagine a group where you have thousands of nodes in the group, and the sender has to send a lot of messages (increases the latency). The average time of for a reveiver to receive a multicast could be as high as $O(N)$.

* Tree-based

  <img src="Gossip/Screen Shot 2021-10-05 at 12.25.08 PM.png" style="zoom:50%;" />

  * Protocols develop a spanning tree among the nodes or the processes in the group.
  * The latency for a message to reach any of the nodes in the group is $O(\log (N))$.
  * The overhead on each member in the group, both the sender and any of the receivers, is constant especially if the number of children is constant.
  * Problem 1: set up and maintain the tree. When load failures happen, the nodes in the system might actually not receive multicast for a while. 
  * Use either acknowledgments (ACKs) or negative acknowledgments (NAKs) to repair multicasts not received.
  * SRM (Scalable Reliable Multicast)
    * Uses NAKs
    * But adds random delays, and uses exponential backoff to avoid NAK storms
  * RMTP (Reliable Multicast Transport Protocol)
    * Uses ACKs
    * But ACKs only sent to designated receivers, which then re-transmit missing multicasts
  * These protocols still cause an $O(N)$ ACK/NCK overhead

#### 2. The Gossip Protocol

