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

