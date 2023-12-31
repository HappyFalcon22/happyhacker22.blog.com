+++
title = "Chapter 1 : Exercise"
date = 2023-08-31T12:43:10+07:00
author = "HappyHacker22"
draft = false
katex = true
color = "blue"
description = "Solutions for exercise Chapter 1 : `An Introduction to Cryptography`"
tag = ["cryptography", "hoffstein", "exercise"]
enableEmoji = true
+++

I suggest reading the Chapter 1 material or you can have a look of the note [here](./../note/)

Let's begin

## 1.1 
---
a. Encrypt the following plaintext using a rotation of 11 clockwise : 
<span> `“A page of history is worth a volume of logic.”` </span>
   
b. Decrypt the following message, which was encrypted with a rotation of 7 clockwise :
<span> `AOLYLHYLUVZLJYLAZILAALYAOHUAOLZLJYLALZAOHALCLYFIVKFNBLZZLZ` </span>
   
c. Decrypt the following message, which was encrypted by rotating 1 clockwise for the first letter, then 2 clockwise for the second letter, etc
<span> `XJHRFTNZHMZGAHIUETXZJNBWNUTRHEPOMDNBJMAUGORFAOIZOCC` </span>  

---

The original purpose of this exeercise is to build a cipher wheel and use it to encrypt-decrypt texts. But I decided to use programming language to solve, which is more practical.

Firstly, let's build the function to encrypt the plaintext using the shift cipher :

```Python
ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
def shift_encrypt(m: str, key: int):
    c = ""
    for x in m:
        if x not in ALPHABET and x not in ALPHABET.lower():
            c += x
        elif x in ALPHABET.lower():
            c += ALPHABET[(ALPHABET.index(x.upper()) + key) % len(ALPHABET)].lower()
        else:
            c += ALPHABET[(ALPHABET.index(x) + key) % len(ALPHABET)]
        print(c)
    return c
```

Here, the `ALPHABET` is not necessary to have only letters, it can contains any character. In this textbook's context, we will work with the standard alphabet (26 English letters). Additionally, I modified the code so it can encrypt the plaintext without changing the non-letter char in the plaintext or changing the case of the plaintext's letter.

So when we encrypt the plaintext : 

`“A page of history is worth a volume of logic.”`

, we get the ciphertext : 

`“L alrp zq stdezcj td hzces l gzwfxp zq wzrtn.”`

Now we will build the decryption method : 

```Python
def shift_decrypt(c: str, key: int):
    m = ""
    for x in c:
        if x not in ALPHABET and x not in ALPHABET.lower():
            m += x
        elif x in ALPHABET.lower():
            m += ALPHABET[(ALPHABET.index(x.upper()) - key) % len(ALPHABET)].lower()
        else:
            m += ALPHABET[(ALPHABET.index(x) - key) % len(ALPHABET)]
    return m
```

Using the function with the ciphertext and the key `11`, we achived this : 

`therearenosecretsbetterthanthesecretesthateverybodyguesses`

A little spacing reveals :

`there are no secrets better than the secretes that everybody guesses`

The last one is a bit tricky, but this kind of encryption doesn't need the key for decryption, since we know the shift amount of all characters in the plaintext when encrypted to ciphertext :

```Python
def shift_decrypt_increment(c: str):
    m = ""
    k = 1
    for x in c:
        m += ALPHABET[(ALPHABET.index(x) - k) % len(ALPHABET)]
        k += 1
    return m.lower() # To emphasize that this is the plaintext
```

Since the ciphertext is all in uppercase, so I will ease out my code a bit.
So, the result is

`whenangrycounttenbeforeyouspeakifveryangryanhundred`

A little spacing reveals :

`when angry count ten before you speak if very angry an hundred`

## 1.2 
---

Decrypt each of the following Caesar encryptions by trying the various possible
shifts until you obtain readable text :

a. 

`LWKLQNWKDWLVKDOOQHYHUVHHDELOOERDUGORYHOBDVDWUHH`

b. 

`UXENRBWXCUXENFQRLQJUCNABFQNWRCJUCNAJCRXWORWMB`

c. 

`BGUTBMBGZTFHNLXMKTIPBMAVAXXLXTEPTRLEXTOXKHHFYHKMAXFHNLX`

---

Since there are only \\(26\\) cases of the key, we can just bruteforce all cases to find the readable message. Of course, before the computer age, all cryptographers must do by hand, which consumes too much time !! I will just do it on Python

```Python
def caesar_bruteforce(c: str):
    for k in range(len(ALPHABET)):
        print(shift_decrypt(c, k).lower(), k)
```

The result is :

a. `i think that i shall never see a billboard lovely as a tree` with the key \\(k = 3\\)

b. `love is not love which alters when it alteration finds` with the key \\(k = 9\\)

c. `in baiting a mouse trap with cheese always leave room for the mouse` with the key \\(k = 19\\)

## 1.3
---

Use this substitution table :

| a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| S | C | J | A | X | U | F | B | Q | K | T | P | R | W | E | Z | H | V | L | I | G | Y | D | N | M | O |

