# Blueprint for Minimal Viable Product.

## Step 1. Create simple Smart-Contract syntax to compile integers

### Part 1. High Level Design

#### 1. Lexing
The first step in most programming languages is lexing, or tokenizing. ‘Lex’ is short for lexical analysis, a very fancy word for splitting a bunch of text into tokens.

The lexer takes in text (source code) and transforms it into tokens. Tokens are things like a number, a string, or a name.

In Cell , the types of tokens are:

Numbers, e.g 12 or 4.2
Strings, e.g. "foo" or 'bar'
Symbols, e.g. `baz` or `qux_Quux`
Operators, e.g. `+` or `–`
Special punctuation, including `(, }` and `;`

So, the lexer is really just a function that takes in a string (some Cell source code) and returns all the tokens it finds in that string. The main function is shown in listing 1.

```
def lex(chars_iter):
  chars = PeekableStream(chars_iter)
  while chars.next is not None:
    c = chars.move_next()
    if c in " \n": pass
      # Ignore white space
    elif c in "(){},;=:": yield (c, "")  
      # Special characters
    elif c in "+-*/":  yield ("operation", c)
    elif c in ("'", '"'): yield ("string",
      _scan_string(c, chars))
    elif re.match("[.0-9]", c): yield ("number",
      _scan(c, chars, "[.0-9]"))
    elif re.match("[_a-zA-Z]", c):
      yield ("symbol", _scan(c, chars, 
             "[_a-zA-Z0-9]"))
    elif c == "\t": raise Exception(
      "Tabs are not allowed in Cell.")
    else: raise Exception(
      "Unexpected character: '" + c + "'.")
```
			
**Listing 1**
The lex function takes in an argument called chars_iter that provides the characters of the code we are lexing. This can be anything that gives us single characters if we loop through it, for example an ordinary string. We immediately wrap chars_iter in a PeekableStream , which is a little class (shown in listing 2) that allows us to check one character ahead in the stream of characters we are receiving.

```
class PeekableStream:
  def __init__(self, iterator):
    self.iterator = iter(iterator)
    self._fill()
  def _fill(self):
    try:
      self.next = next(self.iterator)
    except StopIteration:
      self.next = None
  def move_next(self):
    ret = self.next
    self._fill()
    return ret
```

#### 2. Tokens
A token is a small unit of a language. A token might be a variable or function name (AKA an identifier), an operator or a number.

#### 3. Flex
Flex, a program that generates lexers. You give it a file which has a special syntax to describe the language’s grammar. From that it generates a Java program which lexes a string and produces the desired output.

### Part 2. Parsing
The second stage of the pipeline is the parser. Will try to see if can parse without the need for creating trees (compiling integers only as first method), if not, then proceed with the parser that turns a list of tokens into a tree of nodes. A tree used for storing this type of data is known as an Abstract Syntax Tree, or AST.

*Parser Duties*: The parser adds structure to the ordered list of tokens the lexer produces. To stop ambiguities, the parser must take into account parenthesis and the order of operations. Simply parsing operators isn’t terribly difficult, but as more language constructs get added, parsing can become very complex.

***More To Be Added Here***

## Step 2: Integrate lexed syntax into compiler for translation.

### Part 1. Transpiling

#### 1. Transpiling
Write Jehova to Solidity transpiler, and the ability to automatically compile the output source with GCC.

## Notes for Production

[Structure of a Smart-Contract](https://blog.knoldus.com/structure-of-a-contract-in-solidity/#:~:text=Smart%20Contracts%20for%20Ethereum%20are,classes%20in%20object%2Doriented%20languages)
