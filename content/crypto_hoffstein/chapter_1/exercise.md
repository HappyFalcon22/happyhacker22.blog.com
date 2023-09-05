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
+ Most common bigrams in the ciphertext :

b. ***"A Botanical Code"***
+ Ciphertext : 

`KZRNK GJKIP ZBOOB XLCRG BXFAU GJBNG RIXRU XAFGJ BXRME MNKNG
BURIX KJRXR SBUER ISATB UIBNN RTBUM NBIGK EBIGR OCUBR GLUBN
JBGRL SJGLN GJBOR ISLRS BAFFO AZBUN RFAUS AGGBI NGLXM IAZRX
RMNVL GEANG CJRUE KISRM BOOAZ GLOKW FAUKI NGRIC BEBRI NJAWB
OBNNO ATBZJ KOBRC JKIRR NGBUE BRINK XKBAF QBROA LNMRG MALUF
BBG`

+ Frequency table : 
+ Most common bigrams in the ciphertext :

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
+ Most common bigrams in the ciphertext :

---

