+++
title = "Parser Combinators"
date = 2025-09-27
description = "An explanation of common parsing methods for programming languages, an introduction to parser combinators, and why I chose to write my own parser library"

[taxonomies]
tags = ["Programming Languages", "Rust"]
+++

## Parsing Programming Languages

Most programming language compilers use a parser generator or a handwritten parser to parse the language and construct the syntax tree. Handwritten parsers provide the most control, allowing detailed errors to be constructed while being very performant. Parser generators turn high-level grammar descriptions into highly optimized state machine based parsers. Another elegant approach is parser combinators. These combinators can be composed into parsers with other combinators, allowing the entire parser to be constructed in a purely functional manner.

## What Are Parser Combinators?

Parser combinators are not often seen in compilers for common languages however. Implementing them in lower level languages like C would be awkward, and purely functional languages like Haskell come with unwanted overhead, and a lack of programmers that know how to use them.

## How They Can Be Optimized

Many performance issues with parser combinators arise from issues that are perfectly fixable with the right language:

+ **Function call overhead** -> Can be fixed in any language that allows control over inlining.
+ **Excessive Allocation** -> The allocations used to store errors and output can be optimized away when the errors and output aren't needed with clever use of parameterized types.
+ **Redundant Parsing of Long Branches** -> Parsers may repeatedly attempt the same branch for overlapping prefixes; caching results avoids this.

## Pups - My Parser Combinator Library

Rust’s zero-cost abstractions make parser combinators practical for language parsing. However, existing solutions were not quite right for my use case. The [nom](https://crates.io/crates/nom) crate seems mainly designed for byte/text based input, and I was wanting to tokenize my programming language syntax before parsing, so I wanted something designed to be more general purpose with a more transparent API. [Chumsky](https://crates.io/crates/chumsky) seemed to be what I wanted at first, but I didn't want the lack of type-level distinction between fatal errors, and accumulated recoverable errors.

I decided to create [Pups](https://github.com/rob-gage/pups) to address these gaps. By designing it to easily allow handwritten parsers to be used alongside parser combinators, I will have a future-proof way of writing parsers that allows rapid iteration, but full control when necessary. I've been working on several small-to-large language projects, and having my own library will also allow me to improve the tooling for all of them simultaneously.
