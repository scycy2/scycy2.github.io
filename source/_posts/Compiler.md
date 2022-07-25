---
title: Compiler
date: 2021-10-02 18:35:06
tags: ['Compiler', 'Continuously updated']
categories: ['class notes']
cover: true
summary: "Compilers: Principles, Techniques and Tools"
img: /medias/featureimages/31.jpg
mathjax: true
---

### 1. Introduction

#### 1.1 What is a Compiler?

* Compilers are **program translators**:

  <img src="Compiler/Screen Shot 2021-09-23 at 10.43.33 AM.png" style="zoom:50%;" />

* To make it easier for human beings to write programs.

#### 1.2 Source and Target Languages

* Large spectrum of possibilities, for example:
  * Source languages:
    * (High-level) programming languages
    * Modelling languages
    * Document Markup languages
    * Database query languages
  * Target languages:
    * High-level programming language
    * Low-level programming language (assembly or machine code, byte code)

#### 1.3 Compilers vs. Interpreters

**Intepreters** are another class of translators:

* Compiler: translates a program once for all into target language.
* Interpreter: effectively translates (the used parts of) a source program, on the fly, every time it is run.
* Techniques such as **Just-In-Time Compilation** (JIT) blur this distinction.
* Compilers and interpreters are sometimes used together, e.g., in Java:
  * Java is first complied into Java byte code;
  * Then, the byte code is interpreted by a Java Virtual Machine (JVM).
  * JVM might use JIT.

#### 1.4 Inside the Compiler

* A compiler is usually composed of several components, such as:
  * **Scanner**: lexical analysis
  * **Parser**: syntactic analysis
  * **Checker**: contextual analysis (e.g. type checking)
  * **Optimizer**: code improvement
  * **Code generator**

* <img src="Compiler/Screen Shot 2021-09-23 at 11.01.40 AM.png" style="zoom:50%;" />

* The aim of lexical analysis is to:
  * verify that the input character sequence is lexically valid.
  * group characters into sequence of lexical symbols, i.e., **tokens**.
  * discard white space and comments (typically).
* The purpose of syntactic analysis/parsing is to:
  * verify that the input program is syntactically valid, i.e., conforms to the **Context Free Grammer** of the language.
  * determine the program structure.
  * construct a representation of the program reflecting that structure without unnecessary details, usually an **Abstract Syntax Tree** (AST).
* Contexttual Analysis/Checking Static Semantics:
  * Resolve meaning of symbols.
  * Report undefined symbols.
  * Type checking.
* Optimization:
  * Making the generated code run faster, use less space, energy, etc.
  * Almost always **heuristic**: cannot **guarantee** optimal result.
* Code Generation:
  * Output the appropriate sequence of target language instructions.
  * Might involve further **low-level** (target-specific) optimization.

### 2. Defining Programming Languages

#### 2.1 Syntax and Semantics

* Notions of **Syntax** and **Semantics** are central to languages.
* In fact, they have their origins in mathematical logic.

##### 2.1.1 Syntax: the form of programs

* **Concrete Syntax** (or Surface Syntax): What program "look like"
  * Usually **strings** of characters or symbols.
  * Some languages have *graphical* syntax
* **Abstract Syntax**: A **tree** representing the essential structure of a syntactically valid program.

##### 2.1.2 Semantics: the meaning of a program

* **Static Semantics**:
  * Refers to the meaning of (a fragment of) a program, as may be infered at **compile-time**.
  * Typically, aspects related to scope and type.
* **Dynamic Semantics**:
  * What (a fragment of) a program means when executed.
  * Hence, the meaning as referred at **run-time**, rather than compile-time.
  * For example, what a fragment of a program:
    * *evaluates* to, if it is an expression.
    * *does*, if it is a set of instructions.

#### 2.2 Defining Programming Languages

##### 2.2.1

* In order to develop a compiler:
  * the **Source** and **Target** languages must be defined
    * syntax
    * semantics
* In theory, to avoid inaccuracies, language definitions must be fully formalized.
* In practice, language **specifications** are presented using a mixture of **formal** or **informal** descriptions, to make it easier for human readers.

##### 2.2.2

Why is it important to define the source and target languages precisely?

* The **source language syntax** must be known to design the **scanner** and **parser** properly
* The **target language syntax** must be known to generate **syntactically correct target code**
* The **semantics** of source and target languages must be known to ensure that the translation **preserves the meaning** of source programs, i.e., **compiler correctness**.
  * **compiler correctness is of utmost importance**.
  * For instance, it is not acceptable to sacrifice compiler correctness to optimize the generated code.

#### 2.3 Object Language and Metalanguage

* In any language definition, informal or formal, a careful distinction must be made between
  * the **Object Language**: the language being defined:
    * Java, Haskell, Assembly, etc.
  * the **Metalanguage**: the language of the definition it self.
    * English, for informal specifications.
    * BNF, or context-free grammars (CFGs) in general, for formal specifications.
* The semantics of the metalanguage must be well understood.
  * e.g., how a context-free language (CFL) is generated from a CFG, how the production rules are used, parsing issues, etc.

#### 2.4 Informal Specifications

* In an informal specification, the metalanguage is a natural language, e.g., English.
* Most programming languages are defined more or less informally.
* 'Informal' does not mean imprecise: it is possible to be precise also in a natural language.

#### 2.5 Formal Specifications

* A **Formal Specification** is mathematically precise
* Usually, a **Formal Metalanguage** is used, e.g.
  * **ENBF** for specifying context-free syntax
  * **inference rules and logic** for specifying static and/or dynamic semantics
  * **denotational semantics** for specifying dynamic semantics
* Languages with rich denotational semantics are ideal where:
  * absolute correctness is essential, e.g., in safety-critical systems.
  * high efficiency is required, e.g., energy efficient computation.
* Compared with imperative languages (e.g., C or Java), functional languages (e.g., Lisp pr Haskell) have richer denotational semantics.

#### 2.6 Context-Free Grammers

**Context-free grammars** generate **context-free languages**:

* CFLs are suitable for specifying some common programming language constructs, e.g.
  * balanced parentheses
  * nested structures
  * matching keywords such as **begin** and **end**.
* In language design, CFLs that can be recognized by **deterministic pushdown automata (DPDAs)** are preferred over non-deterministic ones.

#### 2.7 CFG Notation

We will give CFGs by stating the productions in one of the two styles:

* **Formal languages style**

  * Used for: small, abstract examples; at meta level when talking about grammars
  * Simple naming conventions used to distinguish terminals and variables (i.e., nonterminals):
    * variables: uppercase letters, e.g., *A*, *B*, *S*
    * terminals: lowercase letters or digits, e.g., *a*, *b*, 3
  * Start symbol usually called *S*

* **Programming Language Specification style**:

  * Used for larger and more applied examples.

  * Typographical conventions used to distinguish terminals and variables:

    * Variables are written like *this*
    * Terminals are written like <font color=blue>this</font>
    * Terminals with variable spelling and special symbols are written like <u>*<font color=blue>this</font>*</u>

  * The start symbol is often implied by the context.

  * For example:

    *AssignStmt -> <u><font color=blue>Identifier</font></u>* <font color=blue>:=</font> *Expr*

    Here,

    * *AssignStme* and *Expr* are nonterminals
    * <font color=blue>:=</font> is a terminal
    * <u>*<font color=blue>Identifier</font>*</u> is also a terminal, but its possible spellings are defined elsewhere, usually by a lexical (ie, regular) grammar.

#### 2.8 BNF and Extended BNF

The previous CFGs were essentially in **Backus-Naur form (BNF)**.

**Extended Backus-Naur form (EBNF)** is a bit more convenient.

* Additional EBNF constructs:
  * parentheses for grouping
  * | for alternatives within parentheses
  * \* for iteration
