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

In practice, this cipher is completely broken, as if Bob receives the ciphertext from Alice using the shift cipher, Bob just have to bruteforce all \\( a \\) possible cases of the key to find the plaintext (which is human-readable).

To make the cipher harder to attack, Bob can instead encrypt each letter in the alphabet with a different letter in the same alphabet. For example, Bob can replace `p` with `a`, `c` with `z`, etc. This idea derives a new cipher : ***simple substitution cipher***, that is, a cipher that each letter is replaced with another letter in the alphabet.

## Divisibility and greatest common divisors

## Modular arithmetics

## Prime numbers, unique factorization, and finite fields

## Powers and primitive roots in finite fields

## Cryptography before the computer age

## Symmetric and asymmetric ciphers



