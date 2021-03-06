# Compilers and Parsing
Your program is also a piece of data!

## How Humans Read
We learn through practice and exposure. We don't study grammar till much later in life.
We break down a sentence into a sequence of words.

1. Lexing (to tokens)
2. Parsing (grammar)
3. Semantic Analysis
4. Generation (feeling/emotion/meaning)

Then we parse. We understand the types of speech and how they related to one another.

Then we apply semantic meaning. We disambiguate and try to understand what something 'means'.
We get meaning from context.

Then we 'generate'. The brain combines all these steps to create meaning in our minds.

## How Computers Read
1. Lexing (to tokens)
Computers convert a program to a string of words (tokens).
2. Parsing (grammar)
Parse those tokens using a grammar into a structure.

3. Semantic Analysis
Is this program correct?

4. Optimization (humans don't do this as much -- it's editing!)

5. Generation of Executable Code
Generates a lower level of code -- typically object code or machine code. Now Compilers
have a broader meaning, including compiling to an 'IR' intermediate representation. This is often
called 'Transpiling'.

However computers can't handle ambiguity. Grammar is strict and formal -- rules are clear and important. Lots of focus on optimization.

#### Hierarchy of Abstraction
Ultimately all code reaches down to the instruction set of the CPU.
Above this is assembly code.
Above this is a language like C. It has vars and functions which are meaningless to the CPU but you are still handling memory directly.
Above this is a language like Java or Javascript, which is much more abstract and does not handle memory or garbage collection directly.

> Aside: LLVM (lower level virtual machine) is a project that takes many different types of languages and compiles it into many different lower level languages.

## Why Study Compilers?
Combines many different CS fields like data structures, algorithms, memory allocation, stack management, and optimization.

> Aside: Tree-shaking – you shake a tree of dependencies and old code to remove lightly coupled code.

Compilers are use all over and embedded in many tools that we use: React, babel.js, ESLint, Uglify, Angular.

## History of Compilers
+ 1842: Ada Lovelace – first programming language. She rocks!
+ 1936: Alan Turing – describes first computing system (Turing Machine).
+ 1952: Grace Hopper – wrote first compiler A-0. Later worked on COBOL.
+ 1953: John Backus – Inventor of Backus-Naur Form, a meta way of describing programming languages. Creator of Fortran.
+ Present: Alfred Aho – author of Dragon Book (_Compilers_).

## How do Compilers Work?
(Input) Source code --> Take in Front End (Parses and Checks Input) --> Back End (Optimizes and Generates Code) --> Lower Level Code, Assembly, or IR. (Output)

#### Inside Frontend
(Input) SourceCode --> Lexical --> Syntactic --> Semantic --> Parse Tree (Output)

> Aside: Languages become popular because they solve a problem in an interesting way. They don't really become obsolete, they just stop being the new hot thing. New businesses and problems are tied to their language.

Lexical analysis looks through the language and checks its list for valid tokens. Based on valid tokens it generates a valid 'token stream'. Like an array of tokens.

Parsing looks at the tree structure of statements to see what makes sense.

Grammars: Symbols and tokens. We use production rules to parse our symbols and tokens.
– Left hand side of production rule must be a single symbol. These are _non-terminals_.
– Right hand side of production rules is a string with 0 or more symbols. These are _terminals_.

The language of a grammar are all the terminals that can be generated by the grammar. If you can generate a valid tree from a sentence that uses the production rules then you have a _valid sentence_.

E

E + E

num + E x E

num + num * num

A Parse Tree for string(x) = proof that x is in L(G). Parsing is **deterministic**, they parse into the same tree every time.

Grammar defines production rules => Given these rules you can create statements in a language.
Given a statement in a language => Can you parse it back into a tree of the production rules?

## Parsing
**Languages are parsed either top-down or bottom-up.**

Bottom-up: backtracking and shift reduce. More unwieldy than top-down. (I did not get this.)

Top-down:
1. Recursive descent:
We descend into the tree and consume tokens. The result of the recursion will be the parse tree. Manually built. Grammar defined usually as LL(1). LL means 'left to right'. Left-most derivation means we choose the left-most way of moving the tree. You must be able to apply a rule based on the next left side token. <= most popular way of making compilers.

        parse_E():
          t1 = parse_T();
          t2 = parse_A();
          return makeTree("E", t1, t2);

E = Expression => "Legal combination of terms and combinations"
T = Term => Similar to factor, the one or two in 1 * 2
F = Factor => Similar to term, the one or two in 1 * 2
A, B, and other symbols are arbitrary

** Terminals are our base case. They stop our parsing! **

1 + 2 * 3
E

    E => TA   // E -> TA
        FBA   // T -> FB
        nBA     // F -> num
        nA      // B -> eps (nothing)
        n + TA  // A -> + TA
        n + FBA // T -> FB
        n + nBA // F -> num
        n + n * FBA // B -> starFB
        n + n * nBA // F -> num
        n + n * b // B-> eps A -> eps

> Parser looks one step ahead to look for the production rules that matches the next token ahead.


    E
    |\
    T A
    FB +TA
    ne  F <-- evidence of in


    parseF() {
      if (nextToken() == "(")) {
        // get (
        parseE();
        // get )        
      } else if (nextToken().match(/0-9/)) {
        parseNumber();
      } else {
        throw new ParseError("No factor found.");
      }
    }

> Token stream is like a string of PacMan cheeses. C x x x x x x x
Can I eat this? No. Ok, call next rule. Can I eat this? No. Ok, call next rule. CONSUME CHEESE!

>Attempt at summary:
Is this a fair summary:  We would begin by evaluating the ‘expression’ E. When that evaluation parseE() starts, it calls these many sub rules/functions (from production rules) that look at the value of the next token then use conditionals to recursively call evaluations on the current token?

ESPRIMA is a ECMA script parser.

## Key Takeaways
Your program is also a piece of data.
Compilers first two main tasks are lexing and parsing.
Programming languages are defined by grammars.
Top-down and bottom-up parsing.
Top-down parsing often uses recursive-descent if grammar is LL(1) (left to right, left recursive, one token lookahead).