* EBNF is **no more powerful** than BNF:
  * any EBNF grammar can be transformed into an equivalent BNF.
* EBNF is more concise and readable than plain BNF.

#### 2.9 Concrete Syntax

The **Concrete Syntax** (a. k. a. surface syntax) of a language is usually defined at two levels:

* The **Lexical syntax**: usually a **regular language**, with grammar rules written as regular expressions, specifying the syntax of:
  * **language symbols** or **tokens**
  * **white space**
  * **comments**
* The **Context-Free syntax**

#### 2.10 MiniTriangle Lexical Syntax

<img src="Compiler/Screen Shot 2021-10-12 at 10.28.53 PM.png" style="zoom:50%;" />

* Essentially a (left-)linear grammar; i. e., the lexical syntax specifies a **regular** language.

* Not completely formal (e. g., the use of "except" for excluding keywords from identifiers).

* Each individual character of a terminal is actually a terminal symbol, i. e., it should be:

  <img src="Compiler/Screen Shot 2021-10-12 at 10.32.04 PM.png" style="zoom:50%;" />

* Special characters are written like <u>*<font color=blue>this</font>*</u>. They are single terminal symbols.

#### 2.11 MiniTriangle Tokens

Some valid MiniTriangle tokens:

* const3 (Identifier)
* const (Keyword)
* 42 (Integer Literal)
* \+ (Operator)

#### 2.11 MiniTriangle: Maximal Munch Principle

**Question**: Is const3 a single token?

**Answer**: It is hard to tell.

* The grammar is ambiguous.
* This type of ambiguity exists in the grammar of almost all commonly used programming languages.
* The ambiguity is handled by using the *longest match* - a.k.a., maximal munch - principle
* Essentially, the tokens must be built from the maximum possible number of characters from the input stream.

#### 2.12 MiniTriangle Context-Free Syntax

<img src="Compiler/Screen Shot 2021-10-19 at 3.36.56 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-10-19 at 3.37.03 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-10-19 at 3.37.09 PM.png" style="zoom:50%;" />

#### 2.12 Why a Lexical Grammar?

Together, the lexical grammar and the context-free grammar specify the concrete syntax.

In our case, both grammars are expressed in (E)BNF and look similar.

* Why not join them?
* Why not do away with scanning, and just do parsing?

Answer:

* **Simplicity**:
  * Dealing with white space and comments in the context free grammar becomes unnecessarily complicated
  * In constrast, the lexical grammar manages tokenization by using regular grammars.
* **Efficiency**:
  * Working on classified groups of characters (tokens) facilitates parsing, i.e., makes it possible to use a simpler parsing algorithm.
  * Grouping and classifying characters by simple means increases efficiency.

#### 2.13 MiniTriangle Abstract Syntax

This grammar specifies the **phrase structure** of MiniTriangle. In addition, it gives node labels to be used when drawing Abstract Syntax Trees.

<img src="Compiler/Screen Shot 2021-10-20 at 2.57.47 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-10-20 at 2.57.52 PM.png" style="zoom:50%;" />

**Notes:**

* Keywords and other fixed-spelling terminals serve only to make the connection with the concrete syntex clear.
* <font color='blue'><u>*Identifier*</u></font> $\cup$ <font color='blue'><u>*Operator*</u></font> $\subseteq$ <font color='blue'><u>*Name*</u></font>

<img src="Compiler/Screen Shot 2021-10-20 at 3.08.18 PM.png" style="zoom:50%;" />

Note: **fixed-spelling** terminals are **omitted** because they are implied by the node labels.

#### 2.14 Concrete vs. Abstract Syntax

Key points:

* Concrete syntax: **string** (generated from the lexical and context-free grammars)
* Abstract syntax: **tree**

(Ways to describe **graphical** concrete syntax are more varied)

#### 2.17 Concrete AST Representation

Algebraic datatypes (as in Haskell) allow a direct way to represent the abstract syntax:

* Each non-terminal is mapped to a **type**
* Each label is mapped to a **constructor** for the corresponding type
* The constructors get one argument for each non-terminal and "variable" terminal in the RHS of the production
* Sequences are represented by lists
* Options are represented by values of type $Maybe$
* "Literal" terminals are ignored

<img src="Compiler/Screen Shot 2021-10-20 at 3.24.08 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-10-20 at 3.24.13 PM.png" style="zoom:50%;" />

One may use labeled fields. It is also possible to include source position for error reporting to facilitate debugging:

<img src="Compiler/Screen Shot 2021-10-20 at 3.24.19 PM.png" style="zoom:50%;" />

### 3. Syntactic Analysis: Bottom-Up Parsing

#### 3.1 Parsing Strategies

* There are two basic strategies for parsing:
  1. ***Top-down parsing***:
     * Attempts to construct the parse tree *from the root downward*.
     * Traces out a ***leftmost derivation***.
  2. ***Bottom-up parsing***:
     * Attempts to construct the parse tree *from the leaves working up toward the root*.
     * Traces out a ***rightmost derivation in reverse***.

#### 3.2 Top-Down: Leftmost Derivation

 Consider the grammar:

<img src="Compiler/Screen Shot 2021-10-20 at 4.26.37 PM.png" style="zoom:50%;" />

Call sequence for predictive parser on *abccde*:

<img src="Compiler/Screen Shot 2021-10-20 at 4.27.32 PM.png" style="zoom:50%;" />

#### 3.3 Bottom-Up Parsing

* In practice, bottom-up parsers are more common, because they are more efficient.
* The machinery behind them, however, is significantly more complicated than that of top-down parsers.
* Thus, it is advisable to use **parse generator tools**, instead of coding parsers by hand in conventional programming languages.
* Using these specialized tools makes the process a lot simpler, **yet it is important to acquire some basic knowledge of the operations involved in bottom-up parsing.**

#### 3.4 Shift-Reduce Parsing

There are many different approaches to bottom-up parsing, but most implementations are based on **shift-reduce parsing**.

A shift-reduce parser:

* performs a single left-to-right pass over the input without any backtracking.
* works from leaves toward the root of the parse tree.
* has two basic actions:
  * ***Shift*** (read) next terminal symbol
  * ***Reduce*** a sequence of terminals (that have been read already) and previously reduced nonterminals, corresponding to the right-hand side (RHS) of a production, to left-hand side (LHS) nonterminal of that production.

#### 3.5 Bottom-Up: A Simple Example

* Consider the context-free grammar (CFG) $G$ with productions:

  <img src="Compiler/Screen Shot 2021-10-21 at 1.50.09 PM.png" style="zoom:50%;" />

  for simple arithmetic expressions, with:

  * a single terminal '*a*'
  * two operations + and *
  * all ending in $
    * (This makes the example easier to handle.)

* The aim is to design a pushdown automaton (PDA) $NB(G)$ that accepts the language $L(G)$, effectively performing a bottom-up parsing, and producing rightmost derivations in reverse.

* In the nondeterministic bottom-up PDA $NB(G)$, the two types of moves, other than the initial move and the moves to accept, are shifts and reductions.

* There are five possible reductions in the PDA $NB(G)$, two requiring only one move and three requiring two or more.

* Nondeterminism arises in two ways: first, in choosing whether to shift or to reduce, and second, in making a choice of reductions if more than one is possible.

* There is only one correct move at each step, and $G$ turns out to be a grammar for which the combination of the input symbol and stack symbol will allow us to find the correct move.

* One basic principle is that, if there is a reduction that should be made, then it should be made as soon as it is possible.

* For example, if '$a$' is on top of the stack, then a reduction, not a shift, is the move to make.

  * Any symbol that goes onto the stack, will have to be removed later in order to execute the reduction of '$a$' or $T*a$ to $T$.
  * The only way to remove it is to do another reduction
  * and because the derivation tree is built from the bottom up, any subsequent reduction to construct the portion of the tree involving the '*a*' node needs its parent, not the node itself.

