---
title: Graph
date: 2021-04-10 13:13:44
tags: ['Data Structure', 'Algorithm']
categories: ['class notes']
cover:
summary: "Graph (traversal algorithms, Dijkstra's algorithm, Topological sort, Minimal spanning tree)"
img:
mathjax: true
---

#### 1. Definition: Graph

* $G$ is an ordered triple $G = (V, E, f)$
  * $V$ is a set of nodes, points, or vertices.
  * $E$ is a set, whose elements are known as edges or lines.
  * $f$ is a function maps each element of $E$ to an unordered pair of vertices in $V$

#### 2. Definition: vertex & edge

* Vertex
  * Basic element
  * Drawn as a ***node*** or a dot
  * Vertex set of $G$ is usually denoted by $V(G)$, or $V$

* Edge
  * A set of two elements
  * Drawn as a ***line*** connecting two vertices, called end vertices, or endpoints
  * The ***edge*** set of $G$ is usually denoted by $E(G)$, or $E$

#### 3. Simple graphs

* *Simple graphs* are graphs without ***multiple edges*** or ***self-loops***.

#### 4. Directed and undirected graphs

* Graphs can be
  * Undirected (edges don't have direction),
  * Directed (edges have direction).
* Undirected graphs can be represented as directed graphs where for each edge $(X, Y)$ there is a corresponding edge $(Y, X)$
* Directed edge $(A, B)$.
* Undirected edge $\{A, B\}$.

#### 5. Weighted and unweighted graphs

* Graphs can also be
  * unweighted
  * weighted (edges have weights)

#### 6. Adjacency, path & reachability

* $A\longrightarrow B \longrightarrow C \longrightarrow D$
* Node $B$ is ***adjacent*** to $A$ is there is an edge from $A$ to $B$.
* A ***path*** from $A$ to $D$: a sequence of vertices, such that there is an edge from $A$ to $B$, $B$ to $C$, $\cdots$, from $C$ to $D$.
* Vertex $B$ is ***reachable*** from $A$ is there is a path from $A$ to $B$.

#### 7. Cycle, acyclic & connected

* ***Cycle***: A path from a vertex to itself.
* Graph is ***acyclic*** if it does not have cycles.
* Graph is ***connected*** if there is a path between every pair of vertices.
* Graph is ***strongly connected*** if there is a path in ***both directions*** between every pair of vertices

#### 8. Degree

* Number of edges incident on a node.

#### 9. Degree (directed graphs)

* In-degree: Number of edges entering,
* Out-degree: Number of edges leaving,
* Degree = indeg + outdeg.

#### 10. Degree: simple facts

* If $G$ is a graph with $m$ edges, $\Sigma deg(v) = 2m = 2|E|$
* If $G$ is a digraph, $\Sigma indeg(v) = \Sigma outdeg(v) = |E|$
* The number of ***odd-degree nodes*** is even
* Let $G$ be a simple graph with $n$ vertices and $m$ edges
  * If $G$ is undirected, $m\le n(n-1)/2, C_n^{2}$
  * If $G$ is directed, $m\le n(n-1)$
* Let $G$ be an undirected graph with $n$ vertices and $m$ edges
  * If $G$ is connected, $m\ge n-1$
  * If $G$ is a tree, $m = n-1$

#### 11. Data structures for graphs

* Static implementation: adjacency matrix

  * Store node in the array: each node is associated with an integer (array index).

  * Represent information about the edges using a two-dimensional array, where
    $$
    array[i][j] = 1
    $$
    iff there is an edge from node with index $i$ to the node with index $j$.

  * Disadvantages of adjacency matrices

    * ***Sparse graohs***
      * Very few edges for a large number of vertices.
      * Result in many zero entries in adjacency matrix.
      * Waste space and makes many algorithms less efficient.
    * Matrix representation is ***inflexible***, if the number of nodes in the graph may change, (especially if we don't know the maximal size of the graph).

* Adjacency list

  * For every vertex, keep a list of adjacent vertices.
  * Keep a list of vertices, or keep vertices in a Map as keys and lists of adjacent vertices as values.

* Edge list

  * Represent the graph as a list of edges.

#### 12. Graph traversals

* Graph traversal: visit all vertices and edges in a graph in a systematic way.

  * Breadth-first search.
  * Depth-first search.

* What is it used for?

  * Search for a certain ***node*** in a graph.
  * Search for a certain ***edge*** in a graph.
  * Search for a ***path*** between two nodes.
  * Check if a graph is ***connected***.
  * Check if a graph contains ***loops***.

* Breadth-first search

  * ```pseudocode
    BFS starting from vertex v,
    Create a queue Q,
    Mark v as visited and put v into Q,
    While Q is non-empty,
      Remove the head u of Q,
      Mark and enqueue all (unvisited) neighbours of u.
    ```

  * The BFS order is that those closet to the start point $A$ occur earliest.

  * The BFS search tends to "go broad (wide, flat)".

* Depth-first search

  * ```pseudocode
    DFS starting from vertex v.
    Create a stack S.
    Mark v as visited and push v onto S.
    While S is non-empty,
    	Peek at the top u of S,
    	If u has an (unvisited) neighbour w,
    		mark w and push it onto S,
    	else
    		pop S.
    ```

  * The DFS search tends to "go deep".

* Detect cycles in a directed graph

* * Idea: If we encounter a vertex which is already on the *stack*, we find a loop

    * Stack contains vertices on a path, and if we see the same vertex again, the path must contain a cycle.

  * Instead of visited and unvisited, use three colors:

    * White = unvisited
    * Grey = on the stack
    * Black = finished

  * Modified DFS for cycle detection

    * ```pseudocode
      Modified DFS starting from v.
      All vertices coloured white.
      Create a stack S.
      Colour v grey and push v onto S.
      While S is non-empty
      	peek at the top u of S
      	if u has a grey neighbour,
      		a cycle is detected,
      	else if u has a white neighbour w,
      		colour w grey and push it onto S,
      	else
      		colour u black and pop S.
      ```

* $firstUnmarkedAdj()$

  * Adjacency list implementation.

  * Assume that we have a method which returns the first unmarked vertex adjacent to a given one:

    ```pseudocode
    GraphNode firstUnmarkedAdj(GraphNode v)
    ```

  * Implementation

    * Keep ***a pointer into the adjacency list*** of each vertex
      * Do not start to traverse the list of adjacent vertices from the beginning each time.
    * Or use the same iterator for this list, so when we call $next()$ it returns the next element in the list.
      * Again does not start from the beginning.

* Pseudocode for breadth-first search

  * ```pseudocode
    Starting from vertex s
    s.marked = true; // marked is a field in GraphNode
    Queue Q = new Queue(); //FIFO
    Q.enqueue(s); // add s into queue
    while (!Q.isempty()) {
    	v = Q.dequeue(); //remove the first element
    	u = firstUnmarkedAdj(v);
    	while (u != null) {
    		u.marked = true;
    		Q.enqueue(u);
    		u = firstUnmarkedAdj(v);
    	}
    }
    ```

  * Complexity of BFS

    * Assume an adjacency list representation, $|V|$ is the number of vertices, $|E|$ is the number of edges.
      * ***Each vertex*** is enqueued and dequeued at most once.
      * Scanning for all ***adjacent vertices*** takes $O(|E|)$ time, since sum of lengths of adjacency lists is $|E|$.
      * Gice a $O(|V|+|E|)$ time complexity.

* Pseudocode for DFS

  * ```pseudocode
    s.marked = true;
    Stack S = new Stack(); //FILO
    S.push(s); //push s into stack
    while (!S.isempty()) {
    	v = S.peek(); //topest element in stack
    	u = firstUnmarkedAdj(v);
    	if (u == null) {
    		S.pop(); //no element, remove
    	}else {
    		u.marked = true;
    		S.push(u); //push u into stack
    	}
    }
    ```

  * Complexity of DFS

    * ***Each vertex*** is pushed on the stack and popped at most once.
    * For every vertec we check what the next unvisited neighbour is.
    * In our implementation, we traverse the ***adjacency list*** only once. This gives $O(|V| + |E|)$ again.

* BFS vs. DFS

  * |                  |            BFS            |            DFS            |
    | :--------------: | :-----------------------: | :-----------------------: |
    |  Implementation  | Queue, size $|V|$<br>FIFO | Stack, size $|V|$<br>FILO |
    | Space complexity |         $O(|V|)$          |         $O(|V|)$          |
    | Time complexity  |       $O(|V|+|E|)$        |       $O(|V|+|E|)$        |
    |   Application    |       Shortest path       |      Cycle detection      |

#### 13. Dijkstra's algorithm

* An algorithm for solving the single-source shortest path problem.

  * ***Greedy*** algorithm
  * Compute ***distances*** from a source vertex to all others

* Assumptions:

  * The graph is ***connected***.
  * The edges are ***undirected***.
  * The edge weights are ***nonnegative***.

* Grow a "cloud" of vertices, beginning with $s$.

* Store $d(v)$ representing the distance of $v$ from $s$.

* At each step

  * Add to the cloud the vertex $u$ outside the cloud with the *shortest distance* $d(u)$.
  * *Update* the distances of the vertices *adjacent to $u$*.

* Edge relaxation

  * Consider an edge $e = (u, z)$ such that

    * $u$ is the vertex most recently added to the cloud
    * $z$ is not in the cloud

  * The relaxation of edge $e$ updates distance $d(z)$ as follows:

    $d(z)\leftarrow \min\{d(z), d(u) + weight(e)\}$

* Implementation

  To find the shortest paths from the start vertex $s$:

  * Priority queue PQ: vertices to be processed.
  * An array: Shortest distances from $s$ to every vertex.
    * Initially set to be infinity for all but $s$, and $0$ for $s$.
  * Order the queue by its distance. (shortest in front).
  * Loop while there are vertices in the queue PQ:
    * Dequeue a vertex $u$.
    * Update the distances by Edge Relaxation.
    * Re-order the queue.

* Modified Dijkstra's algorithm

  * To make Dijkstra's algorithm to **return the path itself**, not just the distance:

    * In addition to distances, ***maintain a path*** (list of vertices) for every vertex.

    * In the beginning, paths are empty.

    * When assigning $dist(s, v) = dist(s, u) + weight(u, v)$ also assign $path(v) = path(u)$.

    * When dequeuing a vertex, add in to its path.

* Complexity

  * Assume that the priority queue is implemented as a heap.
  * At each step (dequeuing a vertex $u$ and recomputing distances) we do $O(|E_u|\times \log (|V|))$ work, where $E_u$ is the set of edges with source $u$. (Up-heap, find the min).

  * We do this for every vertex, so total complexity is $O((|V|+|E|)\times \log(|V|))$.
  * Really similar to BFS and DFS, but instead of choosing some successor, we ***reorder*** a priority queue at each step, hence the $\log(|V|)$ factor.
  * Adaptable priority queue using unsorted sequence
    * Complexity: $O(|V|^2+|E|)$
  * Fibonacci heap
    * Complexity: $O(|V|\times \log(|V|) + |E|)$.

#### 14. Directed acyclic graphs (有向无环图)

* Informal definition: directed graph without directed cycles.

#### 15. Topological ordering

* Definition: A topological ordering of graph $G$ is an ordering of its $n$ vertices such that for every edge $(v_i, v_j)$ of $G$, it is the case that $i\lt j$.
  * Any directed path in $G$ traverses vertices in increasing order.
  * May be more than $1$ ordering.
* $G$ has a topological ordering iff it is acyclic.

#### 16. Topological sort

* Objective: sort vertices in order so that every edge folloes such an order.

* Input: directed acyclic graph.

* Output: a ***linear sequence of vertices*** such that for any two vertices $u$ and $v$, if there is an edge from $u$ to $v$, then $u$ is before $v$ in the sequence.

* Algorithm

  * ```pseudocode
    Create an empty array to store the ordering.
    While the graph is not emoty, repeat:
    	Find a vertex with no incoming edges.
    	Put this vertex in the array.
    	Delete the vertex from the graph.
    ```

  * Note that this destructively updates a graph

    * Make a copy of the graph first and do topological sort on the copy.

#### 17. Cycle detection with topological sort

* What happens if we run topological sort on a ***cyclic*** graph?
* There will be either no vertex with $0$ prerequisites to begin with, or at some point in the iteration.
* If we run a topological sort on a graph and there are vertices left undeleted, the graph contains a cycle.
* Why does topological sort work?
  * A vertex cannot be removed before all its prerequisites have been removed. (No incomming edges)
  * It cannot be inserted in the array before its prerequisite. (So the array is arranged in topological order).
  * Cycle detection: in a cycle, a vertex is its own prerequisite. So it can never be removed.

#### 18. Minimal spanning tree

* Input: ***connected, undirected, weighted*** graph

* Output: a tree which connects all vertices in the graph using only the edges present in the graph and is minimal in the sense that ***the sum of weights of the edges is the smallest*** possible.

* ***Spanning tree***: a tree contains all vertices of a connected graph.

* ***Minimal spanning tree***: The spanning tree with the smallest total weight.

* Prim's algorithm

  * A ***greedy*** algorithm to construct an MST:

  * ```pseudocode
    Pick any vertex M.
    Choose the shortest edge from M to any other vertex N.
    Add edge (M, N) to the MST.
    Continue to add at every step the shortest edge from a vertex in MST to a vertex outside, until all vertices are in MST.
    ```

#### 19. Euler path

* A path that uses every **edge** of a graph exactly once.
  * Start and end at **different** vertices.
* The Criterion for Euler Paths
  * The two endpoints of $P$ must be **odd** vertices.
  * All vertices other than the two endpoints of $P$ must be **even** vertices.
* Explanation Using Degree of Graph
  * A graph has Eular paths iff it has two **odd-degree** nodes, and all other nodes are **even-degree** nodes.

#### 20. Euler Circuit

* An Euler circuit is a **circuit** that uses every edge of a graph exactly once.
* An Euler circuit starts and ends at the **same** vertex.
* Explanation Using Degree of Graph
  * A connected graph has Euler circuits iff all nodes are **even-degree** node.