a. Encrypt the message : `The gold is hidden in the garden.`

b. Make the decryption table from this substitution table.

c. Use the decryption table to decrypt the ciphertext : `IBXLX JVXIZ SLLDE VAQLL DEVAU QLB`

---

Let's make the encryption - decryption substitution cipher in Python before solving this exercise : 

```Python
def encrypt_substitution(m: str, alp_m: str, alp_c: str):
    c = ""
    for x in m:
        if x not in alp_m:
            c += x
        else:
            c += alp_c[alp_m.index(x)]
    return c
```

Example usage : 

```Python
alp_m = "abcdefghijklmnopqrstuvwxyz"
alp_c = "SCJAXUFBQKTPRWEZHVLIGYDNMO"
m = "The gold is hidden in the garden.".lower()
print(encrypt_substitution(m, alp_m, alp_c))
```

a. The ciphertext is : `IBX FEPA QL BQAAXW QW IBX FSVAXW.`

b. The decryption table : 

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| d | h | b | w | o | g | u | q | t | c | j | s | y | x | z | l | i | m | a | k | f | r | n | e | v | p |

c. The plaintext using the decryption table from b. is : `the secret password is swordfish`


# 1.4

---

Each of the following messages has been encrypted using a simple substitution
cipher. Decrypt them. For your convenience, we have given you a frequency table and a list of the most common bigrams that appear in the ciphertext.

a. ***"A Piratical Treasure"***
+ Ciphertext : 

`JNRZR BNIGI BJRGZ IZLQR OTDNJ GRIHT USDKR ZZWLG OIBTM NRGJN
IJTZJ LZISJ NRSBL QVRSI ORIQT QDEKJ JNRQW GLOFN IJTZX QLFQL
WBIMJ ITQXT HHTBL KUHQL JZKMM LZRNT OBIMI EURLW BLQZJ GKBJT
QDIQS LWJNR OLGRI EZJGK ZRBGS MJLDG IMNZT OIHRK MOSOT QHIJL
QBRJN IJJNT ZFIZL WIZTO MURZM RBTRZ ZKBNN LFRVR GIZFL KUHIM
MRIGJ LJNRB GKHRT QJRUU RBJLW JNRZI TULGI EZLUK JRUST QZLUK
EURFT JNLKJ JNRXR S`

+ Frequency table : 

| R | J | I | L | Z | T | N | Q | B | G | K | U | M | O | S | H | W | F | E | D | X | V |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|33 |30 |27 |25 |24 |20 |19 |16 |15 |15 |13 |12 |12 |10 | 9 | 8 | 7 | 6 | 5 | 5 | 3 | 2 |

+ Most common bigrams in the ciphertext : `JN` (11 times), `NR` (8 times), `TQ` (6 times), and `LW`, `RB`, `RZ`, and `JL` (5 times each)

b. ***"A Botanical Code"***
+ Ciphertext : 

`KZRNK GJKIP ZBOOB XLCRG BXFAU GJBNG RIXRU XAFGJ BXRME MNKNG
BURIX KJRXR SBUER ISATB UIBNN RTBUM NBIGK EBIGR OCUBR GLUBN
JBGRL SJGLN GJBOR ISLRS BAFFO AZBUN RFAUS AGGBI NGLXM IAZRX
RMNVL GEANG CJRUE KISRM BOOAZ GLOKW FAUKI NGRIC BEBRI NJAWB
OBNNO ATBZJ KOBRC JKIRR NGBUE BRINK XKBAF QBROA LNMRG MALUF
BBG`

+ Frequency table : 

| B | R | G | N | A | I | U | K | O | J | L | X | M | F | S | E | Z | C | T | W | P | V | Q |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|32 |28 |22 |20 |16 |16 |14 |13 |12 |11 |10 |10 | 8 | 8 | 7 | 7 | 6 | 5 | 3 | 2 | 1 | 1 | 1 |


+ Most common bigrams in the ciphertext : `NG` and `RI` (7 times each), `BU` (6 times), and `BR` (5 times)

c. ***“A Brilliant Detective”*** (this ciphertext does not contain the word `the`)
+ Ciphertext :

`GSZES GNUBE SZGUG SNKGX CSUUE QNZOQ EOVJN VXKNG XGAHS AWSZZ
BOVUE SIXCQ NQESX NGEUG AHZQA QHNSP CIPQA OIDLV JXGAK CGJCG
SASUB FVQAV CIAWN VWOVP SNSXV JGPCV NODIX GJQAE VOOXC SXXCG
OGOVA XGNVU BAVKX QZVQD LVJXQ EXCQO VKCQG AMVAX VWXCG OOBOX
VZCSO SPPSN VAXUB DVVAX QJQAJ VSUXC SXXCV OVJCS NSJXV NOJQA
MVBSZ VOOSH VSAWX QHGMV GWVSX CSXXC VBSNV ZVNVN SAWQZ ORVXJ
CVOQE JCGUW NVA`

+ Frequency table : 