* Another principle that is helpful is that, in the moves made by $NB(G)$, the current string in the derivation being simulated contains the reverse of the stack contents (not including $Z_0$) followed by the remaining input.

* We can use this to answer the question of which reduction to execute if there seems to be a choice:

  * If $a * T$ (the reverse of $T {*} a$) were on top of the stack, and we simply reduced $a$ to $T$, then the current string in a rightmost derivation would contain the substring $T*T$, which is never possible.
  * Similarly, if $T*S_1$ (the reverse of $S_1*T$) were on top, then reducing $T$ to $S_1$ cannot be correct.

* These two situations can summarized by saying that **when a reduction is executed, the longest possible string should be removed from the stack during the reduction.**

* Essentially, the only remaining question is what to do when $T$ is the top stack symbol: shift or reduce?

* We can answer this by considering the possibilities for the next input symbol.

  * If it is $+$, then this $+$ should eventually be part of the expression $S_1 + T$ (in reverse) on the stack, so that $S_1 + T$ can be reduced to $S_1$; therefore, the non-terminal $T$ should eventually be reduced to $S_1$, and so the time to do it is now.
  * Similarly, if the next input is ${$}$, then the non-terminal $T$ should be reduced to $S_1$, so that $S_1{$}$ can be reduced to $S$.
  * The only other symbol that can follow $T$ in a rightmost derivation is $*$, and $*$ cannot follow any other symbol; therefore, $*$ should be shifted onto the stack.

With these observations, we can formulate the rules that the deterministic bottom-up PDA should follow in choosing its moves:

1. If the top stack symbol is $Z_0$, $S_1$, $+$, or $*$, shift the next input symbol to the stack. (None of these four symbols is the rightmost symbol in the right side of a production.)
2. If the top stack symbol is ${$}$, reduce $S_1{$}$ to $S$.
3. If the top stack symbol is '$a$', reduce $T*a$ to $T$ if possible; otherwise reduce $a$ to $T$.
4. If the top stack symbol is $T$, then reduce (to $S_1$) if the next input is $+$ or \$, otherwise shift.
5. If the top stack symbol is $S$, then pop it from the stack; if $Z_0$ is the next top symbol, accept, otherwise reject.

* All these rules, except rule 4, are easily incorporated into a transition table for a deterministic pushdown automaton (DPDA), but the fourth may require a little clarification:

  * If the PDA sees the combination ($*$, $T$) of next input symbol and stack symbol, then it shifts $*$ onto the stack.
  * If it sees ($+$, $T$) or (${$}$, $T$), then the moves it makes are the ones that carry out the appropriate reduction, and then shift the input symbol onto the stack.

* The point is that "seeing" ($+$, $T$), for example, implies reading the $+$, and the PDA then uses auxiliary states to remember that after it has performed a reduction, it should place $+$ on the stack.

* We now have the essential specifications for a DPDA.

* Like the non-deterministic PDA $NB(G)$, the moves it makes in accepting a string simulate, **in reverse**, the steps in a **rightmost derivation** of that string.

* and in this sense, our DPDA serves as a bottom-up parser for $G$.

  <img src="Compiler/Screen Shot 2021-10-25 at 2.59.01 PM.png" style="zoom:50%;" />

* The moves for the DPDA and the input string $a+a*a{$}$ are shown in the table above.

* For each configuration, we show the rule (from the set of five rules) that the PDA used to get there, and, in the case of rule 4, whether the move was a shift or a reduction.

* As mentioned before, the details of the shift-reduce theory are quite involved.

* The previous example serves as a relatively simpler starting point for obtaining a general understanding of the method.

* In what follows, we will discuss some details that are helpful in understanding the general concepts.

* This will make it easier to understand how to use parser generators.

#### 3.6 Bottom-Up: Rightmost Derivation in Reverse

Consider (again) the grammar:

<img src="Compiler/Screen Shot 2021-10-20 at 4.26.37 PM.png" style="zoom:50%;" />

The reduction steps for the sentence $abccde$ to $S$:

<img src="Compiler/Screen Shot 2021-10-25 at 3.11.18 PM.png" style="zoom:50%;" />

trace out a rightmost derivation in reverse:

<img src="Compiler/Screen Shot 2021-10-25 at 3.11.49 PM.png" style="zoom:50%;" />

How can we know when and what to reduce?

**Idea**:

* Construct a deterministic finite automaton (DFA) in which:
  * **each state is labeled by "all possibilities"**, considering the input and reductions thus far.
* A bit similar to the subset construction that we had for converting a non-deterministic finite automaton (NFA) to an equivalent DFA.
* Whenever reduction is possible,
  * if there is only one possible reduction, then it is clear what to do.

#### 3.7 LL, LR, and LALR parsing

Three important classes of parsing methods:

* ***LL(k):***
  * input scanned left to right
  * Leftmost derivation
  * $k$ symbols of lookahead
* ***LR(k):***
  * input scanned left to right
  * Rightmost derivation in reverse
  * $k$ symbols of lookahead
* ***LALR(k):*** Look Ahead ***LR***, simplified LR parsing

By extension, the classes of grammars these methods can handle are also classified as LL($k$), LR($k$), and LALR($k$).

#### 3.8 Why study LR and LALR parsing?

* These methods handle a wide class of grammars of **practical significance**.
* In particular, **left- and right-recursive grammars**
  * but left-recursive needs smaller stack.
* LALR is a good compromise between **expressiveness and space cost** of implementation.
* Consequently, **many parser generator tools are based on LALR**.
* We will mainly study LR(0) parsing because:
  * it is the simplest,
  * yet, it used the same fundamental principles as LR(1) and LALR(1).

#### 3.9 Shift-Reduce Parsing Theory

Terminology:

An **item** for a CFG is a production with a dot anywhere in the RHS.

For example, the items for the grammar

<img src="Compiler/Screen Shot 2021-10-25 at 3.32.33 PM.png" style="zoom:50%;" />

are

<img src="Compiler/Screen Shot 2021-10-25 at 3.33.05 PM.png" style="zoom:50%;" />

* Given a CFG $G=(N,T,P,A)$, a string $\phi \in (N\cup T)^*$ is a **sentantial form** for $G$ iff $S\mathop{\stackrel*\Rightarrow}_{G} \phi$.

* A **right-sentential form** is a sentential form that can be derived by a **rightmost derivation**.

* A **handle** of a right-sentential form $\phi = \delta \alpha w$ is the substring $\alpha$ such that:

  <img src="Compiler/Screen Shot 2021-10-26 at 1.49.12 PM.png" style="zoom:50%;" />

  where $\alpha, \delta, \phi \in (N\cup T)^*$, and $w\in T^*$.

  * The grammar must have the production $A\rightarrow \alpha$.

For example, consider the grammar:

<img src="Compiler/Screen Shot 2021-10-20 at 4.26.37 PM.png" style="zoom:50%;" />

The following is a rightmost derivation:

<img src="Compiler/Screen Shot 2021-10-26 at 1.53.15 PM.png" style="zoom:50%;" />

* $aABe$, $aAde$ and $abcAde$ are right-sentential forms.
* Handle for each? $aABe$, $d$, and $bcA$, respectively.

For an unambiguous grammar, the rightmost derivation is unique. Thus, we can talk about *"the handle"* rather than merely "a handle".

* *Finding handles is a central issue in bottom-up parsing.*

* Efficient handle finding is the key to efficient bottom-up parsing.

* A **viable prefix** of a right-sentential form $\phi$ is any **prefix $\gamma$** of $\phi$ ending **no farther** right than the right end of the handle of $\phi$.

* An item $A\rightarrow \alpha · \beta$ is **valid** for a viable prefix $\gamma$ if there is a rightmost derivation

  <img src="Compiler/Screen Shot 2021-10-26 at 2.00.45 PM.png" style="zoom:50%;" />

  and $\delta \alpha = \gamma$.

