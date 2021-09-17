---
title: Priority Queue
date: 2021-06-02 12:48:53
tags: ["Data Structure", 'Algorithm']
categories: ["class notes"]
cover: false
summary: "Priority Queue Introduction"
img: /medias/featureimages/20.jpg
mathjax: true
---

#### 1. Definition

* A priority queue is an abstract data type for storing a collection of prioritized elements that supports
  * arbitrary element insertion,
  * removal of elements in order of priority (the element with the first priority can be removed at any time)

#### 2. Priority Queue ADT

* A priority queue stores a collection of entries
* Each entry is a pair (key, value)
* **Entry ADT**
  * An *entry* in a priority queue is simply a key-value pair
  * Priority queues store entries to allow for efficient insertion and removal based on keys

#### 3. Sequence-based Priority Queue

* Implementation with an unsorted list
  * *insert* takes $O(1)$ time since we can insert the item at the beginning or end of the sequence
  * *removeMin* and *min* take $O(n)$ time since we have to traverse the entire sequence to find the smallest key
* Implementation with a sorted list
  * *insert* takes $O(n)$ time since we have to find the place where to insert the item
  * *removeMin* and *min* take $O(1)$ time, since the smallest key is at the beginning

#### 4. Priority Queue Sorting

* Use a priority queue to sort a list of comparable elements
  * Insert the elements one by one with a series of *insert* operations
  * Remove the elements in sorted order with a series of *removeMin* operations
* Selection-Sort
  * Selection-sort is the variation of PQ-sort where the prioroty queue is implemented with an unsorted sequence
  * Running time of Selection-sort:
    * Inserting the elements into the priority queue with $n$ insert operations takes $O(n)$ time
    * Removing the elements in sorted order from the priority queue with $n$ *removeMin* operations takes time proportional to $1+2+\cdots + n$
  * Selection-sort runs in $O(n^2)$ time
* Insertion-Sort
  * Insertion-sort is the variation of PQ-sort where the priority queue is implemented with a sorted sequence
  * Running time of Insertion-sort:
    * Inserting the elements into the priority queue with $n$ *insert* operations takes time proportional to $1+2+\cdots +n$
    * Removing the elements in sorted order from the priority queue with a series of $n$ *removeMin* operations takes $O(n)$ time
  * Insertion-sort runs in $O(n^2)$ time