| V | S | X | G | A | O | Q | C | N | J | U | Z | E | W | B | P | I | H | K | D | M | L | R | F |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|39 |29 |29 |22 |21 |21 |20 |20 |19 |13 |11 |11 |10 | 8 | 8 | 6 | 5 | 5 | 5 | 4 | 3 | 2 | 1 | 1 |

+ Most common bigrams in the ciphertext : `XC` (10 times), `NV` (7 times), and `CS, OV, QA` and `SX` (6 times each)

---

I will just put down the results here : 

a. Plaintext : 

`these characters as one might readily guess form a cipher that 
is to say they convey a meaning but then from what is known of
captain kiddi could not suppose him capable of constructing
any of the more abstruse cryptographs i made up my mind at once
that this was of a simple species such however as would appear
to the crude intellect of the sailor absolutely
insoluble without the key`

b. Plaintext :

`i was i think well educated for the standard of the day my sister
and i had a german governess a very sentimental creature
she taught us the language of flowers a forgotten study nowadays
but most charming a yellow tulip for instance means hopeless
love while a china aster means i die of jealousy at your feet`

c. Plaintext :

`i am fairly familiar with all forms of secret writing and am 
myself author of a trifling monograph upon subject in which i
analyze one hundred separate ciphers but i confess that this
is entirely new to me object of those who invented this system
has apparently been to conceal that these characters convey
a message and to give idea that they are mere random sketches
of children`

## 1.5

---

Suppose that you have an alphabet of 26 letters.

a. How many possible simple substitution ciphers are there?

b. A letter in the alphabet is said to be **fixed** if the encryption of the letter is the letter itself. How many simple substitution ciphers are there that leave:

+ no letters fixed?
+ at least one letter fixed?
+ exactly one letter fixed?
+ at least two letters fixed?

---

## 1.6

---

Let \\(a, b, c, \in \mathbb{Z}\\). Use the definition of divisibility to directly prove the following properties of divisibility. (This is Proposition 1.4.)

a. If \\(a | b\\) and \\(b | c\\), then \\(a | c\\)

b. If \\(a | b\\) and \\(b | a\\), then \\(a = \pm b\\)

c. If \\(a | b\\) and \\(a | c\\), then \\(a | (b + c)\\) and \\(a | (b - c)\\)

---

Proving these will be easy :

a. Suppose \\(a | b\\) and \\(b | c\\), then :
\\[ b = ax \\;\\;;\\; c = by \\]
\\[ \longrightarrow c = a(xy) \\]
\\[ \longrightarrow a | c \\]

## 1.7

---

Use a calculator and the method described in Remark 1.9 to compute the following quotients and remainders :

a. \\(34787\\) divided by \\(353\\)

b. \\(238792\\) divided by \\(7843\\)

c. \\(9829387493\\) divided by \\(873485\\)

d. \\(1498387487\\) divided by \\(76348\\)

---

## 1.8

---

Use a calculator and the method described in Remark 1.9 to compute the following remainders, without bothering to compute the associated quotients :

a. The remainder of \\(78745\\) divided by \\(127\\)

b. The remainder of \\(2837647\\) divided by \\(4387\\)

c. The remainder of \\(8739287463\\) divided by \\(18754\\)

d. The remainder of \\(4536782793\\) divided by \\(9784537\\)

---

## 1.9

---

Use the Euclidean algorithm to compute the following greatest common divisors :

a. \\(gcd(291, 252)\\)

b. \\(gcd(16261, 85652)\\)

c. \\(gcd(139024789, 93278890)\\)

d. \\(gcd(16534528044, 8332745927)\\)

---

## 1.10

---

For each of the \\(gcd(a, b)\\) values in Exercise 1.9, use the extended Euclidean algorithm (Theorem 1.11) to find integers \\(u\\) and \\(v\\) such that \\(au + bv = gcd(a, b)\\).

---

## 1.11

---

Let \\(a\\) and \\(b\\) be positive integers.

a. Suppose that there are integers \\(u\\) and \\(v\\) satisfying \\(au + bv = 1\\). Prove that \\(gcd(a, b) = 1\\)

b. Suppose that there are integers \\(u\\) and \\(v\\) satisfying \\(au + bv = 6\\). Is it necessarily true that \\(gcd(a, b) = 6\\)? If not, give a specific counterexample, and describe in general all of the possible values of \\(gcd(a, b)\\)

c. Suppose that \\((u_1, v_1)\\) and \\((u_2, v_2)\\) are two solutions in integers to the equation \\(au + bv = 1\\). Prove that a divides \\(v_2 − v_1\\) and that b divides \\(u_2 − u_1\\)

d. More generally, let \\(g = gcd(a, b)\\) and let \\((u_0, v_0)\\) be a solution in integers to \\(au + bv = g\\). Prove that every other solution has the form \\(u = u_0 + \dfrac{kb}{g}\\) and \\(v = v_0 − \dfrac{ka}{g}\\) for some integer \\(k\\). (This is the second part of Theorem 1.11.)

---