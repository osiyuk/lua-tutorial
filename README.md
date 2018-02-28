# Lua From Scratch

This is a tutorial that walks through writing a complete, production-ready
implementation of a real programming language in 17,000 lines of C. By the end
you will have a thorough understanding of every detail of the Lua implementation.

The tutorial isn't done yet. You can [sign up](http://eepurl.com/cIOGCD) to be notified
when it's done, or maybe when part of it is ready for consumption.

## What you will implement

Embeddable Lua library and interpreter, defined by
[Reference Manual](https://www.lua.org/manual/5.3/manual.html). This includes:

* Lua code examples (for testing)
* Parser/lexer (handwritten, not generated)
* Virtual machine
* Code generator (compiling to bytecode)
  * Constant folding
  * Code optimizations
* Value representation
* The Lua C API
* Prototype inheritance (metatables)
* Hash table implementation (chained scatter table)
* Garbage collection (incremental mark-and-sweep)
  * Weak references
* String interning
* Closures
* Coroutines
* Operator overloading (tag methods)
* Error handling
* The Lua standard library
  * String pattern matching (similar to regular expressions)
* Debugging features
* `lua`, the Lua REPL executable
* `luac`, the Lua compiler
* Portable `Makefile`
* And much, much more...

## The Plan

Here are steps to take in developing this tutorial:

1. Get familiar with [Lua docs](https://www.lua.org/docs.html). (make sure to read papers!)
2. Get familiar with the [Lua source code](https://www.lua.org/source/5.3/). (Mike Pall wrote
   a [nice guide](https://www.reddit.com/r/programming/comments/63hth/ask_reddit_which_oss_codebases_out_there_are_so/c02pxbp/))
3. Annotate the Lua source code, in great detail.
4. Decide on a general order in which to implement everything in Lua.
5. Split the Lua source code into a series of 20 to 70 "macro-steps", where
   each step adds about a chapter's worth of functionality.
6. Iterate on these macro-steps, adding/removing/reordering them until I find a
   good order for them to go in that will best suit the book.
7. Turn each macro-step into a chapter by splitting it into micro-steps,
   writing 1-3 paragraphs to explain each micro-step, and also explaining any
   higher level concepts (like explaining garbage collection in general and
   then giving an overview of how Lua's GC algorithm works).

_I'm well into step 2, and am starting work on step 3._

Update: _I'm skipping step 3, it's not worth it. I'm diving right in to step 4.
See `MACROSTEPS.md`._

### Possible orders

* Topic based: Jump around to different parts of the codebase early and often,
  to keep it interesting and to help with holding the entire codebase
  in your head. Like a TV show that jumps to many different story arcs
  each episode, making just a bit of progress on each one.
  * I really like the idea of this but will have to figure out if it would
  actually work in practice
* Classic: repl -> lexing -> parsing -> evaluating expressions -> ...
  * Not good because Lua does parsing/compiling in one single, tightly-written
    step. Also this is how every other programming language implementation book
    does it, it's getting old.
* Stages: Implement a single component of Lua as the first stage, test it,
  then add more components (like parser, code generator, VM, GC) in next stages.
  * Each stage should produce meaningful results and be fruitful
* Augmenting C: tvalue -> stack -> c api -> hash tables -> GC -> ...
  * Starts by adding duck-typing features to C, then gets right into the lua
    API and then goes into actual interesting language features like hash
    tables and GC, with no need for boring parsing
* Historical: Implement things in the order they were actually implemented in
  Lua over the years.
  * Let's not do that, okay? ("Chapter 105: Adding `true` and `false` to our
    language")
  * Although, Lua did start out as a data-description language (kinda like
    JSON), so for the parser we could start by parsing literal values/tables
    before going on to parse executable code.

_I think the "Augmenting C" order will be used for the beginning, and then
we'll try to keep it interesting by doing the "topic after topic" thing. Some
features might use the "Stages" idea, where a basic version will be
implemented in say the first half of the book, then the more advanced version
will come much later. So there, it'll be a combination of the above ordering
ideas. (Even the "Classic" order will probably make an appearance... it just
won't be anywhere near the beginning of the book.)_

### Ideas

* Self-contained chapters: Might be nice to make at least certain chapters more
  or less self-contained. So if chapter #38 is all about GC, don't assume
  they've worked through the entire book up to that point, or that they even
  know anything about Lua. They might just be someone interested in learning
  about GC. Keep those people in mind.
* "Final" lines of code: mark lines of code that are in their "final" state.
  That is, lines that exist in the final version of the code. Give them a
  subtle grey background maybe, like the line is "set in stone".
