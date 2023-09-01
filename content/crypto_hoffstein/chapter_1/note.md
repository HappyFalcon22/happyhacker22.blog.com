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

In this section, we explore the most fundamental cipher that almost anyone (including non-cryptographers) knows about : ***Caesar Cipher***, named in honor of ***Julius Caesar***. Basically, the cipher encrypts the *plaintext* by shifting each letter in it by a constant, unknown amount (we call this the *key*), the result of this action forms the *ciphertext*. In more mathematical terms, the ***Shift Cipher*** is the cipher : 
+ Has the alphabet : \\( A \\) of length \\( a \\)
+ A key : \\( 0 \leq k < A \\)
+ Plaintext : \\( m = m_1m_2m_3...m_n \\)
+ Ciphertext : \\( c = c_1c_2c_3...c_n \\)
+ Encryption : \\( c_i = m_i + k \pmod{a} \\)
+ Decryption : \\( m_i = c_i - k \pmod{a} \\)

The Caesar Cipher uses \\( k = 3 \\), his cipher named after him in honor.

Let's make an example : suppose we want to encrypt the message `Rules are made to be broken.` using the shift cipher with key \\( k = 15 \\) and the alphabet `abcdefghijklmnopqrstuvwxyz`. The result ciphertext is ``.


## Divisibility and greatest common divisors

## Modular arithmetics

## Prime numbers, unique factorization, and finite fields

## Powers and primitive roots in finite fields

## Cryptography before the computer age

## Symmetric and asymmetric ciphers