* An item in **complete** if the **dot is the rightmost** symbol in the item.

* The right-sentential form $abcAde$ has handle $bcA$.

* Viabel prefixes? $\epsilon, a, ab, abc, abcA$.

Last derivation step <img src="Compiler/Screen Shot 2021-10-26 at 2.07.33 PM.png" style="zoom:50%;" /> by production $A\rightarrow bcA$, indicating that the handle is $bcA$.

<img src="Compiler/Screen Shot 2021-10-26 at 2.08.34 PM.png" style="zoom:50%;" />

Knowing the valid items for a viable prefix allows us to find a rightmost derivation in reverse:

* If $A\rightarrow \alpha·$ is a **complete** valid item for a viable prefix $\gamma = \delta \alpha$ of a right-sentential form $\gamma w$, then it **appears** that $A\rightarrow \alpha$ can be used at the last step, and that the previous right-sentential form is $\delta A w$.
* If this is **always the case** for a CFG $G$, then for any $x\in L(G)$, since $x$ is a right-sentential form, previous right-sentential forms can be determined until $S$ is reached, giving a right-most derivation of $x$.

If $A\rightarrow \alpha·$ is a complete valid item for a viable prefix $\gamma = \delta \alpha$, we only know that it may be possible to use $A\rightarrow \alpha$ to derive $\gamma w$ from $\delta Aw$.

For example:

* $A\rightarrow \alpha·$ may be valid because of a **different** rightmost derivation <img src="Compiler/Screen Shot 2021-10-26 at 2.18.08 PM.png" style="zoom:50%;" />

* There could be **two or more complete items** valid for $\gamma$.
* There could be a handle of $\gamma w$ that **includes symbols of $w$**.

#### 3.10 LR(0) Parsing

* **LR(0) grammar:**

  A CFG for which knowing a complete valid item is enough to determine the previous right-sentential form.

* The set of viable prefixes for any CFG is **regular**!

  (A bit unexpected: the language of a CFG is not regular in general.)

* We can develop an **efficient parser** for the LR(0) CFG based on a **DFA for recognizing viable prefixes and their valid items**.

* The **states of the DFA** are **sets of items** valid for a recognized viable prefix.

A DFA recognizing viable prefixes for thr CFG

<img src="Compiler/Screen Shot 2021-10-20 at 4.26.37 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-10-26 at 2.30.28 PM.png" style="zoom:50%;" />

Drawing conventions for "LR DFAs":

* For the purpose of recognizing the set of viable prefixes, all drawn states are considered accepting.
* Error transitions and error states are not drawn.

Recall that:

* the **viable prefixes** for the right-sentential form $abcAde$ are $\epsilon, a, ab, abc, abcA$. They are indeed all recognized by the DFA (all states are considered accepting).
* the item $A\rightarrow bc·A$ is valid for the viable prefix $abc$. The corresponding DFA state indeed contains that item. (Along with ***more items*** in this case!)
* item $A\rightarrow bcA·$ is a **complete** valid item for the viable prefix $abcA$. The corresponding DFA state indeed contains that item (and ***only*** that item).

Given a DFA recognizing viable prefixes, an LR(0) parser can be constructed as follows:

* In a state *without complete items*: ***Shift***
  * Read the next terminal symbol and push it onto an internal parse stack.
  * Move to new state by following the edge labeled by the terminal symbol just read.
* In a state with a *single complete item*: ***Reduce***
  * The top of the parse stack contains the ***handle*** of the current right-sentential form (since we have recognized a viable prefix for which a single ***complete*** item is valid).
  * The handle is just the ***RHS*** of the valid item.
  * Reduce to the previous right-sentential form by ***replacing the handle*** on the parse stack with the ***LHS*** of the valid item.
  * ***Move*** to the state indicated by the new viable prefix on the parse stack.
* If a state contains both complete and incomplete items, or if a state contains more than one complete item, then the grammar ***is not LR(0)***.

Complete sequence ($\gamma w$ is right-sentential form):

<img src="Compiler/Screen Shot 2021-10-26 at 4.57.23 PM.png" style="zoom:50%;" />

Cf: <img src="Compiler/Screen Shot 2021-10-26 at 5.04.41 PM.png" style="zoom:50%;" />

To see more clearly that the parser carries out the rightmost derivation in reverse, look at the right-sentential forms $\gamma w$ of the **reduction steps only**:

<img src="Compiler/Screen Shot 2021-10-26 at 5.06.10 PM.png" style="zoom:50%;" />

#### 3.11 LR Parsing & Left/Right Recursion

Consider parsing of strings such as:

<img src="Compiler/Screen Shot 2021-10-26 at 5.16.18 PM.png" style="zoom:50%;" />

* Note how the ***right-recursive*** production $A\rightarrow bcA$ causes symbols $bc$ to pile up on the parse stack until a reduction by $A\rightarrow c$ can occur, which, in turn, allows the stacked symbols to be reduced away.
* ***Left-recursive*** allows reduction to happen sooner, thus keeping the size of the parse stack down.
* This is why left-recursive grammars are often preferred for LR parsing.

#### 3.12 LR(1) Grammars

* In practice, LR(0) tends to be a bit too restrictive.
* If we add one symbol of "lookahead" by determining the set of ***terminals that could possibly follow a handle*** being reduced by a production $A\rightarrow \beta$, then a wider class of grammars can be handled.
* Such grammars are called ***LR(1)***.

Idea:

* Associate a ***lookahead set*** with items:
  $$
  A \rightarrow \alpha · \beta, \{a_1, a_2, \cdots, a_n\}
  $$

* On reduction, a complete item is ***only valid*** if the next input symbol belongs to its lookahead set.

* Thus, it is OK to have two or more simultaneously valid complete items, as long as their lookahead sets are ***disjoint***.

### 4. Syntactic Analysis: Parser Generators

#### 4.1 Parser Genereators

* Constructing parsers by hand can be very tedious and time consuming.

* This is true in particular for LR(*k*) and LALR parsers: constructing the corresponding deterministic finite automata (DFAs) is extremely laborious.

* For instance, this simple grammar:

  <img src="Compiler/Screen Shot 2021-10-20 at 4.26.37 PM.png" style="zoom:50%;" />

  gives rise to a 10 state LR(0) DFA.

* **Parser construction** is, for the most part, an **algorithmic** process.

* Why not write a program to do the heavy and tedious work for us?

* A **Parser Generator** (or "compiler compiler") takes a grammar as input and outputs a parser (i.e., a program) for that grammar.

* The input grammar is augmented with **"semantic actions"**, i.e., code fragments that get invoked when a derivation step is performed.

* The semantic actions typically:

  * construct an abstract syntax tree (AST) or
  * interpret the program being parsed.

Consider an LR shift-reduce parser:

* A **reduction** corresponds to a derivation step in the grammar:
  * Remember that an LR parser performs a rightmost derivation in reverse.
* At a reduction, the terminals and non-terminals of the right-hand side (RHS) of the production (the **handle**) are on the parse stack, associated with:
  * **semantic information**, e. g., the correspoding AST fragments, or
  * **semantic values**, e. g., expression values.
* Both construction of an AST and evaluation of expressions proceed in **bottom-up** order.

#### 4.2 Happy Parser for TXL

* In order to impart associativity, a **left-recursive** grammar is provided.
  * A grammar $G$ is left-recursive if and only if there exists a nonterminal symbol $A$ that can derive to a sentential form with itself as the leftmost symbol, i. e., $A\rightarrow^+ A\alpha$ in which $\alpha$ is any sequence of terminal and nonterminal symbols.
  * The dual definition provides right-recursive grammars.
* LR parsers have no problem with left- or right-recursion, except that right-recursion requires more stack.

