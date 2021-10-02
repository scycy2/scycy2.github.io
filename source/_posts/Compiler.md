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
