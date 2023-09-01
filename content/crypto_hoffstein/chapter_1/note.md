+++
title = "Chapter 1 : Note"
date = 2023-08-31T12:45:22+07:00
author = "HappyHacker22"
draft = false
katex = true
color = "pink"
description = "Notes from Chapter 1 : `An Introduction to Cryptography`"
tag = ["notes", "hoffstein", "cryptography"]
enableEmoji = true
+++

# Chapter 1 : An Introduction to Cryptography

## Simple substitution ciphers

Firstly, I want to put some of the definitions here :
+ **plaintext** :
+ **ciphertext** :
+ **cryptography - cryptology** :
+ **cryptic** :

In this section, we explore the most fundamental cipher that almost anyone (including non-cryptographers) knows about : ***Caesar Cipher***, named in honor of ***Julius Caesar***. Basically, the cipher encrypts the *plaintext* by shifting each letter in it by a constant, unknown amount (we call this the *key*), the result of this action forms the *ciphertext*. In more mathematical terms, the ***Shift Cipher*** is the cipher : 
+ Has the alphabet : \\( A \\) of length \\( a \\)
+ A key : \\( 0 \leq k < A \\)
+ Plaintext : \\( m = m_1m_2m_3...m_n \\)
+ Ciphertext : \\( c = c_1c_2c_3...c_n \\)
+ Encryption : \\( c_i = m_i + k \pmod{a} \\)
+ Decryption : \\( m_i = c_i - k \pmod{a} \\)

The Caesar Cipher uses \\( k = 3 \\), his cipher named after him in honor.

Let's make an example : suppose we want to encrypt the message `Rules are made to be broken.` using the shift cipher with key \\( k = 15 \\) and the alphabet `abcdefghijklmnopqrstuvwxyz`. The result ciphertext is ``.

Note that we consider the ciphertext's letters to be in upper case while the plaintext's letters are in lower case for differentiation.

In practice, this cipher is completely broken, as if Bob receives the ciphertext from Alice using the shift cipher, Bob just have to bruteforce all \\( a \\) possible cases of the key to find the plaintext (which is human-readable).

To make the cipher harder to attack, Bob can instead encrypt each letter in the alphabet with a different letter in the same alphabet. For example, Bob can replace `p` with `a`, `c` with `z`, etc. This idea derives a new cipher : ***simple substitution cipher***, that is, a cipher that each letter is replaced with another letter in the alphabet. This cipher can be described as the rule of function :

\\[ \left{ a, b, c, d, e, f, g, h, ..., x, y, z \right. \longleftarrow \left{ A, B, C, D, E, F, G, H, ..., X, Y, Z \right. \\]

assigning each plaintext letter in the domain a different ciphertext letter in the range. This function can be described as a **subtitution table**, for example : 


| a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Y | A | D | F | Z | I | J | W | U | T | O | Q | C | B | L | X | Q | V | E | G | H | N | U | S | I | K |

Note that, a letter can be replaced by the same letter, it is still count as a substitution.

Why is it that doing this will make the cipher hard to break ? Statistically, there are total \\( 26! \\) ways of creating a substitution rule. Hence, Bob will have to bruteforce waaaaay to much cases to find the correct rule ! (Readers can verify just how much \\(26!\\) is).

But, in practice, this cipher is still breakable (easily, in fact). Let's look at the small subsection.

### Cryptanalysis of simple substitution ciphers

In definition, **cryptanalysis** means the art of decrypting the ciphertext without any knowledge about the encryption key of a cipher.

Now, what makes the substitution cipher breakable is that *the plaintext is human-readable* (yes, as we see in later chapters, the plaintext can be unmeaningful to be readable). Hence, there is an invariant ***letter frequency*** from each language (English, Turkey, Thailand, Vietnamese, etc). Just take a random \\( 100000 \\) letters' paragraph and calculate the ratio of each letter in that paragraph. For example, in English, the letter frequency table is shown here :

| *Letter* | *Frequency (%)* | *Letter* | *Frequency (%)* |
|:--------:|:---------------:|:--------:|:---------------:|
|     a    |       8.15      |     n    |       7.10      |
|     b    |       1.44      |     o    |       8.00      |
|     c    |       2.76      |     p    |       1.98      |
|     d    |       3.79      |     q    |       0.12      |
|     e    |      13.11      |     r    |       6.83      |
|     f    |       2.92      |     s    |       6.10      |
|     g    |       1.99      |     t    |      10.47      |
|     h    |       5.26      |     u    |       2.46      |
|     i    |       6.35      |     v    |       0.92      |
|     j    |       0.13      |     w    |       1.54      |
|     k    |       0.42      |     x    |       0.17      |
|     l    |       3.39      |     y    |       1.98      |
|     m    |       2.54      |     z    |       0.08      |



## Divisibility and greatest common divisors

## Modular arithmetics

## Prime numbers, unique factorization, and finite fields

## Powers and primitive roots in finite fields

## Cryptography before the computer age

## Symmetric and asymmetric ciphers



