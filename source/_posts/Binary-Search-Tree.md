---
title: Binary Search Tree
date: 2021-03-11 12:14:36
tags: ['Data Structure', 'Algorithm']
categories: ['class notes']
cover:
summary: Binary Search Tree (Definition, Search, Insertion, Deletion)
img:
mathjax: true
---

#### 1. Ordered Maps

* Keys are assumed to come from a total order
* Items are stored in order by their keys
* This allows us to support **nearest neighbor queries**:
  * Item with largest key less than or equal to $k$
  * Item with smallest key greater than or equal to $k$

#### 2. Sorted Search Tables

* Store the map's entries in an array list $A$ so that they are in increasing order of their keys.

* We refer to this implementation as a ***sorted search table***.

* A search table is an ordered map implemented by means of a sorted sequence

  * We store the items in an array-based sequence, sorted by key
  * We use an external comparator for the keys

* Performance:

  * Searches take $O(\log n)$ time, using binary search
  * Inserting a new item takes $O(n)$ time, since in the worst case we have to shift $n$ items to make room for the new item
  * Removing an item takes $O(n)$ time, since in the worst case we have to shift $n$ items to compact the items after the removal

* The lookup table is efficient only for ordered maps of small size or for maps on which searches are the most common operations, while insertions and removals are rarely performed

* Performance of a sorted map (implemented using a sorted search table)

  * |                       Method                        |                      Running Time                      |
    | :-------------------------------------------------: | :----------------------------------------------------: |
    |                        size                         |                         $O(1)$                         |
    |                         get                         |                      $O(\log n)$                       |
    |                         put                         | $O(n)$; $O(\log n)$ if map has entry<br>with given key |
    |                       remove                        |                         $O(n)$                         |
    |                firstEntry, lastEntry                |                         $O(1)$                         |
    | ceilingEntry, floorEntry<br>lowerEntry, higherEntry |                      $O(\log n)$                       |
    |                       subMap                        |     $O(s+\log n)$ where $s$ items are<br>reported      |
    |              entrySet, keySet, values               |                         $O(n)$                         |

* Motivation

  * Binary search on ordered arrays is efficient: $O(\log_2 n)$
  * However insertion or removal os an item in an ordered array is slow: $O(n)$
  * Ordered arrays are best suited for static searching, where search space does not change
  * Binary search trees can be used for efficient dynamic searching

#### 3. Binary Search Trees

* A binary search tree is a *proper* binary tree storing keys (or key-value entries) at its *internal nodes* and satisfying the following properties:
  * Let $u, v$, and $w$ be three nodes such that $u$ is in the left *subtree* of $v$ and $w$ is in the right *subtree* of $v$. We have $key(u)\le key(v)\le key(w)$
  * Assuming there are no duplicate keys, we have $key(u)\lt key(v)\lt key(w)$
* External nodes do not store items
  * and likely are not actually implemented, but are just null links from the parent
* Search
  * To search for a key $k$, we trace a downward path starting at the root
  * The next node visited depends on the comparison of $k$ with the key of the current node
  * If we reach a leaf, the key is not found
* Insertion
  * Have to insert $k$ where a $get(k)$ would find it
  * So natural that $put(k, o)$ starts with $get(k)$
  * Search for key $k$ (using TreeSearch)
  * Assume $k$ is already in the tree then just replace the value
  * Otherwise, let $w$ be the leaf reached by the search, we insert $k$ at node $w$ and expand $w$ into an internal node
* Deletion
  * To perform operation $remove(k)$, we search for $k$
  * Assume key $k$ is in the tree, and let $v$ be the node storing $k$
  * If node $v$ has a leaf child $w$, we remove $v$ and $w$ from the tree with operation $removeExternal(w)$, which removes $w$ and its parent
  * We consider the case where the key $k$ to be removed is stored at a node $v$ whose children are both internal
    * we find the internal node $w$ that follows $v$ in an **inorder traversal**
    * we copy $key(w)$ into node $v$
    * we remove node $w$ and its left child $z$ (which must be a leaf) by means of operation $removeExternal(z)$
* Performance
  * Consider an ordered map with $n$ items implemented by means of a binary search tree of height $h$
    * the space used is $O(n)$
    * methods *get*, *put* and *remove* take $O(h)$ time
  * The height $h$ is $n$ in the worst case and $\log n$ in the best case

