---
title: Pattern Matching
date: 2021-04-15 20:06:45
tags: ["模式匹配", "算法"]
categories: ["课堂笔记"]
cover:
summary: Pattern Matching
img:
mathjax: true
---

#### 1. What is pattern matching?

* Given:
  * A text string of length $n$
  * A pattern string of length $m\lt n$
* Determine:
  * Whether the pattern is a ***substring*** of the text;
  * Find all indices at which the pattern begins.

#### 2. Brute-force algorithm

* Compare the pattern $P$ with the text $T$ for each possible shift of $P$ relative to $T$, until either
  * A match is found, or
  * All placements of the pattern have been tried.
* Brute-force pattern matching runs in time $O(nm)$ in worst case.

#### 3. Boyer-Moore algorithm

* Boyer-Moore heuristics

  * ***Looking-glass heuristic***: Compare $P$ with a subsequence of $T$ from  ***right to left***.
  * ***Character-jump heuristic***: When a mismatch occurs at $T[i] = c$
    * If $P$ contains $c$, shift $P$ to align the ***last (right-most)*** occurrence of $c$ in $P$ with $T[i]$
    * Else, shift $P$ to align $P[0]$ with $T[i+1]$

* Last-occurrence function

  * Objective: speed up the search by ***look-up table***.
  * BM algorithm ***preprocesses the pattern $P$*** and the alphabet $S$ to build the last-occurrence function $L$ mapping $S$ to integers, where $L(c)$ is defined as
    * The largest index $i$ such that $P[i] = c$ or
    * $-1$ if no such index exists.

* The Boyer-Moore algorithm

  * ```pseudocode
    Algorithm BoyerMooreMatch(T, P, Alphabet)
    	L = lastOccurenceFunction(P, Alphabet)
    	i = m - 1
    	j = m - 1
    	repeat
    		if T[i] == P[j] // matching
    			if j == 0
    				return i // match at i
    			else
    				i = i - 1
    				j = j - 1
    		else // character-jump
    			l = L[T[i]]
    			i = i + m - min(j, 1 + l)
    			j = m - 1 //rightmost
    	until i > n - 1
    	return -1 // no match
    ```

* Analysis

  * Boyer-Moore's algorithm runs in time $O(nm)$ in worst case
  * The worst case may occur in images and DNA sequences but unlikely in English text.
  * Boyer-Moore's algorithm is ***significantly faster*** than the brute-force algorithm on English text.

* Key innovations of BM algorithm

  * ***Pre-indexing mismatched Text characters in Pattern.***

#### 4. KMP algorithm

* Key idea of KMP algorithm: precompute ***self-overlaps*** between portion of the pattern, so when a mismatch occurs, we know the maximum amount to shift the pattern.

* Failure function

  * Indicate the shift of the pattern upon a failed comparison.

    * $f(k)$: the length of ***the longest prefix*** of the pattern that is a suffix of the substring pattern$[1,2,\cdots,k]$. pattern$[0]$ is excluded as we shift at least one character.
    * Mismatch at pattern$[k+1]$, $f(k)$ tells how many preceding characters can be reused to restart the pattern.

  * ```pseudocode
    Algorithm failureFunction(P)
    	F[0] = 0
    	i = 1
    	j = 0
    	while i < m
    		if P[i] == P[j]
    			F[i] = j + 1
    			i = i + 1
    			j = j + 1
    		else if j > 0 then
    			j = F[j-1]
    		else
    			F[i] = 0 //no match
    			i = i + 1
    ```

* Time complexity

  * $O(|P|+|T|)$.