We are going to use Happy to develop a parser for the TXL language, generated by the CFG:

<img src="Compiler/Screen Shot 2021-11-10 at 7.57.32 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-11-10 at 7.57.59 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-11-10 at 7.58.21 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-11-10 at 7.58.42 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-11-10 at 7.59.02 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-11-10 at 7.59.22 PM.png" style="zoom:50%;" />

* The code fragment between curly braces is a Haskell **pattern** that is matched against the actual tokens returned by the parsing function.
* If this pattern contains the special variable $$, then the corresponding part of the matched token becomes the semantic value.
* Otherwise the entire token becomes the semantic value.
* The semantic values of different terminal symbols may thus have different types.

The grammar productions are written in BNF, with an additional semantic action defining the semantic value for each production:

<img src="Compiler/Screen Shot 2021-11-10 at 8.03.40 PM.png" style="zoom:50%;" />

It is also possible to add type annotations:

<img src="Compiler/Screen Shot 2021-11-10 at 8.04.23 PM.png" style="zoom:50%;" />

#### 4.3 Shift/Red. and Red./Red. Conflicts

In LR-parsing, ambiguous grammars lead to **shift/reduce** and **reduce/reduce** conflicts:

* shift/reduce: some states of the DFA have mixed complete and incomplete items:
  $$
  A\rightarrow a·\\
  A\rightarrow a·b
  $$

* reduce/reduce: some states have more than one complete item:
  $$
  A\rightarrow a·\\
  B\rightarrow a·
  $$

* Shift/reduce conflicts are often resolved by **opting for shifting**:

  * Typically the default option.
  * Usually gives the desired result. For instance, it resolves the dangling else problem in a natural way.

* Reduce/reduce conflicts are not so easy, because there is no obvious reason for picking one production over another: the grammar **must** be manually disambiguated.

#### 4.4 Precedence and Associativity

Happy (like Yacc and Bison) allows operator precedence and associativity to be explicitly specified to disambiguate a grammar:

<img src="Compiler/Screen Shot 2021-11-10 at 8.14.10 PM.png" style="zoom:50%;" />

The relative precedence is implicit in the ordering: lower precedences precede higher precedences. For instance, '+' and '-' appear before '*' and '/'.

#### 4.5 A TXL Interpreter

* The semantic actions do not have to construct an AST.

* An alternative is to **interpret** the code being parsed.

* Basic idea:

  <img src="Compiler/Screen Shot 2021-11-10 at 8.31.06 PM.png" style="zoom:50%;" />

* But TXL has a **let**-construct

  * What about TXL **variables**? For instance, how should we reflect the assignment of $3$ to $x$ in the following expression?

    <img src="Compiler/Screen Shot 2021-11-10 at 8.32.49 PM.png" style="zoom:50%;" />

One option:

* Each semantic action returns a function of type

  <img src="Compiler/Screen Shot 2021-11-10 at 8.33.37 PM.png" style="zoom:50%;" />

​		where (for example)

​																<img src="Compiler/Screen Shot 2021-11-10 at 8.34.07 PM.png" style="zoom:50%;" />

* The semantic action for evaluating a composite expression passes on the environment. For instance, the semantic action for + could be:

  <img src="Compiler/Screen Shot 2021-11-10 at 8.35.45 PM.png" style="zoom:50%;" />

* The semantic action for a variable looks up the variable value in the environment:

  <img src="Compiler/Screen Shot 2021-11-10 at 8.36.32 PM.png" style="zoom:50%;" />

* The semantic action for $let$ extends the argument environment and then evaluates the body in the extended environment:

  <img src="Compiler/Screen Shot 2021-11-10 at 8.37.34 PM.png" style="zoom:50%;" />

* The semantic action for literal just returns the value of the literal, ignoring the environment:

  <img src="Compiler/Screen Shot 2021-11-10 at 8.38.19 PM.png" style="zoom:50%;" />

* A program get evaluated by applying the overall result function to the **empty environment**:

  <img src="Compiler/Screen Shot 2021-11-10 at 8.39.05 PM.png" style="zoom:50%;" />

### 5. Contextual Analysis: Scope 1

#### 5.1 Contextual Analysis

The aim of contextual analysis is to ensure that a program is **statically well-formed**.

* But syntax is also related to "form".
* So, why is it not possible to perform contextual analysis using the context-free grammar (CFG) that generates the context-free syntax?
* For instance, is it possible to use CFGs to express *type constraints*?
  * Note that, if we could do this, then the parser would be able to do the type checking for us as well.

#### 5.2 Limitations of CFGs

Let us see if we can enforce the "declare before use" requirement using a CFG.

If we have only **one** variable **a**:

<img src="Compiler/Screen Shot 2021-12-22 at 2.17.09 PM.png" style="zoom:50%;" />

Now, let us see how the same approach may be generalized to **two** variables, **a** and **b**:

<img src="Compiler/Screen Shot 2021-12-22 at 2.18.05 PM.png" style="zoom:50%;" />

Some observations:

* Already for two variables, the grammar is a lot more complicated.
* If we extend the same approach to *n* variables, then how many "*ExprXYZ*" rules will be needed?
  * The number of nonterminals grows exponentially: for a set of $n$ variables $V = \{a_i\mid 1\le i \le n\}$, we need $2^n$ nonterminals $Expr[W]$, one for each subset $W\subseteq V$.
  * Moreover, normally, the number of variables is **unlimited**, which would imply **infinitely** many productions.
    * This will no longer be a CFG.

Attempt to describe simple type constraints using a CFG:

<img src="Compiler/Screen Shot 2021-12-22 at 2.24.16 PM.png" style="zoom:50%;" />

This might look reasonable at first sight. **However**:

* The scheme hinges on partitioning the variables **by name** into two groups, i. e.:
  * integer variables and boolean variables.
* But in most languages the type of a variable is given by the **context**, not its name.
* Furthermore, how could we, in general, infer argument types from the name of a procedure or function?

In simple terms, **context-free** grammars are not powerful enough to capture **context-sensitive** information.

#### 5.3 Unrestricted Grammars

* These previous arguments and examples do not form a mathematical **proof** that it is impossible to check static semantics using CFGs.

* For a clear proof, using the pumping lemma for context-free languages (CFLs).

* Contextual constraints result in **context sensitive**, or even **recursively enumerable**, languages:

  * Recall the Chomsky hierarchy.
  * Such languages cannot be described by CFGs.

* **Unrestricted grammars** with productions
  $$
  \alpha \rightarrow \beta,
  $$
  where $\alpha$ and $\beta$ are both **arbitrary strings**, could be used to express arbitrary contextual constraints.

* Unrestricted grammars are, however, equivalent to Turing Machines in their expressive power.

* Thus, to check contextual constraints, we must use Turing machines.

* Simpler machines such as pushdown automata (PDAs) are not powerful enough.

#### 5.4 Expressing Contextual Constraints

Turing Machines are abstract models and Unrestricted Grammars are not very practical. Therefore, we consider the following approach:

* Specifying contextual constraints:
  * Informally: using natural language.
  * Formally: using a mathematical formalism such as logical inference rules.
* Implementing contextual checks:
  * General purpose programming languages.
  * Direct support of mathematical formalism, unifying specification and implementation.

#### 5.5 Contexual Analysis

Two important contextual constraints on which we will focus in this course:

* **Scope rules**: where in a program a declaration is valid.
* **Type rules**: ensuring that every expression computes a value of acceptable form, i. e., has a valid type.

Corresponding subphrases of the contextual analysis:

* **Identification** or **Name Resolution**: applying the scope rules in order to relate each applied identifier occurrence to its declaration.
* **Type checking**: applying the type rules to infer the type of each expression, and compare it with the expected type.

Note that, there exist other kinds of contextual constraints in common programming languages.

For instance, Java has rules concerning:

