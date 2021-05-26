---
title: Red-Black Tree
date: 2021-05-26 13:22:45
tags: ['数据结构','红黑树']
categories: '学习笔记'
cover: true
summary: "红黑树定义及简单操作"
img:
---

<h2 align='center'>Red-Black Trees</h2>

#### 1. Definition

* A red-black tree is a binary search tree that satisfies the following properties:
  * **Root Property**: the root is black
  * **External Property**: every leaf is black
  * **Internal/Red Property**: the children of a red node are black
  * **Depth Property**: all the leaves have the same *black depth*, defined as the number of proper ancestors that are black

#### 2. From Red-Black to $(2,4)$ Trees

* Given a red-black tree, we can construct a corresponding $(2,4)$ tree:
  * merge every red node $w$ into its parent, storing the entry from $w$ at its parent
  * the children of $w$ become ordered children of the parent
* Depth Property:
  * $(2,4)$ Tree: all the external nodes have the same depth
  * Red-Black Tree: all the leaves have the same *black depth*
* A red-black tree is a representation of a $(2,4)$ tree by means of a binary tree whose nodes are colored red or black
* In comparison with its associated $(2,4)$ tree, a red-black tree has
  * same logarithmic time performance
  * simpler implementation with a single node type

#### 3. Height of a Red-Black Tree

* Theorem: A red-black tree storing $n$ items has height $O(\log n)$
  * Proof:
    * The height of a red-black tree is at most twice the height of its associated $(2,4)$ tree, which is $O(\log n)$

#### 4. Search

* The search algorithm for a red-black tree is the same as that for a binary search tree
* Searching in a red-black tree takes $O(\log n)$ time

#### 5. Insertion

* To insert $(k, o)$, we execute the insertion algorithm for binary search trees and color **red** the newly inserted node $z$ *unless it is the root*

  * We preserve the root, external, and depth properties
  * If the parent $v$ of $z$ is black, we also preserve the internal property and we are done
  * Else ($v$ is red) we have a **double red** (i.e., a violation of the internal property), which requires a reorganization of the tree

* **Remedying a Double Red**

  * Consider a double red with child $z$ and parent $v$, and let $w$ be the sibling of $v$
  * Case1: $w$ is black
    * The double red is an incorrect replacement of a 4-node
    * **Restructuring**: we change the 4-node replacement
  * Case2: $w$ is red
    * The double red corresponds to an overflow
    * **Recoloring**: we perform the equivalent of a **split**

* **Restructuring**

  * A restructuring remedies a child-parent double red when the parent red node has a black sibling

  * It is equivalent to restoring the *correct replacement* of a 4-node

  * The internal property is restored and the other properties are preserved

  * There are four restructuring configrations depending on whether the double red nodes are left or right children

    <img src="Red-Black-Tree/Screen Shot 2021-05-10 at 7.22.25 PM.png" style="zoom:50%;" />

* **Recoloring**

  * A recoloring remedies a child-parent double red when the parent red node has a red sibling
  * The parent $v$ and its sibling $w$ become black and the grandparent $u$ becomes red, unless it is the root
  * It is equivalent to performing a split on a 5-node
  * *The double red violation may propagate to the grangparent $u$*

* Analysis of Insertion

  * ```pseudocode
    Algorithm insert(k, o)
    1. We search for key k to locate the insertion node z
    2. We add the new entry (k, o) at node z and color z red
    3. while doubleRed(z)
    		if isBlack(sibling(parent(z)))
    			z <- restructure(z)
    			return
    		else sibling(parent(z)) is red
    			z <- recolor(z)
    ```

  * Recall that a red-black tree has $O(\log n)$ height

  * Step 1 takes $O(\log n)$ time because we visit $O(\log n)$ nodes

  * Step 2 takes $O(1)$ time

  * Step 3 takes $O(\log n)$ time because we perform

    * $O(\log n)$ recoloring, each taking $O(1)$ time, and
    * at most one restructuring taking $O(1)$ time

  * Thus, an insertion in a red-black tree takes $O(\log n)$ time

#### 6. Deletion

* To perform operation $remove(k)$, we first execute the deletion algorithm for binary search trees
* Let $v$ be the internal node removed, $w$ the external node removed, and $r$ the sibling of $w$
  * If $v$ was red, the resulting tree remains a valid red-black tree
  * If $v$ was black and $r$ was red, we color $r$ black and we are done
  * Else ($v$ and $r$ were both black) we color $r$ ***double black***, to preserve the depth property
* **Remedying a Double Black**
  * The algorithm for remedying a double black node $r$ with sibling $y$ considers three cases
  * Case1: $y$ is black and has a red child
    * We perform a **restructuring**, equivalent to a **transfer**, and we are done.
  * Case2: sibling $y$ of $r$ is black and its children are both black
    * We perform a **recoloring**, equivalent to a **fusion**, which may propagate up the double black violation
  * Case3: $y$ is red
    * We perform an **adjustment**, equivalent to choosing a different representation of a 3-node, after which either Case 1 or Case 2 applies
* Deletion in a red-black tree takes $O(\log n)$ time
