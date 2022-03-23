# Blueprint for Minimal Viable Product.

## Step 1. Create simple Smart-Contract syntax to compile integers

### Part 1. High Level Design

#### 1. Lexing
The first step in most programming languages is lexing, or tokenizing. ‘Lex’ is short for lexical analysis, a very fancy word for splitting a bunch of text into tokens.

A Cell program

This program shows how to make variables and call functions in Cell:
```
  x = 3;
  y = x + 2;
  print(y);
```
When you run it, this program will print out the value of y, which is 5.

Cell programs should be relatively familiar to people who've used Java, and also takes inspiration from dynamic languages like Python and Ruby. (In fact, under the covers, the language Cell looks most like is Lisp, but its syntax is different.)

The lexer takes in text (source code) and transforms it into tokens. Tokens are things like a number, a string, or a name.

In Cell , the types of tokens are:

Numbers, e.g 12 or 4.2
Strings, e.g. "foo" or 'bar'
Symbols, e.g. `baz` or `qux_Quux`
Operators, e.g. `+` or `–`
Special punctuation, including `(, }` and `;`

**Listing 1**

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
The `lex` function takes in an argument called `chars_iter` that provides the characters of the code we are lexing. This can be anything that gives us single characters if we loop through it, for example an ordinary string. We immediately wrap `chars_iter` in a `PeekableStream` , which is a little class (shown in listing 2) that allows us to check one character ahead in the stream of characters we are receiving.

**Listing 2**
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

The main body of the lex function is a while loop stepping through the characters one by one, and for each one doing something based on what type of character it is. When we are in this part of the code, we know we are looking for the beginning of a new token, and the first character of that token will help us decide what type of token it is.

The first line of the if allows us to skip over any white space (spaces or newlines) we find. The second line identifies any special characters. The values yielded by the lex function are pairs that look like (TYPE, VALUE), for example ( "string", "Hello" ) would represent a string token that contains the word ‘Hello’. The special characters are slightly different – we treat each character as a unique type, so when we find a ; character, we yield ( ";", "" ). This means the parser (which we will see in the next article in this series) can treat each special character differently. Because in Cell special characters are always exactly one character long, we can immediately yield a token when we find one.

The next part ( elif c in "+-*/" ) identifies arithmetic operations. Again, these tokens are always one character long, so we can immediately yield a token with type "operation" , and value the actual symbol the user typed.

Now we move on to more interesting tokens. If the first character of a token is a single or double quote, we know we are dealing with a string. We must scan forwards through the characters until we find a matching close quote, and then yield a token with type string. To scan forwards we call the function _scan_string , which is shown in listing 3.

**Listing 3**
```
def _scan_string(delim, chars):
  ret = ""
  while chars.next != delim:
    c = chars.move_next()
    if c is None:
      raise Exception( \
      "A string ran off the end of the program.")
    ret += c
  chars.move_next()
  return ret
```

`_scan_string` moves through the characters of the string, and stops when it reaches a matching quote. The characters between the quotes are returned, and this is the value of the token that is yielded from the lex function. Unlike most programming languages, Cell does not provide a way of ‘escaping’ a quote symbol by writing \" or similar. This is a limitation we have chosen to accept to keep our lexer simple.

Continuing through the big `if` / `elif` block in the `lex` function (listing 1), next we come to numbers. The first character in a number will always be a number or a decimal point, so when we see one of those we call the `_scan` function, telling it to keep consuming characters while it can see numbers or decimal points. 

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