* **Abstract class**; e. g.:
  * Only abstract classes may have abstract methods.
  * Abstract classes may not be instantiated.
* **Final classes**; e. g.:
  * a final class cannot be extended.
  * a class cannot be both final and abstract.
* **Exceptions**; e. g., the set of exceptions that a method can raise must be declared (except for unchecked exceptions).
* **Definite assignment**: Each local variable and every blank final field must have a definitely assigned value when any access of its value occurs.

#### 5.6 Identification

**Identification** (or **Name Resolution**) is the task of relating each **applied** identifier occurrence to its **declaration**.

<img src="Compiler/Screen Shot 2021-12-22 at 2.57.02 PM.png" style="zoom:50%;" />

In the body of set, the applied occurrence of:

* $x$ refers to the **instance variable** $x$.
* $n$ refers to the **argument** $n$, not the instance variable $n$.

#### 5.7 Scope and Scope Rules

The identification process is governed by the **scope rules** of the language.

Important terms:

* **Scope**: the section of a program over which a declaration takes effect.
* **Block**: a program phrase that delimits the scope of declarations within it.

Consider the MiniTriangle $let$ block command: $let$ *decls* $in$ *body*

The scope of each declaration is the rest of the block.

For example:

<img src="Compiler/Screen Shot 2021-12-22 at 3.02.05 PM.png" style="zoom:50%;" />

In contrast with MiniTriangle, in Haskell's let-expression: $let$ *id = expr* $in$ *body*

the scope of *id* includes both *expr* and *body*.

For example:

<img src="Compiler/Screen Shot 2021-12-22 at 3.10.09 PM.png" style="zoom:50%;" />

In addition to deciding the range of declarations, the scope rules also deal with issues such as:

* whether explicit declarations are required;
* whether multiple declarations at the same level are allowed;
* whether shadowing/hiding is allowed.

Let us now consider a version of Mini-Triangle extended with procedures and functions.

* The scope of a declared entity is extended to include the bodies of **all** procedures and functions declared in the same let-block.
* This allows procedures and functions to be (mutually) recursive.
* However, definition/initialization expressions for constants/variables must not use functions defined in the same let-block.
  * This avoids calling function that may refer to as-yet uninitialized variables.

With the above scope rule, it is possible to write programs such as:

<img src="Compiler/Screen Shot 2021-12-23 at 8.19.49 PM.png" style="zoom:50%;" />

#### 5.8 Some Java Scope Rules

From the Java Language Specification ver. 1.0:

* The scope of a member declared in, or inherited by, a class type or interface type, is the *entire* declaration of the class or interface type.
* The declaration of a member needs to appear before it is used only when the use is in a field initialization.
* The scope of a parameter of a method is the entire body of the method.
* Hiding the name of a local variable is not permitted.

#### 5.9 Symbol Table

A **symbol table**, also called **identification table** or **environment**, is used during identification to keep track of **symbols** and their **attributes**, such as:

* kind of symbol (class name, local variable, etc.)
* scope level
* type
* source code position

#### 5.10 Block Structure

The organization of the symbol table depends on the source language's **block structure**:

* **Monolithic block structure**: one common, global scope.
* **Flat block strucrure**: blocks with local scope enclosed in a global scope.
* **Nested block structure**: blocks can be nested to arbitrary depth.

We focus on **nested block structure** because:

* Nested block structure is by far the most common in modern high-level languages.
* Monolithic and flat block structures may be considered special cases of nested block structure.

#### 5.11 Using the Symbol Table

For a simple language with:

1. a declare-before-use rule and
2. redeclarations not allowed,

during identification, the symbol table would be used as follows:

* Initialize the table; e. g., enter the standard environment.
* When a declaration is encountered:
  * check if declared identifier clashes with existing symbol;
  * if it does, then report error;
  * if not, then enter the declared identifier into the table, along with its attributes.
* When an applied identifier occurrence is encountered:
  * look up the identifier in the table, taking scope rules into account;
  * if the identifier is not found, then report error;
  * if found, then annotate the applied occurrence with symbol attributes from the table.

Before identification:

<img src="Compiler/Screen Shot 2021-12-23 at 8.38.19 PM.png" style="zoom:50%;" />

After identification:

<img src="Compiler/Screen Shot 2021-12-23 at 8.38.50 PM.png" style="zoom:50%;" />

(Textual representation of annotated abstract syntax tree (AST).)

Suppose variables have to be declared, and that redeclarations are not allowed.

<img src="Compiler/Screen Shot 2021-12-23 at 8.41.00 PM.png" style="zoom:50%;" />

During symbol table insert and lookup it would be discovered that:

* $x$ is declared twice at the same scope level,
* $y$ is not declared at all.



* When entering a new block, arrange so that symbols that are entered subsequently become associated with the scope corresponding to the block (**"open scope"**).
* When leaving a block, remove/make inaccessible symbols declared in that block (**"close scope"**).

<img src="Compiler/Screen Shot 2021-12-23 at 8.45.31 PM.png" style="zoom:50%;" />

* A new scope is opened for the inner let-block (on line 3) when it is analyzed.
* When the inner let-block has been analyzed, its scope is closed.
* It is then discovered that $y$ (at the end of line 3) is no longer in scope. ($x$ is, however, still in scope.)

#### 5.12 Summary

* Contextual analysis includes checking scope rules and types.
* Contextual constraints lead to **context-sensitive** languages and thus cannot be captured by a context-free grammar.
* **Identification** is the task of relating each **applied** identifier occurrence to its declaration. This is a key step for any contextual analysis.
* The **Symbol Table** or **Environment** records information about declared entities and is the central data structure during contextual analysis.

### 6. Types and Type Systems

#### 6.1 Types and Type Systems

Type systems that we study in this course are an example of **lightweight formal methods**, in that they are:

* highly automated
* but with limited expressive power.

Note that:

* The aim of **static checking** is to prove **absence** of certain errors.
* Done by **classifying** syntactic phrases (or **terms**) according to the **kinds** of values they compute:
  * A type system computes a **static approximation** of the run-time behavior.

* Example:
  * If we know that two program fragments $exp_1$ and $exp_2$ compute integers (**classification**), then it is safe to add those numbers together (**absence of errors**)
  * We also know that the result is an integer.
  * While we do not know what the values are, we know for sure that they are integers and nothing else (**static approximation**).
* **Dynamically typed** languages do not have a type system according to this definition.
  * Perhaps, **'dynamically checked'** is a more appropriate phrase for describing these languages.

A type system is necessarily **conservative**, in the sense that, some well-behaved programs will be rejected.

* For example, typically:

  <img src="Compiler/Screen Shot 2021-12-23 at 9.48.26 PM.png" style="zoom:50%;" />

  will be rejected as ill-typed, even if we know that complex_test always evaluates to true.

* The reason is that, in general, these tests cannot be automatically proved.

A type system checks for **certain** kinds of bad program behavior (**run-time errors**). Exactly which depends on the type system and the language design.

For example: current main-stream type systems typically:

* **do check** that arithmetic operations are done only on numbers;
* **do not check** that the second operand of division is not zero, or that array indices are within bounds.

The **safety** or **soundness** of a type system must be judged with respect to its own set of run-time errors.

#### 6.2 Language Safety

* Programming languages typically make a distiction between normal program actions and erroneous actions.
* For Turing-complete languages (such as C, Java, Haskell, Python, etc.) it is impossible to decide (statically) whether a program will end up in an error or not. In fact, even simpler problems-e. g., checking whether a program terminates or not-may not be decidable
* The only option is to run the code and see.
* In a safe programming language, errors are **trapped** as they happen.
* Java, for example, is largely safe via its exception system.
* In an **unsafe** programming language, errors are not trapped.
* After executing an erroneous operation, the computation continues, but in a silent and faulty way that may have severe consequences.
* C and C++ are unsafe in a strong sense:
  * executing an erroneous operation causes the entire program to be meaningless,
  * as opposed to just the erroneous operation having an unpredictable result.
