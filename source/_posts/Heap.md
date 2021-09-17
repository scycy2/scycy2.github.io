---
title: Heap
date: 2020-11-22 13:38:18
tags: ["Data Structure", "Algorithm"]
categories: ["class notes"]
cover:
summary: "Heap Introduction"
img: /medias/featureimages/12.jpg
mathjax: true
---



#### 1. Definition

* A heap is a binary tree storing keys at its nodes and satisfying the following properties:
  * **Heap-Order**: for every internal node $v$ other than the root, $key(v) \ge key(parent(v))$
  * **Complete Binary Tree**: let $h$ be the height of the heap
    * levels $i = 0, \cdots, h-1$ have the maximal number of nodes, i.e. there are $2^i$ nodes at depth $i$.
    * The remaining nodes at level/depth $h$ reside in the leftmost possible positions at that level/depth.
  * The **last node** of a heap is the rightmost node of maximum depth

#### 2. Insertion into a Heap

* Method insertItem of the priority queue ADT corresponds to the insertion of a key $k$ to the heap

* The insertion algorithm consists os three steps

  * Find the insertion node $z$ (the new last node)
  * Store $k$ at $z$
  * Restore the heap-order property

* **Upheap**

  * After the insertion of a new key $k$, the heap-order property may be violated

  * Algorithm up heap restores the heap-order property by swapping $k$ along an upward path from the insertion node

  * Upheap terminates when the key $k$ reaches the root or a node whose parent has a key smaller than or equal to $k$

  * Since a heap has height $O(\log n)$, up heap runs in $O(\log n)$ time

#### 3. Removal from a Heap

* Method removeMin of the priority queue ADT corresponds to the removal of the root key from the heap
* The removal algorithm consists of three steps
  * Replace the root key with the key of the last node $w$
  * Remove $w$
  * Restore the heap-order property
* **Downheap**
  * After replacing the root key with the key $k$ of the last node, the heap-order property may be violated
  * Algorithm downheap restores the heap-order property by swapping key $k$ along a downward path from the root (always swap with the smallest child)
  * Downheap terminates when key $k$ reaches a leaf or a node whose children have keys greater than or equal to $k$
  * Since a heap has height $O(\log n)$, downheap runs in $O(\log n)$ time

#### 4. Array-besed Heap Implementation

* We can represent a heap with $n$ keys by means of a vector or ArrayList of length $n+1$
* The cell of at index $0$ is not used
* Links between nodes are not explicitly stored
* For the node at index $i$
  * the left child is at index $2i$
  * the right child is at index $2i+1$
* Operation insert corresponds to inserting at index $n+1$
* Operation removeMin corresponds to moving index $n$ to index $1$

#### 5. Implementing Priority Queue with a Heap

* To create a priority queue, initialise a heap
* To insert in the priority queue, insert in the heap
* To get the value with the minimal key, ask for the value of the root of the heap
* To dequeue the highest priority item, remove the root and return the value stored there.

#### 6. Heap-Sort

* Using a heap-based priority queue, we can sort a sequence of $n$ elements in $O(n\log n)$ time
* The resulting algorithm is called heap-sort
* Heap-sort is much faster than quadratic sorting algorithms, such as insertion-sort and selection-sort
