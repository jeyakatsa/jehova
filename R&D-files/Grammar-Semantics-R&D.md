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

##### Primitive Types and Values
A primitive type is predefined by the Java programming language and named by its reserved keyword:

```
PrimitiveType:
    NumericType
    boolean

NumericType:
    IntegralType
    FloatingPointType

IntegralType: one of
    byte short int long char

FloatingPointType: one of
    float double
```

Primitive values do not share state with other primitive values.

The numeric types are the integral types and the floating-point types.

The integral types are byte, short, int, and long, whose values are 8-bit, 16-bit, 32-bit and 64-bit signed two's-complement integers, respectively, and char, whose values are 16-bit unsigned integers representing UTF-16 code units.

The floating-point types are float, whose values include the 32-bit IEEE 754 floating-point numbers, and double, whose values include the 64-bit IEEE 754 floating-point numbers.

The boolean type has exactly two values: true and false.

######  Integral Types and Values
The values of the integral types are integers in the following ranges:

- For byte, from -128 to 127, inclusive
- For short, from -32768 to 32767, inclusive
- For int, from -2147483648 to 2147483647, inclusive
- For long, from -9223372036854775808 to 9223372036854775807, inclusive
- For char, from '\u0000' to '\uffff' inclusive, that is, from 0 to 65535

###### Integer Operations
The Java programming language provides a number of operators that act on integral values:

- The comparison operators, which result in a value of type boolean:
- - The numerical comparison operators `<`, `<=`, `>`, and `>=`
- - The numerical equality operators `==` and `!=`
- The numerical operators, which result in a value of type int or long:
- - The unary plus and minus operators `+` and `-`
- - The multiplicative operators `*`, `/`, and `%`
- - The additive operators `+` and `-`
- - The increment operator `++`, both prefix and postfix
- - The decrement operator `--`, both prefix and postfix
- - The signed and unsigned shift operators `<<`, `>>`, and `>>>`
- - The bitwise complement operator `~`
- - The integer bitwise operators `&`, `^`, and `|`
- The conditional operator `? :`
- The cast operator, which can convert from an integral value to a value of any specified numeric type
- The string concatenation operator `+`, which, when given a String operand and an integral operand, will convert the integral operand to a String representing its value in decimal form, and then produce a newly created String that is the concatenation of the two strings.

###### Reference Types and Values
There are four kinds of reference types: `class` types, `interface` types, type `variables`, and `array` types.

```
ReferenceType:
    ClassOrInterfaceType
    TypeVariable
    ArrayType

ClassOrInterfaceType:
    ClassType
    InterfaceType

ClassType:
    TypeDeclSpecifier TypeArgumentsopt

InterfaceType:
    TypeDeclSpecifier TypeArgumentsopt

TypeDeclSpecifier:
    TypeName  
    ClassOrInterfaceType . Identifier

TypeName:
    Identifier
    TypeName . Identifier

TypeVariable:
    Identifier

ArrayType:
    Type [ ]
```

###### Type Variables
A type variable is an unqualified identifier used as a type in class, interface, method, and constructor bodies.

A type variable is declared as a type parameter of a generic class declaration, generic interface declaration, generic method declaration, or generic constructor declaration.

```
TypeParameter:
    TypeVariable TypeBoundopt

TypeBound:
    extends TypeVariable
    extends ClassOrInterfaceType AdditionalBoundListopt

AdditionalBoundList:
    AdditionalBound AdditionalBoundList
    AdditionalBound

AdditionalBound:
    & InterfaceType
```    
    
The scope of a type variable declared as a type parameter is specified in.

Every type variable declared as a type parameter has a bound. If no bound is declared for a type variable, Object is assumed. If a bound is declared, it consists of either:

- a single type variable T, or

- a class or interface type T possibly followed by interface types I1 & ... & In.

It is a compile-time error if any of the types I1 ... In is a class type or type variable.

The erasures of all constituent types of a bound must be pairwise different, or a compile-time error occurs.

A type variable must not at the same time be a subtype of two interface types which are different parameterizations of the same generic interface, or a compile-time error occurs.

The order of types in a bound is only significant in that the erasure of a type variable is determined by the first type in its bound, and that a class type or type variable may only appear in the first position.

The members of a type variable X with bound T & I1 & ... & In are the members of the intersection type T & I1 & ... & In appearing at the point where the type variable is declared.

###### Parameterized Types
A generic class or interface declaration C with one or more type parameters A1,...,An which have corresponding bounds B1,...,Bn defines a set of parameterized types, one for each possible invocation of the type parameter section.

Each parameterized type in the set is of the form C<T1,...,Tn> where each type argument Ti ranges over all types that are subtypes of all types listed in the corresponding bound. That is, for each bound type Si in Bi, Ti is a subtype of Si[F1:=T1,...,Fn:=Tn].

A parameterized type is written as a ClassType or InterfaceType that contains at least one type declaration specifier immediately followed by a type argument list <T1,...,Tn>. The type argument list denotes a particular invocation of the type parameters of the generic type indicated by the type declaration specifier.

Given a type declaration specifier immediately followed by a type argument list, let C be the final Identifier in the specifier.

It is a compile-time error if C is not the name of a generic class or interface, or if the number of type arguments in the type argument list differs from the number of type parameters of C.

Let P = C<T1,...,Tn> be a parameterized type. It must be the case that, after P is subjected to capture conversion (§5.1.10) resulting in the type C<X1,...,Xn>, for each type argument Xi (1 ≤ i ≤ n), Xi <: Bi[A1:=X1,...,An:=Xn], or a compile-time error occurs.

The notation [Ai:=Ti] denotes substitution of the type variable Ai with the type Ti for 1 ≤ i ≤ n, and is used throughout this specification.

In this specification, whenever we speak of a class or interface type, we include the generic version as well, unless explicitly excluded.

Examples of parameterized types:

```
Vector<String>

Seq<Seq<A>>

Seq<String>.Zipper<Integer>

Collection<Integer>

Pair<String,String>

Examples of incorrect invocations of a generic type:

Vector<int> is illegal, as primitive types cannot be type arguments.

Pair<String> is illegal, as there are not enough type arguments.

Pair<String,String,String> is illegal, as there are too many type arguments.
```