* In these languages, erroneous operations are said to have undefined behavior.
* Language safety is **not** the same as static typing:
  * Safety can be achieved through static typing **and/or** dynamic run-time checks.
  * There are languages that are dynamically checked and safe, e. g., **Lisp**.
* A statically typed language may also perform dynamic checks, e. g.:
  * checking of array bounds
  * down-casting (e. g., Java)
  * checking for division by zero
  * pattern-matching failure

Some examples of statically and dynamically checked safe and unsafe high-level languages:

<img src="Compiler/Screen Shot 2021-12-23 at 10.08.48 PM.png" style="zoom:50%;" />

#### 6.3 Advantages of Typing

* **Detecting errors earlier**: Programs in richly typed languages often "just work", for the following reasons:
  * Many of the simple and common mistakes arise from type inconsistencies.
  * Programmers are forced to think a bit more before coding.
* **Enforcing disciplined programming**: Type systems form the backbone of:
  * Modules
  * Classes
* **Documentation**:
  * Unlike comments, type signatures will always remain current.
* **Efficiency**:
  * First use of types in computing was to distinguish between integer and floating point numbers.
  * This led to the elimination of many of the dynamic checks that otherwise would have been needed to guarantee safety.

#### 6.4 Disadvantages of Typing

* Type systems sometimes do get in the way:
  * Simple concepts may require complicated coding if one has to work around the type system, e. g., heterogeneous lists in Haskell.
  * Sometimes it becomes impossible to do what one wants to do, at least not without loss of efficiency.
* Increasingly sophisticated type systems can help. But that can make the type systems harder to understand and less automatic.

#### 6.5 Static and Dynamic Semantics

In summary:

* A type system **statically** proves properties about the **dynamic** behavior of a program.
* To make precise exactly what these properties are, and formally **prove** that a type system achieves its goals, both of the following must be formalized first:
  * **static semantics**,
  * **dynamic semantics**.

#### 6.6 Example Language: Abstract Syntax

Example language. (Will be extended later.)

<img src="Compiler/Screen Shot 2021-12-24 at 12.46.56 PM.png" style="zoom:50%;" />

#### 6.7 Values

* The **values** of a language are a subset of the terms that are **possible results of evaluation**.
* In other words, values are the **meaning** of terms according to the **dynamic semantics** of the language.
* A term is **reduced** using a given set of evaluation rules until a value is obtained, at which point no further reduction is possible.
  * For instance, **pred(succ(0))** may be reduced to **0**, at which point, no further reduction is possible.
* In general, a term to which no (further) evaluation rule applies is said to be in **normal form**.
* All values are in normal form.

<img src="Compiler/Screen Shot 2021-12-24 at 12.52.24 PM.png" style="zoom:50%;" />

#### 6.8 One Step Evaluation Relation

$t\rightarrow t'$ is an **evaluation relation** on terms.

**Read**: $t$ evaluates to $t'$ in one step.

The evaluation relation constitutes an **operational** (dynamic) **semantics** for the example language.

<img src="Compiler/Screen Shot 2021-12-24 at 12.55.09 PM.png" style="zoom:50%;" />

* Evaluation Relation

  * Recall that a **relation** between two sets $X$ and $Y$ is just a subset of $X\times Y$.

  * For example, the "less than or equal" relation $\le$ on $N$ is a subset of $N\times N$, and we have:

    $\{(1,1),(1,2),(1,3),(2,2),(2,3)\} \subseteq (\le)$

  * The "one step evaluation" is a relation on **terms**. **One term is related to another iff the first evaluates to the second in one step**.

  * For example:

    <img src="Compiler/Screen Shot 2021-12-24 at 1.11.41 PM.png" style="zoom:50%;" />

  * The evaluation relation is infinite. Hence, we cannot enumerate all pairs.

  * Instead, (schematic) **inference rules** are used to specify relations:

    <img src="Compiler/Screen Shot 2021-12-24 at 1.13.50 PM.png" style="zoom:50%;" />

  * If there are no premises, the line is often omitted:

    <img src="Compiler/Screen Shot 2021-12-24 at 1.14.56 PM.png" style="zoom:50%;" />

  * **Schematic** means that universally quantified variables are allowed in the rules.

  * For example, the following holds **for all** $t_2$ and $t_3$:

  * <img src="Compiler/Screen Shot 2021-12-24 at 1.20.09 PM.png" style="zoom:50%;" />

  * In other words, such a **rule schema** actually stands for an **infinite set** of rules:

  * <img src="Compiler/Screen Shot 2021-12-24 at 1.21.07 PM.png" style="zoom:50%;" />

  * The **domain** of a variable is often specified by **naming conventions**.

  * For example, the name of a variable may indicate some specific **syntactic category** such as $t$ (for terms), $v$ (for values), or $nv$ (for numeric values):

  * <img src="Compiler/Screen Shot 2021-12-24 at 1.22.48 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-12-24 at 1.35.35 PM.png" style="zoom:50%;" />

**Note:** We have the rule $\text{pred}\ 0 \rightarrow 0$ instead of $\text{pred}\ 0$ reducing to $-1$ because the domain of our numeric values is the set of natural numbers $N = \{0,1,2,\cdots\}$.

<img src="Compiler/Screen Shot 2021-12-24 at 1.37.47 PM.png" style="zoom:50%;" />

#### 6.9 Values and Stuck Terms

Note that:

* **Values** cannot be evaluated further, e. g.:
  * **true**
  * $0$
  * $\text{succ}(\text{succ}\ 0)$
* Certain "obviously nonsensical" states result in the computation process **getting stuck**: the term cannot be evaluated further, but it is not a value.
* Definition: A term is **stuck** if it is a normal form but not a value.
* Why stuck?
  * The program is **not well-defined** according to the dynamic semantics of the language.
  * The **abstractions** of the language are being **broken**.

#### 6.10 Stuckness and Run-Time Errors

* The notion of **getting stuck** models **run-time errors**.
* The **goal** of a type system is to **guarantee** that a program **never gets stuck** in this manner.

#### 6.11 Why Should We Care About Safety?

* One reason: security.
* C and C++ are unsafe: **buffer overruns** are possible.
* Buffer overruns allow input data to be executed as code.
* This is one of the most common security holes. In other words, had a safe variant of C been used, one might speculate that billions of dollars would have been saved.

#### 6.12 Types

At this point, there are only two **types**, booleans and the natural numbers:

<img src="Compiler/Screen Shot 2021-12-26 at 9.32.56 PM.png" style="zoom:50%;" />

#### 6.13 Typing Relation

We will define a **typing relation** between terms and types:

$t:T$

Read:

$t$ has type $T$

* A term that has a type, i. e., is related to a type by such a typing relation, is said to be **well-typed**.
* The typing relation will be defined by (schematic) typing rules, in the same way as we defined the evaluation relation.

#### 6.14 Typing Rules

<img src="Compiler/Screen Shot 2021-12-26 at 9.35.40 PM.png" style="zoom:50%;" />

#### 6.15 Safety = Progress + Preservation

* An important reason for having a type system is **safety**.
* In other words, **"well typed programs do not go wrong"**, where "wrong" means entering a "stuck state".

This breaks down into two parts:

**Progress**: A well-typed term is not stuck.

**Preservation**: If a well-typed term is evaluated one step, then the resulting term is also well-typed (and has the same type).

Together, these two properties say that a well-typed term can never reach a stuck state during evaluation.

<img src="Compiler/Screen Shot 2021-12-26 at 9.39.47 PM.png" style="zoom:50%;" />

#### 6.16 Exceptions

What about terms such as the following that are (usually) considered well-typed?

* division by zero
* head of empty list
* array indexing out of bounds (buffer overrun)

