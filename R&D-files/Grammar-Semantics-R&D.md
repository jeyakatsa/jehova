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

#### Unicode Escapes
A compiler for the Java programming language ("Java compiler") first recognizes Unicode escapes in its input, translating the ASCII characters \u followed by four hexadecimal digits to the UTF-16 code unit (§3.1) of the indicated hexadecimal value, and passing all other characters unchanged. Representing supplementary characters requires two consecutive Unicode escapes. This translation step results in a sequence of Unicode input characters.


```java
UnicodeInputCharacter:
    UnicodeEscape
    RawInputCharacter

UnicodeEscape:
    \ UnicodeMarker HexDigit HexDigit HexDigit HexDigit

UnicodeMarker:
    u
    UnicodeMarker u

RawInputCharacter:
    any Unicode character

HexDigit: one of
    0 1 2 3 4 5 6 7 8 9 a b c d e f A B C D E F
```
 
The \, u, and hexadecimal digits here are all ASCII characters.

In addition to the processing implied by the grammar, for each raw input character that is a backslash \, input processing must consider how many other \ characters contiguously precede it, separating it from a non-\ character or the start of the input stream. If this number is even, then the \ is eligible to begin a Unicode escape; if the number is odd, then the \ is not eligible to begin a Unicode escape.

```
For example, the raw input "\\u2126=\u2126" results in the eleven characters " \ \ u 2 1 2 6 = Ω " (\u2126 is the Unicode encoding of the character Ω).
```

If an eligible \ is not followed by u, then it is treated as a RawInputCharacter and remains part of the escaped Unicode stream.

If an eligible \ is followed by u, or more than one u, and the last u is not followed by four hexadecimal digits, then a compile-time error occurs.

The character produced by a Unicode escape does not participate in further Unicode escapes.

```
For example, the raw input \u005cu005a results in the six characters \ u 0 0 5 a, because 005c is the Unicode value for \. It does not result in the character Z, which is Unicode character 005a, because the \ that resulted from the \u005c is not interpreted as the start of a further Unicode escape.
```

The Java programming language specifies a standard way of transforming a program written in Unicode into ASCII that changes a program into a form that can be processed by ASCII-based tools. The transformation involves converting any Unicode escapes in the source text of the program to ASCII by adding an extra u - for example, \uxxxx becomes \uuxxxx - while simultaneously converting non-ASCII characters in the source text to Unicode escapes containing a single u each.

This transformed version is equally acceptable to a Java compiler and represents the exact same program. The exact Unicode source can later be restored from this ASCII form by converting each escape sequence where multiple u's are present to a sequence of Unicode characters with one fewer u, while simultaneously converting each escape sequence with a single u to the corresponding single Unicode character.

#### Line Terminators
A Java compiler next divides the sequence of Unicode input characters into lines by recognizing line terminators.
```
LineTerminator:
    the ASCII LF character, also known as "newline"
    the ASCII CR character, also known as "return"
    the ASCII CR character followed by the ASCII LF character
```    

InputCharacter:
    UnicodeInputCharacter but not CR or LF
Lines are terminated by the ASCII characters CR, or LF, or CR LF. The two characters CR immediately followed by LF are counted as one line terminator, not two.

A line terminator specifies the termination of the // form of a comment.

The lines defined by line terminators may determine the line numbers produced by a Java compiler.

The result is a sequence of line terminators and input characters, which are the terminal symbols for the third step in the tokenization process.

#### Input Elements and Tokens
The input characters and line terminators that result from escape processing and then input line recognition are reduced to a sequence of input elements.

```
Input:
    InputElementsopt Subopt

InputElements:
    InputElement
    InputElements InputElement

InputElement:
    WhiteSpace
    Comment
    Token

Token:
    Identifier
    Keyword
    Literal
    Separator
    Operator
```    
### Chapter 4. Types, Values, and Variables

The Java programming language is a statically typed language, which means that every variable and every expression has a type that is known at compile time.

The Java programming language is also a strongly typed language, because types limit the values that a variable can hold or that an expression can produce, limit the operations supported on those values, and determine the meaning of the operations. Strong static typing helps detect errors at compile time.

The types of the Java programming language are divided into two categories: primitive types and reference types. The primitive types are the boolean type and the numeric types. The numeric types are the integral types byte, short, int, long, and char, and the floating-point types float and double. The reference types are class types, interface types, and array types. There is also a special null type. An object is a dynamically created instance of a class type or a dynamically created array. The values of a reference type are references to objects. All objects, including arrays, support the methods of class Object. String literals are represented by String object.

#### The Kinds of Types and Values
There are two kinds of types in the Java programming language: primitive types and reference types. There are, correspondingly, two kinds of data values that can be stored in variables, passed as arguments, returned by methods, and operated on: primitive values and reference values.

```
Type:
    PrimitiveType
    ReferenceType
 ```

There is also a special null type, the type of the expression null, which has no name.

Because the null type has no name, it is impossible to declare a variable of the null type or to cast to the null type.

The null reference is the only possible value of an expression of null type.

The null reference can always undergo a widening reference conversion to any reference type.
