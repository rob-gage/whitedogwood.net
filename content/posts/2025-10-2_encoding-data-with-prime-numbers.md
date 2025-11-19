+++
title = "Encoding Data with Prime Numbers"
date = 2025-10-02
description = "A method of encoding data using the fundamental theorem of arithmetic"

[taxonomies]
tags = ["Haskell", "Mathematics"]
+++

## The Fundamental Theorem of Arithmetic

Every integer greater than 1 has one unique prime factorization, if it is not a prime number itself. The uniqueness of each number's prime factorization allows prime numbers to be used as the
representation of data or symbols. If different primes are assigned to different symbols, then multiplying them together yields a unique numerical encoding. By factoring the code number back into primes, we can recover the symbols, making primes a perfect method for encoding data numerically.

## Representing Ordered Symbols

If the word ***DOG*** is simply represented as `2 x 3 x 5`, it will result in the encoding of `30`, which has a prime factorization that can be read as `5 x 3 x 2`, leading to potential misconceptions about the divinity of canines. However, if we tie each prime to a unique position in the sequence instead, and apply exponents to them, associated with the symbols in those positions, the sequence order is preserved.

The following steps must be followed to encode ordered data:

1. Assign each position in the sequence a unique prime number:
	+ 1st position → 2
	+ 2nd position → 3
	+ 3rd position → 5

2. Assign an exponent to each symbol. These do not have to be prime:
	+ **D** → **1**
	+ **O** → **2**
	+ **G** → **3**

3. Use the exponent associated with each symbol to exponentiate the prime number associated with its position in the sequence, and then take the product of all the exponentiated primes:

	`2¹ x 3² x 5³ = 2250`

4. Decode the number by computing its prime factorization, and use the exponent of each unique prime factor to determine which symbol it corresponds to:
	+ exponent of 2: **1** → **D**
	+ exponent of 3: **2** → **O**
	+ exponent of 5: **3** → **G**

**2250** has prime factorization `2 x 3 x 3 x 5 x 5 x 5` allowing the letter at any given position to be known. This can be distinguished from `2 x 2 x 2 x 3 x 3 x 5` to prevent the order of symbols being confused.

## Writing a Program to Encode Words

Lets implement this encoding method in Haskell.

First we will create a data type representing the symbols (in this case, letters in the Hebrew alefbet).

```Haskell
data Symbol
    = Alef | Bet | Gimel | Dalet | He | Vav
    | Zayin | Het | Tet | Yod | Kaf | Lamed
    | Mem | Nun | Samekh | Ayin | Pe | Tsadi
    | Qof | Resh | Shin | Tav
    deriving (Enum, Show)
```

We will also need a function to convert a `Symbol` to its corresponding exponent. It must increment the default index provided by the `Enum` typeclass, since `0` cannot be used as an exponent in the encoding.

```Haskell
exponentsFromSymbols :: [Symbol] -> [Integer]
exponentsFromSymbols = map (toInteger . (+ 1) . fromEnum)
```

We will also need a function to generate an infinite list of primes to use for encoding words of any length. This can be done easily in Haskell using a common algorithm known ([somewhat incorrectly](https://www.cs.hmc.edu/~oneill/papers/Sieve-JFP.pdf)) as the Sieve of Eratosthanes.

```Haskell
primes :: [Integer]
primes = sieve [2..] where
    sieve [] = []
    sieve (first:rest) = first : sieve (filter (\x -> x `mod` first /= 0) rest)
```

We can now write the function to encode a list of symbols as a unique `Integer`.

```Haskell
encode :: [Symbol] -> Integer
encode symbols =
    product $
    map (\(prime, exponent) -> prime ^ exponent) $
    zip primes (exponentsFromSymbols symbols)
```

![Sequenced Hebrew letters being encoded as numbers](/gifs/prime-encoding.gif)
