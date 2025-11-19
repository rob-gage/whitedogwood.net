+++
title = "Compose Language Introduction"
date = 2025-11-10
description = "An introduction to my new concatenative programming language, Compose"

[taxonomies]
tags = ["Compose", "Programming Languages"]
+++

## About Compose

Compose is my concatenative language, designed for writing concise functions and programs in a tacit style. Functions are made up of data terms and function applications.

## Data terms

When data terms are encountered, such as `10` or `false` or `( copy * )`, they are pushed to the virtual machine's stack. The most recently pushed data will be consumed first by applied functions.

## Functions

### Core Functions

Compose provides a number of core functions that can be evaluated such as arithmetic operations, Boolean logic operations, and stack operations such as `copy`, `drop`, `swap` and others. Along with data terms, these are the main building blocks of any other function or program.

### Named Functions

Named functions can be defined by writing their identifiers, and then their function bodies (delimited by a `:` and a `;`). Recursion is supported as shown in the factorial example below.

#### Examples

##### **Cubing a Number**: `cube : copy copy * * ;`

`2 cube` → `8`

`5 cube` → `125`

##### **Fibonacci Sequence**: `fibonacci : hop hop + ;`

`5 10 fibonacci` → `5 10 15`

`1 2 fibonacci fibonacci fibonacci` → `1 2 3 5 8`

##### **Factorials**: `factorial : copy = 1 ( ) ( copy 1 - factorial * ) ? ;`

`5 factorial` → `120`

`20 factorial` → `2432902008176640000`

### Lambdas

Lambdas are data terms that represent functions stored on the stack as arguments for other functions. They are written as a function body delimited by parentheses. A lambda on top of the stack can be popped from the stack and evaluated with the `apply` function as shown below.

`8 ( copy * ) apply` → `64`

Lambdas can be composed into new lambdas using the `compose` function. Lambdas are composed simply by concatenating their function bodies. A lambda that adds 5 to a number and a lambda that doubles a number can be combined into one lambda that adds 5 to and then doubles a number.

`( 5 + ) ( 2 * )` → `( 5 + 2 * )`

## Try Compose

You can download and try Compose using Cargo.

`cargo install compositor`

The interactive environment can be started with the command `cmpstr`.
