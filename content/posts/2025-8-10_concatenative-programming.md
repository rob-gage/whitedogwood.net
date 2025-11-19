+++
title = "Concatenative Programming"
date = 2025-08-10
description = "Information about concatenative programming, and about specific concatenative languages"

[taxonomies]
tags = ["Programming Languages"]
+++

## About Concatenative Programming

Concatenative programming is an elegant programming style using sequenced point-free (tacit) functions (functions with anonymous arguments) to compose entire programs. Functions and programs are simply
sequenced applications of other functions.

Concatenative languages are usually implemented on stacks, with function arguments being consumed
from the top of the stack, and outputs placed back on the top. The arguments of functions being on the stack prior to function application often results in concatenative languages using a Reverse
Polish Notation (RPN, also called postfix notation) for arithmetic operations.

The standard infix expression `9 + 3` is written in RPN as `9 3 +`. Infix expression `5 - 4` is
written as `5 4 -` in RPN. In stack-based concatenative languages, number literals are often
treated as functions that produce that number on top of the stack to be consumed by following
functions. As described, `10 / 2 - 2` is written as `10 2 / 2 -`.

## Forth

The first concatenative language was the stack-based Forth. It had an interactive programming
environment that allowed "words" (functions) to be defined, and used in other word definitions. Since its creation in the 1960s, many variants, both free and proprietary, have emerged. It was designed for systems with limited hardware capabilities.

## Joy

Joy was a functional concatenative language. Every function is a pure function, consuming and returning the stack. It can also store "quoted" programs on the stack as data, to be consumed and
used by other functions. Functions can be composed simply by concatenating the bodies of functions as quoted programs.

## Factor

Factor is a full-featured concatenative language with a compiler and standard library. It is more designed for practical use than Joy, and has a web framework, Furnace, along with a database library.

## Compose

I have been developing a simple and comprehensible concatenative language named Compose. A function to square a number in Compose is written as follows:

`square : copy * ;`

When applied to the number 5 like `5 square`, functions can be substituted quite simply by
replacing them with their terms to produce `5 copy *`. `5 copy *` becomes `5 5 *`, which becomes
`25`.

Functions as data are called lambdas in Compose, and they are represented with parentheses.

The code `( copy * ) copy compose` will evaluate to a leave lambda `( copy * copy * )`, allowing
easy function composition at runtime. `5 ( copy * ) apply` evaluates to `36`.
