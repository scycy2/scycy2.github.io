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

  * If $a * T$ (the reverse of $T*a$) were on top of the stack, and we simply reduced $a$ to $T$, then the current string in a rightmost derivation would contain the substring $T*T$, which is never possible.
  * Similarly, if $T*S_1$ (the reverse of $S_1*T$) were on top, then reducing $T$ to $S_1$ cannot be correct.

* These two situations can summarized by saying that **when a reduction is executed, the longest possible string should be removed from the stack during the reduction.**

* Essentially, the only remaining question is what to do when $T$ is the top stack symbol: shift or reduce?

* We can answer this by considering the possibilities for the next input symbol.

  * If it is $+$, then this $+$ should eventually be part of the expression $S_1 + T$ (in reverse) on the stack, so that $S_1 + T$ can be reduced to $S_1$; therefore, the non-terminal $T$ should eventually be reduced to $S_1$, and so the time to do it is now.
  * Similarly, if the next input is $\$$, then the non-terminal $T$ should be reduced to $S_1$, so that $S_1\$$ can be reduced to $S$.
  * The only other symbol that can follow $T$ in a rightmost derivation is $*$, and $*$ cannot follow any other symbol; therefore, $*$ should be shifted onto the stack.

With these observations, we can formulate the rules that the deterministic bottom-up PDA should follow in choosing its moves:

1. If the top stack symbol is $Z_0$, $S_1$, $+$, or $*$, shift the next input symbol to the stack. (None of these four symbols is the rightmost symbol in the right side of a production.)
2. If the top stack symbol is $\$$, reduce $S_1\$$ to $S$.
3. If the top stack symbol is '$a$', reduce $T*a$ to $T$ if possible; otherwise reduce $a$ to $T$.
4. If the top stack symbol is $T$, then reduce (to $S_1$) if the next input is $+$ or $\$$, otherwise shift.
5. If the top stack symbol is $S$, then pop it from the stack; if $Z_0$ is the next top symbol, accept, otherwise reject.

* All these rules, except rule 4, are easily incorporated into a transition table for a deterministic pushdown automaton (DPDA), but the fourth may require a little clarification:

  * If the PDA sees the combination ($*$, $T$) of next input symbol and stack symbol, then it shifts $*$ onto the stack.
  * If it sees ($+$, $T$) or ($\$$, $T$), then the moves it makes are the ones that carry out the appropriate reduction, and then shift the input symbol onto the stack.

* The point is that "seeing" ($+$, $T$), for example, implies reading the $+$, and the PDA then uses auxiliary states to remember that after it has performed a reduction, it should place $+$ on the stack.

* We now have the essential specifications for a DPDA.

* Like the non-deterministic PDA $NB(G)$, the moves it makes in accepting a string simulate, **in reverse**, the steps in a **rightmost derivation** of that string.

* and in this sense, our DPDA serves as a bottom-up parser for $G$.

  <img src="Compiler/Screen Shot 2021-10-25 at 2.59.01 PM.png" style="zoom:50%;" />

* The moves for the DPDA and the input string $a+a*a\$$ are shown in the table above.

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

<h2 align='center'>Syntactic Analysis: Parser Generators</h2>

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

