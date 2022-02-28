# Grammar & Semantics Research & Development

## Java Overall Semantics (and how it might relate to Jehova):

### Chapter 1: Intro

The Java® programming language is a general-purpose, concurrent, class-based, object-oriented language. It is designed to be simple enough that many programmers can achieve fluency in the language. The Java programming language is related to C and C++ but is organized rather differently, with a number of aspects of C and C++ omitted and a few ideas from other languages included. It is intended to be a production language, not a research language, and so, as C. A. R. Hoare suggested in his classic paper on language design, the design has avoided including new and untested features.

The Java programming language is strongly and statically typed. This specification clearly distinguishes between the compile-time errors that can and must be detected at compile time, and those that occur at run time. Compile time normally consists of translating programs into a machine-independent byte code representation. Run-time activities include loading and linking of the classes needed to execute a program, optional machine code generation and dynamic optimization of the program, and actual program execution.

[Reference](https://docs.oracle.com/javase/specs/jls/se7/html/jls-1.html)

### Chapter 2: Grammar
This chapter describes the context-free grammars used in this specification to define the lexical and syntactic structure of a program.

#### 2.1. Context-Free Grammars
A context-free grammar consists of a number of productions. Each production has an abstract symbol called a nonterminal as its left-hand side, and a sequence of one or more nonterminal and terminal symbols as its right-hand side. For each grammar, the terminal symbols are drawn from a specified alphabet.

Starting from a sentence consisting of a single distinguished nonterminal, called the goal symbol, a given context-free grammar specifies a language, namely, the set of possible sequences of terminal symbols that can result from repeatedly replacing any nonterminal in the sequence with a right-hand side of a production for which the nonterminal is the left-hand side.

#### The Syntactic Grammar
A syntactic grammar has tokens defined by the lexical grammar as its terminal symbols. It defines a set of productions, starting from the goal symbol `CompilationUnit`, that describe how sequences of tokens can form syntactically correct programs.

##### 2.4. Grammar Notation
Terminal symbols are shown in fixed width font in the productions of the lexical and syntactic grammars, and throughout this specification whenever the text is directly referring to such a terminal symbol. These are to appear in a program exactly as written.

Nonterminal symbols are shown in italic type. The definition of a nonterminal is introduced by the name of the nonterminal being defined followed by a colon. One or more alternative right-hand sides for the nonterminal then follow on succeeding lines.

For example, the syntactic definition:

```java
IfThenStatement:
    if ( Expression ) Statement
```

states that the nonterminal IfThenStatement represents the token if, followed by a left parenthesis token, followed by an Expression, followed by a right parenthesis token, followed by a Statement.

As another example, the syntactic definition:

```java
ArgumentList:
    Argument
    ArgumentList , Argument
```    
states that an ArgumentList may represent either a single Argument or an ArgumentList, followed by a comma, followed by an Argument. This definition of ArgumentList is recursive, that is to say, it is defined in terms of itself. The result is that an ArgumentList may contain any positive number of arguments. Such recursive definitions of nonterminals are common.

### Chapter 3: Lexical Structure
Programs are written in Unicode, but lexical translations are provided so that Unicode escapes can be used to include any Unicode character using only ASCII characters. Line terminators are defined to support the different conventions of existing host systems while maintaining consistent line numbers.

The Unicode characters resulting from the lexical translations are reduced to a sequence of input elements, which are white space, comments, and tokens. The tokens are the identifiers, keywords, literals, separators, and operators of the syntactic grammar.

#### Unicode:
Programs are written using the Unicode character set. Information about this character set and its associated character encodings may be found at http://www.unicode.org/.

The Java SE platform tracks the Unicode specification as it evolves. The precise version of Unicode used by a given release is specified in the documentation of the class Character.

The Java programming language represents text in sequences of 16-bit code units, using the UTF-16 encoding.

Some APIs of the Java SE platform, primarily in the Character class, use 32-bit integers to represent code points as individual entities. The Java SE platform provides methods to convert between 16-bit and 32-bit representations.

*Can we convert in Jehova, 32-bit integers into 16 bit (similar to Python)?*

[Reference](https://docs.oracle.com/javase/specs/jls/se7/html/jls-3.html)