If the type system does not rule them out, we need to differentiate those from stuck terms, or we can no longer claim that "well-typed programs do not go wrong".

**Idea**: Allow **exceptions** to be raised, and make it **well-defined** what happens when exceptions are raised.

For example:

* introduce a term **error**

* introduce evaluation rules like

  <img src="Compiler/Screen Shot 2021-12-26 at 9.47.29 PM.png" style="zoom:50%;" />

* typing rule: $error: T$

* Introduce **propagation rules** to ensure that the **entire** program evaluates to **error** once the exception has been raised, unless there is some exception handling mechanism.

* Change the Progress theorem slightly to allow for exceptions:

  <img src="Compiler/Screen Shot 2021-12-26 at 9.49.42 PM.png" style="zoom:50%;" />

#### 6.17 Extension: Let-bound Variables

Syntactic extension:

<img src="Compiler/Screen Shot 2021-12-28 at 9.53.13 PM.png" style="zoom:50%;" />

New evaluation rules:

<img src="Compiler/Screen Shot 2021-12-28 at 9.53.43 PM.png" style="zoom:50%;" />

* We now need a **typing context** or **type environment** to keep track of types of variables.

* This is an abstract version of a "symbol table"/

* The typing relation thus becomes a **ternary relation**:

  <img src="Compiler/Screen Shot 2021-12-28 at 10.10.18 PM.png" style="zoom:50%;" />

  **Read:** term $t$ has type $T$ in type environment $\Gamma$.

Environment-related notation:

* Extending an environment:
  $$
  \Gamma, x : T
  $$
  The new declaration is understood to replace any earlier declaration for a variable with the same name.

* Stating that the type of a variable is given by an environment:

  <img src="Compiler/Screen Shot 2021-12-28 at 10.13.13 PM.png" style="zoom:50%;" />

Updating typing rules:

<img src="Compiler/Screen Shot 2021-12-28 at 10.13.43 PM.png" style="zoom:50%;" />

<img src="Compiler/Screen Shot 2021-12-28 at 10.15.23 PM.png" style="zoom:50%;" />

New typing rules:

<img src="Compiler/Screen Shot 2021-12-28 at 10.15.47 PM.png" style="zoom:50%;" />

Recursive bindings:

* Typing is straightforward if the recursively-defined entity is **explicitly** typed:

  <img src="Compiler/Screen Shot 2021-12-28 at 10.17.04 PM.png" style="zoom:50%;" />

* If the recursively-defined entity is **not explicitly** typed, then the question is whether $T_1$ is uniquely defined (and in a type checker how to compute this type):

  <img src="Compiler/Screen Shot 2021-12-28 at 10.19.21 PM.png" style="zoom:50%;" />

#### 6.18 Introduction to $\lambda$-Calculus

* Imperative languages (such as C, Java, and Python) are based on Turing machine model of computation.

* In imperative languages, there is a clear distinction between *programs* and *data*:

  **Programs**: sequences of instructions that are executed sequentially;

  **Data**: values given as input, stored in memory, manipulated during computation, and returned as output.

* Programs and data are distinct and are kept separated.

* Programs are not modified during computation.

  * Note that, in principle, it is possible to modify programs during computation, since they are stored in memory like any other data.
  * This approach is, however, very difficult to manage. Hence, it is generally avoided.

* In contrast, functional languages (such as Haskell and Lisp) are based on the $\lambda$-calculus model of computation.

* In functional programming, there is no distiction between programs and data.

  * Both are represented by terms/expressions belonging to the same language.
  * Computation consists in the *reduction* of terms to *normal forms*, including terms that represent functions, i. e., the programs themselves.

#### 6.19 Syntax of $\lambda$-calculus

* Lambda calculus is a pure theory of functions, i. e., all the objects are functions.
  * The objects of $\lambda$-calculus are represented as $\lambda$-terms.
  * $\lambda$-terms represent both data structures and programs.
* As $\lambda$-calculus is a pure theory of functions, the construction of $\lambda$-terms is very simple.

<img src="Compiler/Screen Shot 2021-12-28 at 10.32.36 PM.png" style="zoom:50%;" />

* At first glance, $\lambda$-terms seem rather limited:

  * no numbers;
  * no arithmetic operations;
  * no data structures;
  * no programming primitives;
  * ...

* It turns out that we do not really need them!

* All (computable) numbers and operations can be defined as purely functional constructions, built using only abstraction and application.

* Nonetheless, for thr purposes of this lesson, we use some operations and numbers without defining them as $\lambda$-terms.

* Furthermore, we will not work within the framework of pure $\lambda$-calculus. Instead, we just add some concepts from $\lambda$-calculus to our example language.

* As an example, let $t:= x^2+1$.

* By **abstracting** $x$, we are designating this variable as an 'input' argument to the function that:

  * takes the input of $x$;
  * squares it;
  * then adds 1 to the result.

* This function is written as $\lambda x.x^2+1$.

* Finally, the function $\lambda x.x^2+1$ is applied on any given argument using **application**.

* As another example, let $s:=x^2+y$.

* By abstracting $x$ and $y$, we are designating these two variables as the 'input' arguments to the function that:

  * takes the inputs $x$ and $y$;
  * squares $x$;
  * then adds $y$ to the result.

* This function is written as $\lambda x.\lambda y.x^2+y$, or simply $\lambda xy.x^2+y$.

* Now, we expect the result of:

  * $((\lambda xy.x^2+y)3)5$

  to be $3^2+5=14$.

#### 6.20 Some conventions

* For convenience, we use some conventions that allow us to save on parentheses:
  * Application associates to the left. Hence, we write $(t_1t_1t_3)$ for $((t_1t_2)t_3)$;
  * $\lambda$-abstraction associates to the right. Thus, we write $\lambda x.\lambda y.x$ for $\lambda x.(\lambda y.x)$;
  * We can use a single $\lambda$ symbol followed by several variables to mean consecutive abstractions. So, we write $\lambda xy.x$ for $\lambda x.\lambda y.x$.

#### 6.21 Variables in $\lambda$-calculus

* In $\lambda$-calculus, variables serve a different function to that of imperative programming.
* In $\lambda$-calculus, *and as a consequence, pure functional programming*, a variable $x$ is just a place-holder to denote any possible value.
* In imperative programming, on the other hand, variables represent mamory locations containing values that can be modified.
* In other words, in pure functional programming, variables are *immutable*, while in imperative programming, they are *mutable*.

#### 6.22 beta-reduction ($\beta$-reduction)

* As $\lambda$-calculus is a model of computation, we need to describe how a basic computation step is carried out over $\lambda$-terms.
* In computing $(\lambda x.x^2+1)3$, we substitute 3 for the variable $x$ in the *body* of the function $\lambda x.x^2 +1$, to obtain $3^2+1=10$.
* This is called $\beta$-reduction.
  * It is essentially the only kind of computation done in $\lambda$-calculus.
* Let $t[x:=t_2]$ denote the term obtained from $t_1$ by substituting all occurrences of the variable $x$ with the term $t_2$.

<img src="Compiler/Screen Shot 2021-12-28 at 10.52.06 PM.png" style="zoom:50%;" />

#### 6.23 Extension: Functions

Now, we return to our example language, and extend its syntax, as follows:

<img src="Compiler/Screen Shot 2021-12-28 at 10.53.37 PM.png" style="zoom:50%;" />

New evaluation rules:

<img src="Compiler/Screen Shot 2021-12-28 at 10.54.02 PM.png" style="zoom:50%;" />

Note:

* **Left to right evaluation order**: first the function (E-APP1), then the argument (E-APP2).
* **Call-by-value**: the argument fully evaluated before function "invoked" (E-APPABS).

New typing rules:

<img src="Compiler/Screen Shot 2021-12-28 at 10.55.37 PM.png" style="zoom:50%;" />
