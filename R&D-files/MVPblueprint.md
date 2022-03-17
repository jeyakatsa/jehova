# Blueprint for Minimal Viable Product.

## Step 1. Create simple Smart-Contract syntax to compile integers

### Part 1. High Level Design

#### 1. Lexing
The first step in most programming languages is lexing, or tokenizing. ‘Lex’ is short for lexical analysis, a very fancy word for splitting a bunch of text into tokens.

#### 2. Tokens
A token is a small unit of a language. A token might be a variable or function name (AKA an identifier), an operator or a number.

#### 3. Flex
Flex, a program that generates lexers. You give it a file which has a special syntax to describe the language’s grammar. From that it generates a Java program which lexes a string and produces the desired output.

### Part 2. Parsing
The second stage of the pipeline is the parser. Will try to see if can parse without the need for creating trees (compiling integers only as first method), if not, then proceed with the parser that turns a list of tokens into a tree of nodes. A tree used for storing this type of data is known as an Abstract Syntax Tree, or AST.

*Parser Duties*: The parser adds structure to the ordered list of tokens the lexer produces. To stop ambiguities, the parser must take into account parenthesis and the order of operations. Simply parsing operators isn’t terribly difficult, but as more language constructs get added, parsing can become very complex.

***More To Be Added Here***

## Step 2: Create simple compiler to read created Syntax.

### Part 1. Transpiling

#### 1. Transpiling
Write Jehova to Java transpiler, and the ability to automatically compile the output source with GCC.

