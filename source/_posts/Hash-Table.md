---
title: Hash Table
date: 2021-04-11 10:33:24
tags: ['Data Structure', 'Algorithm']
categories: ['class notes']
cover:
summary: Hash Table (Hash Functions, Collision Handling)
img:
mathjax: true
---

#### 1. Definition

* **Hash tables** are a concrete data structure which is suitable for implementing maps.
* Basic idea: convert each key into an index.
* Look-up of keys, insertion and deletion in a hash table usually runs in $O(1)$ time.

#### 2. Hash Tables and Hash Functions

* A **hash table** for a given key type consists of
  * Array $A$ (called table) of size $N$
  * Hash function $h$
* A **hash function** $h$ maps keys of a given type to integers in a fixed interval $[0, N-1]$
* When implementing a map with a hash table, the goal is to store item $(k, o)$ at index $i = h(k)$ in the array $A$

#### 3. Hash Functions

* A hash function is usually specified as the *composition* of two function:
  * **Hash code**: $h_1: keys \rightarrow integers$
  * **Compression function**: $h_2: integers \rightarrow [0, N - 1]$
* The hash code is applied first, and the compression function is applied next on the result, i.e., $h(x) = h_2(h_1(x))$
* The goal of the hash function is to **"disperse"** the keys in an apparently **random** way

#### 4. Collision Handling

* Collisions occur when different elements are mapped to the same cell.
* **Separate Chaining**: let each cell in the table point to a list of entries  that map there
  * Separate chaining is simple, but requires additional mempry outside the table
* **Linear Probing**
  * **Open addressing**: the colliding item is placed in a different cell of the table
  * **Linear probing**: handles collisions by placing the colliding item in the next (circularly) available table cell
  * Accessing a cell of the array is referred to as a "probe"
  * Disadvantage: Colliding items lump together, causing future collisions to cause a longer sequence of probes
* Updates with Linear Probing
  * To handle insertions and deletions, we introduce a special object, called ***DEFUNCT***, which replaces deleted elements

#### 5. Double Hashing

* Double hashing uses a secondary hash function $d(k)$ and handles collisions by placing an item in the first available cell of the series
  $$
  (h(k) + jd(k))\mod N
  $$
  for $j = 0, 1, \cdots, N-1$

* The secondary hash function $d(k)$ cannot have zero values

* Linear probing is a special case where $d(k) = 1$

* The table size $N$ must be a *prime* to allow probing of all the cells

* Common choice of compression function for the secondary hash function:
  $$
  d(k) = q - (k\mod q)
  $$
  where

  * $q < N$
  * $q$ is a *prime*

* The possible values for $d(k)$ are $1, 2, \cdots, q$

#### 6. Performance of Hashing

* In the worst case, searches, insertions and removals on a hash table take $O(n)$ time
* The worst case occurs when all the keys inserted into the map collide
* The load factor $\alpha = n/N$ affects the performance of a hash table
* The expected running time of all the map ADT operations in a hash table is $O(1)$

#### 7. Summary

* Let $n$ denote the number of entries in the map, and we assume that the bucket array supporting the hash table is maintained such that its capacity is proportional to the number of entries in the map.

* <style type="text/css">
  .tg  {border-collapse:collapse;border-spacing:0;}
  .tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
    overflow:hidden;padding:10px 5px;word-break:normal;}
  .tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
    font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
  .tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
  .tg .tg-7btt{border-color:inherit;font-weight:bold;text-align:center;vertical-align:top}
  </style>
  <table class="tg">
  <thead align="center">
    <tr>
      <th class="tg-7btt" rowspan="2">Method</th>
      <th class="tg-7btt" rowspan="2">Unsorted<br>List</th>
      <th class="tg-7btt" colspan="2">Hash Table</th>
    </tr>
    <tr>
      <td class="tg-c3ow">expected</td>
      <td class="tg-c3ow">worst case</td>
    </tr>
  </thead>
  <tbody align="center">
    <tr>
      <td class="tg-c3ow">get</td>
      <td class="tg-c3ow">O(n)</td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(1)</span></td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(n)</span></td>
    </tr>
    <tr>
      <td class="tg-c3ow">put</td>
      <td class="tg-c3ow">O(n)</td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(1)</span></td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(n)</span></td>
    </tr>
    <tr>
      <td class="tg-c3ow">remove</td>
      <td class="tg-c3ow">O(n)</td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(1)</span></td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(n)</span></td>
    </tr>
    <tr>
      <td class="tg-c3ow">size, isEmpty</td>
      <td class="tg-c3ow">O(1)</td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(1)</span></td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(1)</span></td>
    </tr>
    <tr>
      <td class="tg-c3ow">entrySet, keySet, values</td>
      <td class="tg-c3ow">O(n)</td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(n)</span></td>
      <td class="tg-c3ow"><span style="font-weight:normal;font-style:normal;text-decoration:none">O(n)</span></td>
    </tr>
  </tbody>
  </table>

* Why disperse?

  * To reduce numbers of collisions

* Why random?

  * Random means no pattern
  * If there is an obvious pattern, then the incoming data may have a matching pattern that leads to many collisions.
